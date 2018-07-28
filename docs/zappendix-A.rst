附录A：基础组件之 **Apache Ambari** 
==============

Apache Ambari是一种基于Web的工具，支持Apache Hadoop集群的供应、管理和监控。
Ambari已支持大多数Hadoop组件，包括HDFS、MapReduce、Hive、Pig、 Hbase、Zookeeper、Sqoop和Hcatalog等。

Ambari主要具有以下特性：

- 通过一步一步的安装向导简化了集群供应。

- 预先配置好关键的运维指标（metrics），可以直接查看Hadoop Core（HDFS和MapReduce）及相关项目（如HBase、Hive和HCatalog）是否健康。

- 支持作业与任务执行的可视化与分析，能够更好地查看依赖和性能。

- 通过一个完整的RESTful API把监控信息暴露出来，集成了现有的运维工具。

- 用户界面非常直观，用户可以轻松有效地查看信息并控制集群。

- Ambari使用Ganglia收集度量指标，用Nagios支持系统报警，当需要引起管理员的关注时（比如，节点停机或磁盘剩余空间不足等问题），系统将向其发送邮件。

- 此外，Ambari能够安装安全的（基于Kerberos）Hadoop集群，以此实现了对Hadoop 安全的支持，提供了基于角色的用户认证、授权和审计功能，并为用户管理集成了LDAP和Active Directory。


下图为Ambari的架构图：

.. figure:: ./images/ambari/ambari-architecture.png
    :width: 550px
    :align: center
    :alt: alternate text
    :figclass: align-center

    Ambari 架构图

Ambari Server 会读取 Stack 和 Service 的配置文件。
当用 Ambari 创建集群的时候，Ambari Server 传送 Stack 和 Service 的配置文件以及 Service 生命周期的控制脚本到 Ambari Agent。Agent 拿到配置文件后，
会下载安装公共源里软件包（如 Redhat，就是使用 yum 服务）。

安装完成后，Ambari Server 会通知 Agent 去启动 Service。
之后 Ambari Server 会定期发送命令到 Agent 检查 Service 的状态，Agent 上报给 Server，并呈现在 Ambari 的 GUI 上。

Ambari Server 支持 Rest API，这样可以很容易的扩展和定制化 Ambari。
甚至于不用登陆 Ambari 的 GUI，只需要在命令行通过 curl 就可以控制 Ambari，以及控制 Hadoop 的 cluster。
具体的 API 可以参见 Apache Ambari 的官方网页 API reference，参见 [Ambari]_ 。

对于安全方面要求比较苛刻的环境来说，Ambari 可以支持 Kerberos 认证的 Hadoop 集群。