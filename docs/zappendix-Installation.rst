附录A：数智大脑集群安装
==============

此部分内容介绍如何安装和配置数智大脑集群。此处以5台机器一般性能机器为例子进行集群的安装和配置。

这个过程分为三个阶段：1）安装准备；2）安装及配置；3）运行及验证

安装准备阶段
---------------

准备好5台操作系统为Centos 7 的机器，此处为虚机。主机IP地址为103.227.51.139，端口为 20002 - 20009。

下载deploy_dpaas.tar.gz文件。

复制压缩包 deploy_dpaas.tar.gz 到所有节点的 /opt目录下。

在每台机器上解压该文件。
  ::

  tar -xzvf deploy_dpaas.tar.gz



在node1上 执行 cd deploy_dpaas

在node1上执行 ./init.sh，在当前安装python等包

在node1上执行 python main.py change_host 改变所有待安装节点的hostname



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

