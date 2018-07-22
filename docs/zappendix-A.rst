附录A：数智大脑部署运行参考方案
==============

此参考体系架构提供了基于Dell EMC服务器硬件和网络配置运行数智慧大脑（DataBrainOS）平台的参考方案 [DELLEMCHADOOP]_ 。 
该架构专注于硬件配置，并没有详细介绍DataBrainOS或运行其中的各个组件。

下面介绍运行DataBrainOS的整个集群架构，包括推荐的服务器配置，网络结构和软件角色分配。

节点架构（Node Architecture）
---------------

DataBrainOS平台由许多基础数智服务组件组成，涵盖范围广泛的功能。 
大多数这些组件都是作为运行的主服务和工作服务，以分布式方式在集群上运行。
在此体系结构中，我们将物理节点分类为角色，然后将各种服务映射到这些角色上。
根据群集工作负载，可以灵活的将服务和角色分配给各个物理节点。
下表显示了物理节点的分类情况。

.. csv-table:: 集群物理节点角色
   :header: "物理节点角色", "是否必须"，"服务器硬件配置"
   :widths: 200, 200, 200
   
   "Active NameNode", "必须", "Master"
   "Standby NameNode", "必须", "Master"
   "Data Node 1 - N", "必须", "Data"
   "High Availability (HA) Node", "必须", "Master"
   "Admin Node", "必须", "Master"
   "Edge Node 1 - N", "建议", "Master"






网络（Network Architecture）
------------------

服务器架构（Server Architecture）
----------------

规模规划指南（Sizing Guidelines）
----------------


