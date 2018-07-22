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
   "Edge Node 1 - N", "建议（否则服务要与其它节点公用资源）", "Master"

下表中列出了数智大脑（DataBrainOS）中运行的数智基础服务。

.. csv-table:: 数智大脑（DataBrainOS）基本数智服务
   :header: "数智基础服务", "功能", "Master", "Worker"
   :widths: 100, 350, 200, 200
   
   "HDFS", "Hadoop分布式文件系统", "Primary Namenode, Secondary Namenode", "Data Node"
   "YARN", "Haddop集群资源管理", "YARN Resource Manager", "YARN NodeManager"
   "Hive", "基于Hadoop的数据仓库工具", "Hive Server", ""
   "HBase", "列式NoSQL数据库", "HBase Master", "HBase Region Server"
   "Ambari", "Hadoop集群管理监控服务", "Ambari Server", "Ambari Agent"
   "Flow", "拖拽式数智单元编排和部署组件", "Data Analyzer", ""
   "NiFi", "数据清洗、转换、ETL、发现与探索组件", "Data Preprocessor", ""
   "Kafka", "高吞吐量的分布式发布订阅-消息系统", "", "Kafka Broker"
   "Kafka Manager", "Kafka 管理工具，支持管理多个集群、轻松检查集群状态等", "Kafka Manager", ""
   "Druid", "海量实时OLAP数据仓库", "Druid Broker, Druid Router, Druid Coordinator", "Druid Middlemanager"
   "Ranger", "集中式安全管理框架, 并解决授权和审计", "Ranger", ""
   "Storm", "分布式高容错的实时计算引擎", "Storm UI", "Storm supervisor"
   "Hue", "Apache Hadoop UI, 支持在浏览器端的Web控制台上与Hadoop集群进行交互来分析处理数据，例如操作HDFS上的数据，运行MapReduce Job，执行Hive的SQL语句，浏览HBase数据库等等", "", "Hue Server"
   "Zeppelin", "交互式数据分析和数据可视化", "Zeppelin", ""
   "H2O", "企业级机器学习服务组件", "", "H2O"
   "AI Manager", "管理H2O构建的模型、发布模型服务等", "", "AI Manager"
   "API Manager", "管理数智大脑（DataBrainOS）对外赋能的API，保留认证、鉴权、流量控制等", "", "API Manager"
   "Kerberos", "数智大脑（DataBrainOS）采用Kerberos作为安全认证系统", "", "Kerberos"
   "DataBrainOS UI", "数智大脑（DataBrainOS）统一访问界面", "", "DataBrainOS UI"

下表为推荐的数智基础服务的服务器物理节点部署映射表。

.. csv-table:: 物理节点 VS 数智基础服务
   :header: "物理节点", "服务"
   :widths: 200, 400
   
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
   Ambari 
    | Operational Databases (PostgreSQL) 
    | Kafka Manager
    | Hue Server
    | Flow
    | Schema Registry
    | Superset
    | Zeppelin
    | MySQL
    | Kerberos
    | ZooKeeper"
   "Edge Nodes", " 
   DataBrainOS UI  
    | API Manager
    | AI Manager
    | Kafka Broker
    | Storm supervisor
    | H2O
    | NiFi
    | Microservices"


网络（Network Architecture）
------------------

集群网络架构旨在满足高性能和可扩展的集群需求，同时兼顾提供冗余和访问管理功能。
该体系结构是基于10GbE网络技术的leaf-spine模型，并使用Dell S4048-ON交换机作为leaf，
使用Dell S6000-ON交换机作为spine。网络采用IPv4。

.. figure:: ./images/network-connections.PNG
    :width: 550px
    :align: center
    :height: 350px
    :alt: alternate text
    :figclass: align-center

    网络架构图

集群网络
***************

从上图可以看出，集群使用了三种网络，具体信息参见下表：

.. csv-table:: 集群网络
   :header: "网络", "连接", "交换机"
   :widths: 200, 200, 200
   
   "集群数据网络（Data Network）", "万兆以太网（Bonded 10GbE）", "双顶架（Pod）交换机和支持端口聚合功能交换机"
   "BMC网络（BMC Network）", "1GbE", "每个机架使用专用交换机"
   "边缘网络（Edge Network）", "10GbE", "直接到边缘网络，或通过pod或聚合交换机"


服务器架构（Server Architecture）
----------------

我们将服务器硬件配置分为两大类：

- 主节点（Master Node）
- 数据节点（Data Node）

主节点（Master Node）
******************

主节点用于托管关键群集服务，并且优化配置以减少停机并提供高性能。 推荐的配置参见下表。

.. csv-table:: 服务器硬件配置-主节点（Master Node） [1]_ 
   :header: "组件", "硬件选型"
   :widths: 200, 400
   
   "平台", "Dell EMC PowerEdge R730xd (12-Drive Option with Flex Bay)"
   "处理器", "2x Intel Xeon E5-2650 v4 2.2 GHz (12-Core)"
   "RAM（最小）", "256 GB"
   "NDC", "Intel X520 Dual-port 10GbE + I350 Dual-port 1GbE"
   "硬盘 (Hot-Plug)", "8x 1TB 7.2K RPM SAS 12Gbps (Data)"
   "Disk (Flex Bay)", "2x 600GB 10K RPM SAS 12Gbps (OS)"
   "存储控制器", "Dell EMC PowerEdge RAID Controller (PERC) H730"



数据节点（Data Node）
********************

数据节点是DataBrainOS集群的核心。数据节点需要综合考虑计算和存储存储能力，在此给出了一般性推荐配置，参见下表。

.. csv-table:: 服务器硬件配置-数据节点（Data Node） [1]_ 
   :header: "组件", "硬件选型"
   :widths: 200, 400
   
   "平台", "Dell EMC PowerEdge R730xd (12-Drive Option with Flex Bay)"
   "处理器", "2x Intel Xeon E5-2650 v4 2.2 GHz (12-Core)"
   "RAM（最小）", "256 GB"
   "NDC", "Intel X520 Dual-port 10GbE + I350 Dual-port 1GbE (LACP Bonded)"
   "硬盘 (Hot-Plug)", "12x 4TB 7.2K RPM SAS 12Gbps (HDFS) – Non-RAID or RAID 0"
   "Disk (Flex Bay)", "2x 600GB 10K RPM SAS 12Gbps (OS) – RAID 1 (Mirror)"
   "存储控制器", "Dell PowerEdge RAID Controller (PERC) H730"







规模规划指南（Sizing Guidelines）
----------------




.. [1] 该配置可采用R740xd或其他类似配置机型根据自身需求进行相应调整。
