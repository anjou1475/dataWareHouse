### 一、什么是数据倾斜

对 Spark/Hadoop 这样的分布式大数据系统来讲，数据量大并不可怕，可怕的是数据倾斜。

对于分布式系统而言，理想情况下，随着系统规模（节点数量）的增加，应用整体耗时线性下降。如果一台机器处理一批大量数据需要120分钟，当机器数量增加到3台时，理想的耗时为120 / 3 = 40分钟。但是，想做到分布式情况下每台机器执行时间是单机时的1 / N，就必须保证每台机器的任务量相等。不幸的是，很多时候，任务的分配是不均匀的，甚至不均匀到大部分任务被分配到个别机器上，其它大部分机器所分配的任务量只占总的小部分。比如一台机器负责处理 80% 的任务，另外两台机器各处理 10% 的任务。

『不患多而患不均』，这是分布式环境下最大的问题。意味着计算能力不是线性扩展的，而是存在短板效应: 一个 Stage 所耗费的时间，是由最慢的那个 Task 决定。

由于同一个 Stage 内的所有 task 执行相同的计算，在排除不同计算节点计算能力差异的前提下，不同 task 之间耗时的差异主要由该 task 所处理的数据量决定。所以，要想发挥分布式系统并行计算的优势，就必须解决数据倾斜问题。

### 二、数据倾斜的危害

当出现数据倾斜时，小量任务耗时远高于其它任务，从而使得整体耗时过大，未能充分发挥分布式系统的并行计算优势。　　

另外，当发生数据倾斜时，部分任务处理的数据量过大，可能造成内存不足使得任务失败，并进而引进整个应用失败。　　

### 三、数据倾斜的现象

当发现如下现象时，十有八九是发生数据倾斜了:

- 绝大多数 task 执行得都非常快，但个别 task 执行极慢，整体任务卡在某个阶段不能结束。
- 原本能够正常执行的 Spark 作业，某天突然报出 OOM（内存溢出）异常，观察异常栈，是我们写的业务代码造成的。这种情况比较少见。

> TIPS

在 Spark streaming 程序中，数据倾斜更容易出现，特别是在程序中包含一些类似 sql 的 join、group 这种操作的时候。因为 Spark Streaming 程序在运行的时候，我们一般不会分配特别多的内存，因此一旦在这个过程中出现一些数据倾斜，就十分容易造成 OOM。

### 四、数据倾斜的原因

在进行 shuffle 的时候，必须将各个节点上相同的 key 拉取到某个节点上的一个 task 来进行处理，比如按照 key 进行聚合或 join 等操作。此时如果某个 key 对应的数据量特别大的话，就会发生数据倾斜。比如大部分 key 对应10条数据，但是个别 key 却对应了100万条数据，那么大部分 task 可能就只会分配到10条数据，然后1秒钟就运行完了；但是个别 task 可能分配到了100万数据，要运行一两个小时。

因此出现数据倾斜的时候，Spark 作业看起来会运行得非常缓慢，甚至可能因为某个 task 处理的数据量过大导致内存溢出。

### 五、问题发现与定位

1、通过 Spark Web UI

通过 Spark Web UI 来查看当前运行的 stage 各个 task 分配的数据量（Shuffle Read Size/Records），从而进一步确定是不是 task 分配的数据不均匀导致了数据倾斜。

知道数据倾斜发生在哪一个 stage 之后，接着我们就需要根据 stage 划分原理，推算出来发生倾斜的那个 stage 对应代码中的哪一部分，这部分代码中肯定会有一个 shuffle 类算子。可以通过 countByKey 查看各个 key 的分布。

> TIPS

数据倾斜只会发生在 shuffle 过程中。这里给大家罗列一些常用的并且可能会触发 shuffle 操作的算子: distinct、groupByKey、reduceByKey、aggregateByKey、join、cogroup、repartition 等。出现数据倾斜时，可能就是你的代码中使用了这些算子中的某一个所导致的。

2、通过 key 统计

也可以通过抽样统计 key 的出现次数验证。

由于数据量巨大，可以采用抽样的方式，对数据进行抽样，统计出现的次数，根据出现次数大小排序取出前几个:

```
df.select("key").sample(false, 0.1)           // 数据采样    
.map(k => (k, 1)).reduceBykey(_ + _)         // 统计 key 出现的次数    
.map(k => (k._2, k._1)).sortByKey(false)  // 根据 key 出现次数进行排序    
.take(10)                                 // 取前 10 个。
```

（滑动可查看）

如果发现多数数据分布都较为平均，而个别数据比其他数据大上若干个数量级，则说明发生了数据倾斜。

### 六、如何缓解数据倾斜

#### 思路1. 过滤异常数据

如果导致数据倾斜的 key 是异常数据，那么简单的过滤掉就可以了。

首先要对 key 进行分析，判断是哪些 key 造成数据倾斜。具体方法上面已经介绍过了，这里不赘述。

然后对这些 key 对应的记录进行分析:

1. 空值或者异常值之类的，大多是这个原因引起
2. 无效数据，大量重复的测试数据或是对结果影响不大的有效数据
3. 有效数据，业务导致的正常数据分布

解决方案

对于第 1，2 种情况，直接对数据进行过滤即可。

第3种情况则需要特殊的处理，具体我们下面详细介绍。

#### 思路2. 提高 shuffle 并行度

Spark 在做 Shuffle 时，默认使用 HashPartitioner（非 Hash Shuffle）对数据进行分区。如果并行度设置的不合适，可能造成大量不相同的 Key 对应的数据被分配到了同一个 Task 上，造成该 Task 所处理的数据远大于其它 Task，从而造成数据倾斜。

如果调整 Shuffle 时的并行度，使得原本被分配到同一 Task 的不同 Key 发配到不同 Task 上处理，则可降低原 Task 所需处理的数据量，从而缓解数据倾斜问题造成的短板效应。

（1）操作流程

RDD 操作 可在需要 Shuffle 的操作算子上直接设置并行度或者使用 spark.default.parallelism 设置。如果是 Spark SQL，还可通过 SET spark.sql.shuffle.partitions=[num_tasks] 设置并行度。默认参数由不同的 Cluster Manager 控制。

dataFrame 和 sparkSql 可以设置 spark.sql.shuffle.partitions=[num_tasks] 参数控制 shuffle 的并发度，默认为200。

（2）适用场景

大量不同的 Key 被分配到了相同的 Task 造成该 Task 数据量过大。

（3）解决方案

调整并行度。一般是增大并行度，但有时如减小并行度也可达到效果。

（4）优势

实现简单，只需要参数调优。可用最小的代价解决问题。一般如果出现数据倾斜，都可以通过这种方法先试验几次，如果问题未解决，再尝试其它方法。

（5）劣势

适用场景少，只是让每个 task 执行更少的不同的key。无法解决个别key特别大的情况造成的倾斜，如果某些 key 的大小非常大，即使一个 task 单独执行它，也会受到数据倾斜的困扰。并且该方法一般只能缓解数据倾斜，没有彻底消除问题。从实践经验来看，其效果一般。

> TIPS 可以把数据倾斜类比为 hash 冲突。提高并行度就类似于 提高 hash 表的大小。



#### 思路3. Reduce 端 Join 转化为 Map 端 Join

通过 Spark 的 Broadcast 机制，将 Reduce 端 Join 转化为 Map 端 Join，这意味着 Spark 现在不需要跨节点做 shuffle 而是直接通过本地文件进行 join，从而完全消除 Shuffle 带来的数据倾斜。

![图片](https://mmbiz.qpic.cn/mmbiz_png/QDdcCFX9vIclXpCz4PEh67vX2sARylWpDjQSEf0qiaafmQiaDVbFMB7icTcVEqGSrdfr3Pk4muFbdDvqKxopJTtYA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
from pyspark.sql.functions import broadcastresult = broadcast(A).join(B, ["join_col"], "left")
```

（滑动可查看）

其中 A 是比较小的 dataframe 并且能够整个存放在 executor 内存中。

（1）适用场景

参与Join的一边数据集足够小，可被加载进 Driver 并通过 Broadcast 方法广播到各个 Executor 中。

（2）解决方案

在 Java/Scala 代码中将小数据集数据拉取到 Driver，然后通过 Broadcast 方案将小数据集的数据广播到各 Executor。或者在使用 SQL 前，将 Broadcast 的阈值调整得足够大，从而使 Broadcast 生效。进而将 Reduce Join 替换为 Map Join。

（3）优势

避免了 Shuffle，彻底消除了数据倾斜产生的条件，可极大提升性能。

（4）劣势

因为是先将小数据通过 Broadcase 发送到每个 executor 上，所以需要参与 Join 的一方数据集足够小，并且主要适用于 Join 的场景，不适合聚合的场景，适用条件有限。

> NOTES

使用Spark SQL时需要通过 SET spark.sql.autoBroadcastJoinThreshold=104857600 将 Broadcast 的阈值设置得足够大，才会生效。



#### 思路4. map 端先局部聚合

在 map 端加个 combiner 函数进行局部聚合。加上 combiner 相当于提前进行 reduce ,就会把一个 mapper 中的相同 key 进行聚合，减少 shuffle 过程中数据量 以及 reduce 端的计算量。这种方法可以有效的缓解数据倾斜问题，但是如果导致数据倾斜的 key 大量分布在不同的 mapper 的时候，这种方法就不是很有效了。

> TIPS 使用 reduceByKey 而不是 groupByKey。
>
> ```scala
> val words = Array("one", "two", "two", "three", "three", "three")  
>   
> val wordPairsRDD = sc.parallelize(words).map(word => (word, 1))  
>   
> val wordCountsWithReduce = wordPairsRDD.reduceByKey(_ + _)  
>   
> val wordCountsWithGroup = wordPairsRDD.groupByKey().map(t => (t._1, t._2.sum)) 
> ```
>
> ![img](https://img-blog.csdn.net/20151121161604854?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
>
> ![img](https://img-blog.csdn.net/20151121162115121?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

#### 思路5. 加盐局部聚合 + 去盐全局聚合

这个方案的核心实现思路就是进行两阶段聚合。第一次是局部聚合，先给每个 key 都打上一个 1~n 的随机数，比如 3 以内的随机数，此时原先一样的 key 就变成不一样的了，比如 (hello, 1) (hello, 1) (hello, 1) (hello, 1) (hello, 1)，就会变成 (1_hello, 1) (3_hello, 1) (2_hello, 1) (1_hello, 1) (2_hello, 1)。接着对打上随机数后的数据，执行 reduceByKey 等聚合操作，进行局部聚合，那么局部聚合结果，就会变成了 (1_hello, 2) (2_hello, 2) (3_hello, 1)。然后将各个 key 的前缀给去掉，就会变成 (hello, 2) (hello, 2) (hello, 1)，再次进行全局聚合操作，就可以得到最终结果了，比如 (hello, 5)。

```
def antiSkew(): RDD[(String, Int)] = {   
val SPLIT = "-"   
val prefix = new Random().nextInt(10)    
pairs.map(t => ( prefix + SPLIT + t._1, 1))        
.reduceByKey((v1, v2) => v1 + v2)        
.map(t => (t._1.split(SPLIT)(1), t2._2))        
.reduceByKey((v1, v2) => v1 + v2)}
```

不过进行两次 mapreduce，性能稍微比一次的差些。