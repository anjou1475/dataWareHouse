# dataWareHouse
数据仓库随想






一 数据仓库概述
1.1 数据仓库的定义


 William H.Inmon（比尔·恩门）：数据仓库(Data Warehouse)是一个面向主题的(Subject Oriented)、集成的(Integrated)、相对稳定的(Non-Volatile)、反映历史变化的(Time Variant)数据集合，用于支持管理决策(Decision Making Support)和信息的全局共享(Global Sharing of Information)。其主要功能是将组织透过资讯系统之联机事务处理(OLTP)经年累月所累积的大量资料，透过数据仓库理论所特有的资料储存架构，作一有系统的分析整理，以利各种分析方法如联机分析处理(OLAP)、数据挖掘(Data Mining)之进行，并进而支持如决策支持系统(DSS)、主管资讯系统(EIS)之创建，帮助决策者能快速有效的自大量资料中，分析出有价值的资讯，以利决策拟定及快速回应外在环境变动，帮助建构商业智能(BI)。
1.2 数据仓库的用途
整合公司所有业务数据，建立统一的数据中心
产生业务报表，用于作出决策
为网站运营提供运营上的数据支持
可以作为各个业务的数据源，形成业务数据互相反馈的良性循环
分析用户行为数据，通过数据挖掘来降低投入成本，提高投入效果
开发数据产品，直接或间接地为公司盈利


1.3 数据仓库的四大特征
1.数据仓库的数据是面向主题的
2.数据仓库的数据是集成的
3.数据仓库的数据是非易失的
4.数据仓库的数据是随时间不断变化的
1.3.1 面向主题
主题（Subject）：特定的数据分析领域与目标。
主题是根据分析的要求来确定的。这与按照数据处理或应用的要求来组织数据是不同的。如在生产企业中，同样是材料供应，在操作型数据库系统中，人们所关心的是怎样更方便和更快捷地进行材料供应的业务处理；而在进行分析处理时，人们就应该关心材料的不同采购渠道和材料供应是否及时，以及材料质量状况等。
数据仓库是面向分析、决策人员的主观要求的，不同的用户有不同的要求，同一个用户的要求也会随时间而经常变化，因此，数据仓库中的主题有时会因用户主观要求的变化而变化的。
面向主题划分如下：
数据仓库面向在数据模型中已经定义好的公司的主要主题领域。典型的主题领域包括顾客、产品、订单和财务或是其他某项事务或活动。

基本主题：
教育机构：学生、讲师、班主任、课程等
电商行业：运营、流量、价值、商品、市场、风控、销售等
传统行业：供应商、商品、客户、仓库等
主题域
主题域通常是联系较为紧密的数据主题的集合。可以根据业务的关注点，将这些数据主题划分到不同的主题域。主题域的确定必须由最终用户和数据仓库的设计人员共同完成。

主题边界的划分应该按照以下规则来进行定义划分。 
首先数据仓库中逻辑模型根据业务划分为多个主题域,主题域下面会涉及具体的实体表,维表以及关系实体,这些划分可以按照下面规则来进行划分。 
a:每个主题域包含一个主要业务概念; 
b:每个主题域包含一个主要交易业务概念,用一个或几个核心实体来表述。 
c:主题域与主题域之间的核心实体不能重叠,核心实体间的关系实体则可以出现在两个主题域内; 
d:每个主题域中包含几个关键的核心实体,且这几个核心实体间具有直接的关联关系。  

主题域的另一种定义是：对某个主题进行分析后确定的主题的边界。分析主题域，确定要装载到数据仓库的主题是信息打包技术的第一步。而在进行数据仓库设计时，一般是一次先建立一个主题或企业全部主题中的一部分，因此在大多数数据仓库的设计过程中都有一个主题域的选择过程。主题域的确定必须由最终用户和数据仓库的设计人员共同完成。
1.3.2 集成
•集成性是指数据仓库中数据必须是一致的。数据仓库的数据是从原有的分散的多个数据库、数据文件和数据段中抽取来的，数据来源可能既有内部数据又有外部数据。
•数据仓库中的数据是为分析服务的，而分析需要多种广泛的不同数据源以便进行比较、鉴别，因此数据仓库中的数据必须从多个数据源中获取，这些数据源包括多种类型数据库、文件系统以及Internet网上数据等，它们通过数据集成而形成数据仓库中的数据。
 集成的方法：
–统一：消除不一致的现象
–综合：对原有数据进行综合和计算
 集成需要考虑的问题：
–数据格式
–计量单位
–数据代码含义混乱
–数据名称混乱


1.3.3 非易失
•数据仓库中的数据是经过抽取而形成的分析型数据，不具有原始性，主要供企业决策分析之用，执行的主要是‘查询’操作，一般情况下不执行‘更新’操作。同时，一个稳定的数据环境也有利于数据分析操作和决策的制订。
•面向应用的事务数据库需要对数据进行频繁的插入、更新操作，而对于数据仓库中数据的操作仅限于数据的初始导入和记录查询。
1.3.4 随时间不断变化
•数据仓库以维的形式对数据进行组织，时间维是数据仓库中很重要的一个维度。并且数据仓库中的数据时间跨度大，从几年甚至到几十年，称为历史数据。
•数据仓库中的数据必须以一定时间段为单位进行统一更新。
•数据变化方式:
 –不断增加新的数据内容
 –不断删去旧的数据内容
 –更新与时间有关的综合数据
数据的生命周期与行业、自己本身的需求有关，比如金融业“在设计银行数据保存周期策略时，最常用的经验法则是7年和13个月规则”
基础数据区里面通过历史表（拉链表）来保存重要信息的历史数据，一般客户类、账户类等信息要保留7年，交易类流水类信息要保留至少13个月以上。除此之外，重要代码、主数据也要通过历史表保存历史。
 根据业务决定数据的生命周期，比如电商交易频繁的可能是2年，保险行业交易比较少的5年。太老的数据对于数据分析没多大作用，你想10年前的电商交易数据对于现在的电商能有多大帮助，价格、产品、用户都已经完全不同了
如果数据仓库是仅用于分析的话（我看好多地方建立的数据仓库仅用于统计分析，对于数据挖掘基本都没有用），如果有大量的数据挖掘的话，那么数据多些对于结果越精确。（当然，前提是你的历史数据质量不太差的情况下）
现在存储设备越来越便宜，如果不是数据量很惊人的话，一般是不用删除或导出的，因为导出后是需要管理的。
 数据仓库的概念确立之后，有关数据仓库的实施方法、实施路径和架构等问题引发了诸多争议。1994年前后，实施数据仓库的公司大都以失败告终，导致数据集市的概念被提出并大范围运用，其代表人物是Ralph Kimball。
二 数据集市概述
2.1 数据集市概念
•建立数据集市的原因 
 数据仓库是一种反映主题的全局性数据组织。但是，全局性数据仓库往往太大，在实际应用中将它们按部门或个人分别建立反映各个子主题的局部性数据组织,它们即是数据集市。因此，有时我们也称它为部门数据仓库。 
 数据集市：是按照主题域组织的数据集合，用于支持部门级的决策。
 
•例：在有关商品销售的数据仓库中可以建立多个不同主题的数据集市： 
–商品采购数据集市 
–库房使用数据集市 
–商品销售数据集市
2.2 集市分类
•按照数据获取来源： 
–独立型：直接从操作型环境获取数据 
–从属型：从企业级数据仓库获取数据
从属集市和独立集市如下图：



•集市建设途径分为如下两种：
–从 全局数据仓库 到 数据集市 
–从 数据集市 到 全局数据仓库

2.3 集市和主题的区别
•数据仓库与数据集市的关系类似于传统关系数据库系统中的基表与视图的关系。 
•数据集市的数据来自数据仓库，它是数据仓库中数据的一个部分与局部，是一个数据的再抽取与组织的过程。
 具体区别如下：
 
 

 由于数据集市仅仅是数据仓库的某一部分，实施难度大大降低，并且能够满足公司内部部分业务部门的迫切需求，在初期获得了较大成功。但随着数据集市的不断增多，这种架构的缺陷也逐步显现。公司内部独立建设的数据集市由于遵循不同的标准和建设原则，以致多个数据集市的数据混乱和不一致。这就是数据孤岛（百度百科），也叫信息孤岛。
 “企业发展到一定阶段，出现多个事业部，每个事业部都有各自数据，事业部之间的数据往往都各自存储，各自定义。每个事业部的数据就像一个个孤岛一样无法(或者极其困难)和企业内部的其他数据进行连接互动。”我们把这样的情况称为数据孤岛。简单说就是数据间缺乏关联性，数据库彼此无法兼容。
专业人士把数据孤岛分为物理性和逻辑性两种。物理性的数据孤岛指的是，数据在不同部门相互独立存储，独立维护，彼此间相互孤立，形成了物理上的孤岛。逻辑性的数据孤岛指的是，不同部门站在自己的角度对数据进行理解和定义，使得一些相同的数据被赋予了不同的含义，无形中加大了跨部门数据合作的沟通成本。
 解决问题的方法只能是回归到数据仓库最初的基本建设原则上来。1998年，Inmon提出了新的BI架构CIF（CorporationInformation Factory，企业信息工厂），新架构在不同架构层次上采用不同的构件来满足不同的业务需求。
2.4 数据仓库架构
inmon架构
 
kimball架构
 
inmon架构和kimball架构的区别就是inmon的数据仓库是三范式企业级数据仓库，kimball的数据库时多维企业级数据仓库。
架构优缺点


2.5 数仓架构演变史




数据仓库第一代架构
（开发时间 2001-2002 年）
海尔集团的一个 BI 项目，架构的 ETL 使用的是 微软的数据抽取加工工具 DTS，老人使用过微软的 DTS 知道有哪些弊端，后便给出了几个 DTS 的截图。
功能：进销存分析、闭环控制分析、工贸分析等 
硬件环境：
•业务系统数据库：DB2 for Windows,SQL SERVER2000,ORACLE8I
•中央数据库服务器：4EXON,2G,480GSCSI
•OLAP 服务器：2PIV1GHZ,2G,240GSCSI
•开发环境：VISUAL BASIC,ASP,SQL SERVER 2000


数据仓库第二代架构


这是上海通用汽车的一个数据平台，别看复杂，严格意义上来讲这是一套 EDW 的架构、在 EDS 数据仓库中采用的是准三范式的建模方式去构建的、大约涉及到十几种数据源，建模中按照某一条主线把数据都集成起来
这个数据仓库平台计划三年的时间构建完毕，第一阶段计划构建统统一生性周期视图、客户统一视图的数据，完成对数据质量的摸底与部分实施为业务分析与信息共享提供基础平台。第二阶段是完成主要业务数据集成与视图统一，初步实现企业绩效管理。第三阶段全面完善企业级数据仓库，实现核心业务的数据统一。
在第一阶段数据仓库中的数据再次通过阶梯型高度聚合进入到数据集市 DM（非挖掘集市）中，完成对业务的支撑。
数据的 ETL 采用 datastage 工具开发。
数据集市架构




这个是国内某银行的一套数据集市，这是一个典型数据集市的架构模式、面向客户经理部门的考核分析。
数据仓库混合性架构 (Cif)




这是太平洋保险的数据平台。
该平台架构显然是一个混合型的数据仓库架构。它有混合数据仓库的经典结构，每一个层次功能定义的非常明确。
ODS 层 支撑单一的客户视图，是一个偏操作行的做唯一客户识别的，同时提供高可用户性客户主信息查询。
EDW 层基于 IIW（IBM 的通用模型去整理与实施）最细粒度、原子、含历史的数据，也支持查询。
各业务数据集市面向详细业务，采用雪花 / 星型模型去做设计的支撑 OLAP、Report、仪表盘等数据展现方式。
三 数据仓库案例
3.1 环境准备
源系统是mysql库，数据模型如下



建表语句如下：
/*==============================================================*/
/* DBMS name:      MySQL 5.0                                    */
/* Created on:     2018/11/23 1:09:10                           */
/*==============================================================*/

CREATE DATABASE IF NOT EXISTS sales_source DEFAULT CHARSET utf8 COLLATE utf8_general_ci; 

USE sales_source;

DROP TABLE IF EXISTS customer;

DROP TABLE IF EXISTS product;

DROP TABLE IF EXISTS sales_order;

/*==============================================================*/
/* Table: customer                                              */
/*==============================================================*/
CREATE TABLE customer
(
   customer_number      INT(11) NOT NULL AUTO_INCREMENT,
   customer_name        VARCHAR(128) NOT NULL,
   customer_street_address VARCHAR(256) NOT NULL,
   customer_zip_code    INT(11) NOT NULL,
   customer_city        VARCHAR(32) NOT NULL,
   customer_state       VARCHAR(32) NOT NULL,
   PRIMARY KEY (customer_number)
);

/*==============================================================*/
/* Table: product                                               */
/*==============================================================*/
CREATE TABLE product
(
   product_code         INT(11) NOT NULL AUTO_INCREMENT,
   product_name         VARCHAR(128) NOT NULL,
   product_category     VARCHAR(256) NOT NULL,
   PRIMARY KEY (product_code)
);

/*==============================================================*/
/* Table: sales_order                                           */
/*==============================================================*/
CREATE TABLE sales_order
(
   order_number         INT(11) NOT NULL AUTO_INCREMENT,
   customer_number      INT(11) NOT NULL,
   product_code         INT(11) NOT NULL,
   order_date           DATETIME NOT NULL,
   entry_date           DATETIME NOT NULL,
   order_amount         DECIMAL(18,2) NOT NULL,
   PRIMARY KEY (order_number)
);

/*==============================================================*/
/* insert data                                        */
/*==============================================================*/

INSERT INTO customer
( customer_name
, customer_street_address
, customer_zip_code
, customer_city
, customer_state
 )
VALUES
  ('Big Customers', '7500 Louise Dr.', '17050',
       'Mechanicsburg', 'PA')
, ( 'Small Stores', '2500 Woodland St.', '17055',
       'Pittsburgh', 'PA')
, ('Medium Retailers', '1111 Ritter Rd.', '17055',
       'Pittsburgh', 'PA'
)
,  ('Good Companies', '9500 Scott St.', '17050',
       'Mechanicsburg', 'PA')
, ('Wonderful Shops', '3333 Rossmoyne Rd.', '17050',
      'Mechanicsburg', 'PA')
, ('Loyal Clients', '7070 Ritter Rd.', '17055',
       'Pittsburgh', 'PA')
;	   
	   
INSERT INTO product(product_name,product_category) VALUES
('Hard Disk','Storage'),
('Floppy Drive','Storage'),
('lcd panel','monitor')
;

DROP PROCEDURE  IF EXISTS usp_generate_order_data;
DELIMITER //
CREATE PROCEDURE usp_generate_order_data()
BEGIN

	DROP TABLE IF EXISTS tmp_sales_order;
	CREATE TABLE tmp_sales_order AS SELECT * FROM sales_order WHERE 1=0;
	SET @start_date := UNIX_TIMESTAMP('2020-1-1');
	SET @end_date := current_timestamp();-- UNIX_TIMESTAMP('2020-1-31');
	
	SET @i := 1;
	WHILE @i<=10000 DO
		SET @customer_number := FLOOR(1+RAND()*6);
		SET @product_code := FLOOR(1+RAND()* 3);
		SET @order_date := FROM_UNIXTIME(@start_date+RAND()*(@end_date-@start_date));
		SET @amount := FLOOR(1000+RAND()*9000);
		INSERT INTO tmp_sales_order VALUES (@i,@customer_number,@product_code,@order_date,@order_date,@amount);
		SET @i := @i +1;
	END WHILE;
	-- TRUNCATE TABLE sales_order;
	INSERT INTO sales_order(customer_number,product_code,order_date,entry_date,order_amount)
	SELECT customer_number,product_code,order_date,entry_date,order_amount
	FROM tmp_sales_order;
	COMMIT;
	DROP TABLE tmp_sales_order;
END //


CALL usp_generate_order_data();
建完库后的表和数据如下






3.2 数仓分层
3.2.1 分层概念
 数据仓库更多代表的是一种对数据的管理和使用的方式，它是一整套包括了etl、调度、建模在内的完整的理论体系流程。数据仓库在构建过程中通常都需要进行分层处理。业务不同，分层的技术处理手段也不同。
 分层的主要原因是在管理数据的时候，能对数据有一个更加清晰的掌控。详细来讲，主要有下面几个原因：
•清晰数据结构
 每一个数据分层都有它的作用域，这样我们在使用表的时候能更方便地定位和理解。
•数据血缘追踪
 简单来说，我们最终给业务呈现的是一个能直接使用业务表，但是它的来源有很多，如果有一张来源表出问题了，我们希望能够快速准确地定位到问题，并清楚它的危害范围。
•减少重复开发
 规范数据分层，开发一些通用的中间层数据，能够减少极大的重复计算。
•把复杂问题简单化
 将一个复杂的任务分解成多个步骤来完成，每一层只处理单一的步骤，比较简单和容易理解。而且便于维护数据的准确性，当数据出现问题之后，可以不用修复所有的数据，只需要从有问题的步骤开始修复。
•屏蔽原始数据的异常
 屏蔽业务的影响，不必改一次业务就需要重新接入数据
3.2.2 分层的价值
–高效的数据组织形式【易维护】
面向主题的特性决定了数据仓库拥有业务数据库所无法拥有的高效的数据组织形式，更加完整的数据体系，清晰的数据分类和分层机制。因为所有数据在进入数据仓库之前都经过清洗和过滤，使原始数据不再杂乱无章，基于优化查询的组织形式，有效提高数据获取、统计和分析的效率。
–时间价值【高性能】
数据仓库的构建将大大缩短获取信息的时间，数据仓库作为数据的集合，所有的信息都可以从数据仓库直接获取，数据仓库的最大优势在于一旦底层从各类数据源到数据仓库的ETL流程构建成型，那么每天就会有来自各方面的信息通过自动任务调度的形式流入数据仓库，从而使一切基于这些底层信息的数据获取的效率达到迅速提升。
从应用来看，使用数据仓库可以大大提高数据的查询效率，尤其对于海量数据的关联查询和复杂查询，所以数据仓库有利于实现复杂的统计需求，提高数据统计的效率。
–集成价值【简单化】
 数据仓库是所有数据的集合，包括日志信息、数据库数据、文本数据、外部数据等都集成在数据仓库中，对于应用来说，实现各种不同数据的关联并使多维分析更加方便，为从多角度多层次地数据分析和决策制定提供的可能。
–历史数据【历史性】
 记录历史是数据仓库的特性之一，数据仓库能够还原历史时间点上的产品状态、用户状态、用户行为等，以便于能更好的回溯历史，分析历史，跟踪用户的历史行为，更好地比较历史和总结历史，同时根据历史预测未来。
3.2.3 常见分层
数仓的常见分层一般为3层，分别为：数据操作层、数据仓库层和数据集市层。当然根据研发人员经验或者业务，可以分为更多不同的层，只要能达到流程清晰、方便查数即可。


ODS:
 Operate data store，操作数据存储，是最接近数据源中数据的一层，数据源中的数据，经过抽取、洗净、传输，也就说传说中的ETL之后，装入本层。本层的数据，总体上大多是按照源头业务系统的分类方式而分类的。
例如这一层可能包含的数据表可为：人口表（包含每个人的身份证号、姓名、性别、年龄、住址等）、机场登机记录（包含乘机人身份证号、航班号、乘机日期、起飞城市等）、银联的刷卡信息表（包含银行卡号、刷卡地点、刷卡时间、刷卡金额等）、银行账户表（包含银行卡号、持卡人身份证号等）等等一系列原始的业务数据。这里我们可以看到，这一层面的数据还具有鲜明的业务数据库的特征，甚至还具有一定的关系数据库中的数据范式的组织形式。
 但是，这一层面的数据却不完全等同于原始数据。在源数据装入这一层时，根据业务不同，可能会进行诸如去噪（例如去掉明显偏离正常水平的银行刷卡信息）、去重（例如银行账户信息、公安局人口信息中均含有人的姓名，但是只保留一份即可）、提脏（例如有的人的银行卡被盗刷，在十分钟内同时有两笔分别在中国和日本的刷卡信息，这便是脏数据）、业务提取、单位统一、砍字段（例如用于支撑前端系统工作，但是在数据挖掘中不需要的字段）、业务判别等多项工作。
ODS层数据的来源方式：
•业务库
 经常会使用sqoop来抽取，比如我们每天定时抽取一次。在实时方面，可以考虑用canal监听mysql的binlog，实时接入即可。
•埋点日志
 线上系统会打入各种日志，这些日志一般以文件的形式保存，我们可以选择用flume定时抽取，也可以用用spark streaming或者storm来实时接入，当然，kafka也会是一个关键的角色。
•其它数据源
 不同的业务其它数据源不一样,比如第三方数据。
DW:
 Data warehouse，数据仓库层。在这里，从ODS层中获得的数据按照主题建立各种数据模型。
例如以研究人的旅游消费为主题的数据集中,便可以结合航空公司的登机出行信息，以及银联系统的刷卡记录，进行结合分析，产生数据集。在这里，我们需要了解四个概念：维（dimension）、事实（Fact）、指标（Index）和粒度（ Granularity）。
DM:
 该层主要是提供数据产品和数据分析使用的数据，一般会存放在es、mysql等系统中供线上系统使用，也可能会存在Hive或者Druid中供数据分析和数据挖掘使用。 比如我们经常说的报表数据，或者说那种大宽表，一般就放在这里。
注意:
每个分层不是必须要用ODS、DW、DM等字样来标识，可以随便起名字，只要统一这一层是什么类型数据，名字符合知名知意即可。
分层协作层次图


分层协作案例


3.2.4 维度层DIM
 针对存在度量值的表创建维度表，来从各个角度描述事实表。而一个项目中可能有很多张维度表，那我们可以选择将这些维度表放到单独的一个库中，形成维度库，即可看成是维度层，意在让数仓数据清晰明了。


3.3 数仓开发规范
1 数据库命名
    数据命名规则：数仓层_业务方式 如 ods_release/sda_release

2 数仓各层对应数据库
	ods/sda层 -> sda/ods_业务（原始数据）
	dw层 -> dw_业务 （主题库）
	dim层 -> dim_维度 （维表库）
	dm层 -> dm_业务（集市库）
	middle层 -> mid_业务（中间库）
	临时数据 -> temp（临时库）

3 表命名
    (3-1) 数据库表命名规则：
            原始层表：数仓层_来源类型_业务 如 ods_01_xxx
            其他表：数仓层_业务 如 dw_xxx
            
            如果业务名称较长可以简写 如 ods_01_xxx_xx_xx ods_01_xx

    (3-2) 数据来源代码(sda层)
        01 -> hdfs数据
        02 -> mysql数据
        03 -> redis数据
        04 -> mongodb数据
        05 -> tidb数据

如
    ods_release.ods_01_release 投放数据
    ods_release.ods_02_user 注册用户表(业务表：存于MYSQL)

    dw_release.dw_customer 目标客户主题表
    dm_release.dm_customer_stat 目标客户统计表


3.4 数仓模型
 维度建模（dimensional modeling）是数据仓库建设中的一种数据建模方法，将数据结构化的逻辑设计方法，它将客观世界划分为度量和上下文，Kimball 最先提出这一概念。
 其最简单的描述就是，按照事实表，维度表来构建数据仓库，数据集市。这种方法的最被人广泛知晓的名字就是星型模式（Star-schema）。维度建模针对零散的业务进程创建个别的模型。例如，销售信息可以创建为一个模型，库存可以创建为另一个模型，而客户帐户也可以创建为另一个模型。每个模型捕获事实数据表中的事实，以及那些事实在链接到事实数据表的维度表中的特性。由这些排列产生的架构称为星型模式或雪花模式，已被证实在数据仓库设计中很有效。
星型模式是维度模型最简单的形式，也是比较常用的模型，我们的案例采用星型模型。所谓星型模型就是以一个事实表为中心，周围围绕多个维度表。
事实是业务数据的度量值，比如销售额、销售数量等，它记录了特定事件的量化指标，一般是度量值和指向维表的外键组成。事实表的粒度级别通常会设计的比较低。
维度是对事实数据属性的描述，如日期，省份，地区等,维度表的数据量通常不大。
常用的维度表有:
日期维度表,每个数据仓库都需要一个日期维度表。
地理维度表:描述位置信息的数据,如国家,省份,城市,区县,邮编等
产品维度表:描述产品及其属性
人员维度表:描述人员相关信息,部门员工表等
范围维度表:描述分段数据的信息等,比如信用等级
3.5 项目物理模型


3.6 建库、装载数据
连接hive
创建rds库
创建表，脚本如下
create database sales_rds;

CREATE TABLE sales_rds.customer
(
   customer_number      INT ,
   customer_name        VARCHAR(128)  ,
   customer_street_address VARCHAR(256)  ,
   customer_zip_code    INT  ,
   customer_city        VARCHAR(32)  ,
   customer_state       VARCHAR(32)  
);


CREATE TABLE sales_rds.product
(
   product_code         INT,
   product_name         VARCHAR(128)  ,
   product_category     VARCHAR(256)  
);


CREATE TABLE sales_rds.sales_order
(
   order_number         INT ,
   customer_number      INT,
   product_code         INT ,
   order_date           timestamp  ,
   entry_date           timestamp  ,
   order_amount         DECIMAL(18,2)  
)
row format delimited fields terminated by '\t'
;

create database if not exists dw;
use dw;

create table dim_Product
(
   product_sk           int   ,
   product_code         int ,
   product_name         varchar(128),
   product_category     varchar(256),
   version              varchar(32),
   effective_date       date,
   expiry_date          date
)
clustered by (product_sk ) into 8 buckets
stored as orc tblproperties('transactional'='true');


create table dim_customer
(
   customer_sk          int   ,
   customer_number      int ,
   customer_name        varchar(128),
   customer_street_address varchar(256),
   customer_zip_code    int,
   customer_city        varchar(32),
   customer_state       varchar(32),
   version              varchar(32),
   effective_date       date,
   expiry_date          date
)
clustered by (customer_sk ) into 8 buckets
stored as orc tblproperties('transactional'='true');

CREATE TABLE IF NOT EXISTS dw.dim_date(
	date_sk int,
	`date` string,
	month int,
	month_name string,
	quarter int,
	year int
)
row format delimited fields terminated by ','
stored as textfile
;

create table dim_order
(
   order_sk             int  ,
   order_number         int,
   version              varchar(32),
   effective_date       date,
   expiry_date          date
)
clustered by (order_sk ) into 8 buckets
stored as orc tblproperties('transactional'='true');
;

create table fact_sales_order
(
   order_sk             int  ,
   customer_sk          int  ,
   product_sk           int  ,
   order_date_sk        int  ,
   order_amount         decimal(18,2)
)
partitioned by (order_date string)
clustered by (order_sk) into 8 buckets
row format delimited fields terminated by '\t' 
stored as orc tblproperties('transactional'='true');
创建完成后，库如下


sales_rds中表如下


dw中表如下


注意,dw库的表除dim_date存储格式是textfile外,其他的表均为orc
虽然表在模型设计中设置了主键,但是在hive中没有主键,外键唯一性约束,非空约束这些概念
3.7 数据仓库初始化 
3.7.1 装载日期维度表
日期维度是数据仓库中一个很重要的角色,一般来说每个数据仓库都要有日期维度,数据仓库存储的是历史数据。分析中一个很重要的指标是基于时间的一个业务的变化趋势,因此,数据仓库中都包含这个维度。
•从源数据库装载日期
DROP TABLE IF EXISTS `dim_date`;
CREATE TABLE `dim_date` (
  `date_sk` int(11) NOT NULL AUTO_INCREMENT,
  `date` varchar(20) DEFAULT NULL,
  `month` int(11) DEFAULT NULL,
  `month_name` varchar(50) DEFAULT NULL,
  `quarter` int(11) DEFAULT NULL,
  `year` int(11) DEFAULT NULL,
  PRIMARY KEY (`date_sk`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

DELIMITER //
CREATE PROCEDURE USP_Load_Dim_Date(dt_start DATE,dt_end DATE)
BEGIN
WHILE dt_start<=dt_end DO
	INSERT INTO dim_date (`date`,`month`,`month_name`,`quarter`,`year`)
	VALUES (dt_start,MONTH(dt_start),MONTHNAME(dt_start),QUARTER(dt_start),YEAR(dt_start));
	SET dt_start =ADDDATE(dt_start,1);
END WHILE;
COMMIT;
END //

CALL USP_Load_Dim_Date('2010-1-1','2050-1-1');
•直接为hive数仓生成日期表
我们需要写一个shell脚本,生成日期数据文件,上传到hdfs对应的日期维度表的目录即可
#!/bin/bash
date1="$1"
date2="$2"
tempdate=`date -d "$date1" +%F`
tempdateSec=`date -d "$date1" +%s`
enddateSec=`date -d "$date2" +%s`
min=1
max=`expr \( $enddateSec - $tempdateSec \) /  \( 24 \* 60 \* 60 \) + 1`
# max=14611
#cat /datas >./dim_date.csv

while [ $min -le $max ] 
do
	month=`date -d "$tempdate" +%m`
	month_name=`date -d "$tempdate" +%B`
	quarter=`echo $month | awk '{print int(($0-1)/3) +1 }'`
	year=`date -d "$tempdate" +%Y`
	echo ${min}","${tempdate}","${month}","${month_name}","${quarter}","${year} >> ./dim_date.csv
	tempdate=`date -d "+$min day $date1" +%F`
	tempdateSec=`date -d "+min day $date1" +%s`
	min=`expr $min + 1`
done
脚本保存为generatedimdate.sh,执行脚本 
sh ./generate_dim_date.sh '2020-1-1' '2050-1-1'
然后再hive中创建日期维度表
CREATE TABLE IF NOT EXISTS dw.dim_date(
	date_sk int,
	date string,
	month int,
	month_name string,
	quarter int,
	year int
)
row format delimited fields terminated by ','
stored as textfile
;
将生成的文件上传到hdfs下面/hive/warehose/dw.db/dim_date/
hdfs dfs -put -f dim_date.csv /user/hive/warehouse/dw.db/dim_date

## 或者在hive客户端执行
load data local inpath 'dim_date.csv' into table dw.dim_date;
进入hive查看一下表 select * from dim_date limit 100;


至此这个简单的数仓模型就搭建完成了。
数据仓库模型搭建完成后,下一步要做的工作就是数据仓库中数据的填充,也就是我们的ETL过程,ETL可以说是数据仓库系统中最基础也是最重要的环节.我们前面讲过数据仓库的数据来源是业务系统，业务系统可能同时使用多种数据库系统，这些系统在物理上各自独立，在逻辑上有相互联系，数据仓库要从中捕获变化的数据，实现增量数据抽取。
3.8 ETL过程
ETL前提
确认ETL范围：通过对目标表信息的收集
选择ETL工具：a.考虑资金 b.运行的平台、对源和目标的支持程度、数据抽取管理监控、对异常处理
确认解决方案：抽取分析、变化数据的捕获、目标表的刷新策略、数据的转换以及数据验证
ETL原则
尽量对数据进行预处理。保证数据的安全性、集成与加载的高效性。
ETL的过程是主动的“拉取”，而不是从内部“推送”，其可控性将大大增加。
流程化的配置管理
数据质量的保证：正确性、一致性、完成性、有效性、可获取性
3.8.1 Extract数据抽取
3.8.1.1 抽取方式
从源数据导入到数据仓库或者贴源层有两种方式，从源数据拉取数据（pull）和请求源数据推送到数据仓库（push）。一般来讲，后一种方式需要增加业务系统的功能才能进行推送，这个在现实情况中不大行的通，一方面影响业务系统的性能，另一方面增加开发者的工作量，理论上讲，数据仓库不应该要求对源系统做任何的改造，因此一般都采用拉取数据的方式。
3.8.1.2 抽取类型
确定了数据的抽取方式，还需要确定数据的抽取类型，即全量抽取还是增量抽取。如果数据量小并且容易处理，一般采用全量抽取即可。这种方式适合基础编码类的数据源，最典型的是我们通常所说的数据字典类的码表，在数据仓库中就是维表。如果数据量很大，就只能抽取变化的源数据，即最后一次抽取以来发生了变化的数据。
3.8.1.3 抽取方案
•导出为文本方式
以文件文件的形式交换数据是一种可行的通用方法，一般需要将数据源库中的数据导出成预定的义好的分隔符，比如逗号分隔，然后用hadoop的dfs命令将文件上传到hive表对应目录中，或者使用hive的load data local inpath 语句将数据装载的目标表中。
大多数据数据库都提供导出的工具或命令。比如mysql中，可以使用 select … into outfile
select * into outfile /tmp/t1.txt’ fields terminated by ',' from t1;
如果不带查询条件或者导出整个库，可以使用mysqldump命令行工具，可以一次导出多个表，多个库或所有库的数据库。
mysqldump -uroot -p123456 –databases test > /tmp/test.sql
•sqoop抽取数据
Sqoop是用来实现结构型数据（如关系数据库）和Hadoop之间进行数据迁移的工具,我们可以使用sqoop来对我们的销售案例进行数据抽取。
源数据表	RDS库中的表(ODS)	DW库中的表	抽取模式
customer	customer	dim_customer	整体拉取
product	product	dim_product	整体拉取
sales_order	sales_order	factsalesorder dim_order	增量拉取
1.全量导入
 对于 customer、 product这两个表采用整体拉取的方式抽取数据。ETL通常是按一个固定的时间间隔,周期性定时执行,因此对于整体拉取的方式而言,每次导入的数据需要覆盖上次导入的数据。
sqoop import \
--connect jdbc:mysql://hadoop0001:3306/sales_source \
--username root --password 123456 \
--table customer \
--hive-import \
--hive-table sales_rds.customer \
--hive-overwrite \
--target-dir temp

sqoop import \
--connect jdbc:mysql://hadoop0001:3306/sales_source \
--username root --password 123456 \
--table product \
--hive-import \
--hive-table sales_rds.product \
--hive-overwrite \
--target-dir temp
