数智应用服务： **数智探索单元** 
==============

.. figure:: ./images/NIFI.PNG
    :width: 600px
    :align: center
    :height: 400px
    :alt: alternate text
    :figclass: align-center

    数智探索单元

DataBrainOS Data Preprocessor用于构建数智探索单元，该组件是在Apache NiFi基础上改造形成。
NiFi是Apache支持下基于可视化流程设计的数据分发平台，是大数据的搬运、提取、推送、转换、聚合、分发的开源软件工具，
能够与Hadoop生态系统的大数据存储和各种文件、REST服务、SOAP服务、消息服务等联合使用，构成一体化的数据流服务。 
Apache NiFi详细使用手册可参见 [NIFI]_ 。

两个重要术语
-------------

为了使用熟练 Data Preprocessor / NiFi 以构建数智探索单元，用户需要了解两个关键术语。

.. csv-table:: 两个关键术语
   :header: "术语", "解释"
   :widths: 100, 400
   
   "FlowFile（流文件）", "每条“用户数据”（用户带入平台进行处理和分发的数据）被称为FlowFile。 FlowFile由两部分组成：属性和内容。 内容是用户数据本身。 属性是与用户数据关联的键值对（key-value）。"
   "Processor（处理器）", "Processor是处理组件，负责创建，发送，接收，转换，路由，拆分，合并和处理FlowFiles。 它是用户可用于构建数智探索和发现单元（数据流）的最重要的构建块，在DataBrainOS中也称之为数智探索神经元。"

架构示意
----------------

为了方便用户深入理解机制，下面对服务组件的架构做简要介绍，如下图所示。

.. figure:: ./images/NIFI/nifi_architecture.png
    :width: 60%
    :align: center
    :alt: alternate text
    :figclass: align-center

    架构示意

.. csv-table:: 架构组件介绍
   :header: "架构组件", "简要介绍"
   :widths: 100, 400
   
   "WebServer", "其目的在于提供基于HTTP的命令和控制API。"
   "Flow Controller", "这是操作的核心，以Processor为处理单元，提供了用于运行的扩展线程，并管理扩展接收资源时的调度。"
   "Extensions", "各种类型的扩展，Extensions的关键在于扩展在JVM中操作和执行。"
   "FlowFile Repository", "FlowFile库的作用是跟踪记录当前在流中处于活动状态的给定流文件的状态，其实现是可插拔的，默认的方法是位于指定磁盘分区上的一个持久的写前日志。"
   "Content Repository", "Content库的作用是给定流文件的实际内容字节所在的位置，其实现也是可插拔的。默认的方法是一种相对简单的机制，即在文件系统中存储数据块。"
   "Provenance Repository", "Provenance库是所有源数据存储的地方，支持可插拔。默认实现是使用一个或多个物理磁盘卷，在每个位置事件数据都是索引和可搜索的。"



