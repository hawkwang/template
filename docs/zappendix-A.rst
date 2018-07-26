附录A：主要数智基础服务组件
==============

数智基础服务组件为数智大脑构建数据智能应用服务提供了基础功能。
下面就数智大脑中主要数智基础服务组件进行一一介绍。

Schema Registry
---------------

Schema Registry为数智大脑提供一种集中管理元数据的机制,
为允许各类组件之间相互的灵活交互提供了有力保障。


元数据实体
************

Schema Registry中主要涉及了三种元数据实体。

- 元数据组

  用户可以按照功能逻辑或其它需求将元数据进行分组以便于管理。
  元数据组主要用 *GroupName* 进行区分。
  例如，*Group Name ： machine-sensors-kafka* 。

- 元数据定义

  命名元数据的详细信息，也可成为元数据的元数据。
  一个元数据只能隶属一个元数据组。 

  元数据定义主要包括：

  * 名称（name） – 元数据唯一名称，用于查找元数据。例如： *Schema Name ： machine_events_avro:v* 

  * 类型（Type） – 元数据类型，采用Avro形式 [ARVO]_ 。例如： *Schema Type ： avro* 

  * 兼容策略（Compatibility Policy） – 当新版本元数据创建时需要考虑的兼容规则，参见下面的兼容策略部分。例如： *Compatibility Policy ： SchemaCompatibility.BACKWARD* 

  * 序列化/反序列化组件（Serializers/Deserializers） - 可上传的与元数据定义相关联的序列化和反序列化组件。

- 元数据版本

  与已定义元数据相关的版本信息。一个元数据可以有多个版本。

  一个例子::

    {   
        "type" : "record",   
        "namespace" : "databrainhub.dbos.app.driving",   
        "name" : "cargeoevent",   
        "fields" : [     
            { "name" : "eventTime" , "type" : "string" },     
            { "name" : "eventSource" , "type" : "string" },      
            { "name" : "carId" , "type" : "int" },      
            { "name" : "driverId" , "type" : "int"},      
            { "name" : "driverName" , "type" : "string"},      
            { "name" : "routeId" , "type" : "int"},      
            { "name" : "route" , "type" : "string"},      
            { "name" : "eventType" , "type" : "string"},      
            { "name" : "longitude" , "type" : "double"},      
            { "name" : "latitude" , "type" : "double"}     
        ]
    }


兼容策略（Compatibility Policy）
************

Schema Registry的一个主要功能是能在元数据演进时对其进行版本控制。 
兼容性策略在元数据定义级别进行创建，这样可以为每个元数据定义版本演进的兼容规则。
一旦定义了兼容性策略，任何后续版本的更新都必须遵守已定义的规则，以确保兼容性，
否则系统将以错误进行处理。

兼容性策略可以采用如下几种情况取值：

.. csv-table:: 兼容性策略取值范围
   :header: "类型", "值", "解释"
   :widths: 200, 200, 400
   
   "后向兼容", "SchemaCompatibility.BACKWARD", "表示新版本元数据与早期版本兼容。 这意味着按照早期版本写入的数据可以采用新版元数据进行反序列化。"
   "前向兼容", "SchemaCompatibility.FORWARD", "表示现有版本元数据与后续版本兼容。 这意味着仍然可以使用旧版本读取按新版元数据写入的数据。"
   "双向兼容", "SchemaCompatibility.FULL", "表示新版本元数据提供向后和向前兼容性。"
   "无兼容性", "SchemaCompatibility.NONE", "表示无兼容性策略。"

用户可以在添加一个新的元数据时设定兼容性策略，一旦设定就不可以修改。
缺省值是 *SchemaCompatibility.NONE* 。