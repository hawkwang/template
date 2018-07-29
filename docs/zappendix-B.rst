附录B：基础组件之 **Hadoop** 
==============

Hadoop是一个开源框架，允许使用简单的编程模型在跨计算机集群的分布式环境中存储和处理大数据。
它的设计是从单个服务器扩展到数千个机器，每个都提供本地计算和存储。

用户可以在不了解分布式底层细节的情况下，开发分布式程序。充分利用集群的威力进行高速运算和存储。
Hadoop实现了一个分布式文件系统（Hadoop Distributed File System），简称HDFS。
HDFS有高容错性的特点，并且设计用来部署在低廉的（low-cost）硬件上；
而且它提供高吞吐量（high throughput）来访问应用程序的数据，适合那些有着超大数据集（large data set）的应用程序。

Hadoop 架构
----------------

Hadoop包括如下几个模块:

- **Hadoop Common**: 该基本户模块用以支撑Hadoop中的其他模块。

- **Hadoop Distributed File System (HDFS™)**: 一种分布式文件系统，提供对应用程序数据的高吞吐量访问支持。

- **Hadoop YARN**: 作业调度和集群资源管理的框架。

- **Hadoop MapReduce**: 基于YARN，用于并行处理大型数据集的模块。

HDFS 架构
***********************

下图为HDFS的架构图：

.. figure:: ./images/hadoop/hdfsarchitecture.png
    :width: 550px
    :align: center
    :alt: alternate text
    :figclass: align-center

    HDFS 架构图


YARN 架构
***********************

下图为YARN的架构图：

.. figure:: ./images/hadoop/yarn_architecture.gif
    :width: 550px
    :align: center
    :alt: alternate text
    :figclass: align-center

    YARN 架构图


主要优点
----------------

主要有以下几个优点：

- **高可靠性** - Hadoop能够自动保存数据的多个副本，并且能够自动将失败的任务重新分配。

- **高扩展性** - Hadoop是在可用的计算机集簇间分配数据并完成计算任务的，这些集簇可以方便地扩展到数以千计的节点中。

- **高效性** - Hadoop能够在节点之间动态地移动数据，并保证各个节点的动态平衡，因此处理速度非常快。

- **低成本** - 与一体机、商用数据仓库等相比，hadoop是开源的，项目的软件成本因此会大大降低。

更多信息可参见官网： [Hadoop]_ 。


相关组件
----------------

Hive、HBase等。