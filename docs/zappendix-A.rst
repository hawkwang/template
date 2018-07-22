附录A：数智大脑部署运行参考方案
==============

此参考体系架构提供了基于Dell EMC服务器硬件和网络配置运行数智慧大脑（DataBrainOS）平台的参考方案 [DELLEMCHADOOP]_ 。 
该架构专注于硬件配置，并没有详细介绍DataBrainOS或运行其中的各个组件。

下面介绍运行数智大脑（DataBrainOS）的整个集群架构，包括推荐的服务器配置，网络结构和软件角色分配。

节点架构（Node Architecture）
---------------

数智大脑（DataBrainOS）平台由许多数智基础服务组件组成，涵盖范围广泛的功能。 
大多数这些组件都是作为运行的主服务和工作服务，以分布式方式在集群上运行。
在此体系结构中，我们将物理节点分类为角色，然后将各种服务映射到这些角色上。
根据群集工作负载，可以灵活的将服务和角色分配给各个物理节点。
下表显示了物理节点的分类情况。

.. csv-table:: 集群物理节点角色
   :header: "物理节点角色", "是否必须", "服务器硬件配置"
   :widths: 200, 200, 200
   
   "Active NameNode", "必须", "Master"
   "Standby NameNode", "必须", "Master"
   "Data Node 1 - N", "必须", "Data"
   "High Availability (HA) Node", "必须", "Master"
   "Admin Node", "必须", "Master"
   "Edge Node 1 - N", "建议", "Master"

下表中列出了数智大脑（DataBrainOS）中运行的数智基础服务。

.. csv-table:: 数智大脑（DataBrainOS）基本数智服务
   :header: "数智基础服务", "功能", "Master", "Worker"
   :widths: 100, 350, 200, 200
   
   "HDFS", "Hadoop分布式文件系统", "Primary Namenode, Secondary Namenode", "Data Node"
   "YARN", "Haddop集群资源管理", "YARN Resource Manager", "YARN NodeManager"
   "Hive", "基于Hadoop的数据仓库工具", "Hive Server", ""
   "HBase", "列式NoSQL数据库", "HBase Master", "HBase Region Server"
   "Ambari", "Hadoop集群管理监控服务", "Ambari Server", "Ambari Agent"
   "Flow", "数据流处理编排和部署服务", "", "Data Analyzer"
   "NiFi", "数据清洗、转换、ETL、发现与探索服务", "", "Data Preprocessor"
   "Kafka", "数据集成", "Kafka Manager", "Kafka Broker"
   "Druid", "实时OLAP", "Druid Broker, Druid Router, Druid Coordinator", "Druid Middlemanager"
   "Ranger", "集中式安全管理框架, 并解决授权和审计", "Ranger", ""
   "Storm", "流处理引擎", "Storm UI", "Storm supervisor"
   "Hue", "Apache Hadoop UI, 支持在浏览器端的Web控制台上与Hadoop集群进行交互来分析处理数据，例如操作HDFS上的数据，运行MapReduce Job，执行Hive的SQL语句，浏览HBase数据库等等", "", "Hue Server"
   "Zeppelin", "交互式数据分析和数据可视化", "", "Zeppelin"

下表为推荐的数智基础服务的服务器物理节点部署映射表。

.. csv-table:: 物理节点 VS 数智基础服务
   :header: "物理节点", "服务"
   :widths: 200, 300
   
   "Active NameNode", " 
   NameNode  
    | Quorum Journal Node
    | ZooKeeper
    | Hive Server
    | HBase Master 2
    | Druid Broker"
   "Standby NameNode", " 
   Standby NameNode  
    | Resource Manager
    | Quorum Journal Node
    | Druid Overload
    | ZooKeeper"
   "HA Node", " 
   Standby Resource Manager  
    | Quorum Journal Node
    | ZooKeeper
    | HBase Master 1
    | Storm UI
    | Druid Router
    | Druid Coordinator
    | Ranger"
   "Data Node(x)", " 
   Data Node  
    | NodeManager
    | ZooKeeper
    | HBase RegionServer
    | Druid Middlemanager"
   "Admin Node", " 
   Operational Databases (PostgreSQL)  
    | Ambari
    | Kafka Manager
    | Hue Server
    | Flow
    | Schema Registry
    | Superset
    | Zeppelin
    | MySQL
    | ZooKeeper"
   "Edge Nodes", " 
   API Manager  
    | AI Manager
    | Kafka Broker
    | Storm supervisor
    | H2O
    | NiFi
    | Microservices"
   "", ""

网络（Network Architecture）
------------------

服务器架构（Server Architecture）
----------------

规模规划指南（Sizing Guidelines）
----------------


