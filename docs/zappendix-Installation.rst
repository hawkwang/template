附录A：数智大脑集群安装
==============

此部分内容介绍如何安装和配置数智大脑集群。此处以5台机器一般性能机器为例子进行集群的安装和配置。

这个过程分为三个阶段：1）安装准备；2）安装及配置；3）运行及验证

安装准备阶段
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

.. figure:: ./images/network-connections.PNG
    :width: 550px
    :align: center
    :height: 350px
    :alt: alternate text
    :figclass: align-center

    网络架构图




下表中列出了数智大脑（DataBrainOS）中运行的数智基础服务。


安装及配置
---------------------


运行及验证
---------------------

