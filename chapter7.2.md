##7.2 **通信对象**
###7.2.1 **简述**
通信对象被描述为服务和协议(~~译者理解服务规定过程，协议规定格式~~)。

所有服务以表格形式给出,包含为服务定义的每个原始参数。特定的服务类型包含不同的参数（如无应答、应答等等）。

所有服务都假定CAN的数据链路层和物理层未出错。这些问题由应用程序解决因而不在本文范畴。

###7.2.2 **过程数据对象(PDO)**
####7.2.2.1 **简述**
实时的数据传输通过“过程数据对象(PDO)”完成。PDO传输无协议开销。

由对象字典提供PDO数据和配置的接口。数据字典中对应的映射结构决定了一个PDO的数据类型和映射关系。 如果CANopen设备支持可变映射PDO，则在配置过程中(见7.3.1)，可通过SDO实现对PDO在数据字典中对应的配置进行修改。

CANopen设备的PDO数量和长度可由应用规范、设备协议或应用协议指定。

PDO分两种用法，发送和接收，分别是Transmit-PDO(TPDO) 和Receive-PDO(RPDO)。支持TPDO 的CANopen设备称为PDO生产者，支持RPDO的称为PDO消费者。PDO由PDO通讯参数和PDO映射参数描述。 其数据类型结构见7.4.8。PDO通讯参数描述了PDO的通信功能。 PDO映射参数包含了PDO传输内容信息。

每个PDO的通信和映射参数都是必不可少的。其对象介绍见7.4。

设备协议中的PDO定义总会涉及CANopen设备中的1st逻辑设备。如果定义用到了2nd逻辑设备，设备协议定义的PDO编号号增加64(40h)见表3中定义。

注意：每个逻辑设备不限制PDO数量，一个只包含单个逻辑设备的CANopen设备可以有多至512个PDO。

|CANopen设备中的逻辑设备|CANopen设备中的PDO编号|设备协议中的PDO编号|
|---|---|---|
|1<sup>st</sup>逻辑设备|PDO number+0<br/>(PDO1到PDO64)|PDO number<br/>(PDO1到PDO64)|
|2<sup>nd</sup>逻辑设备|PDO number+64<br/>(PDO65到PDO128)|PDO number<br/>(PDO1到PDO64)|
|3<sup>rd</sup>逻辑设备|PDO number+128<br/>(PDO129到PDO192)|PDO number<br/>(PDO1到PDO64)|
|4<sup>th</sup>逻辑设备|PDO number+192<br/>(PDO193到PDO256)|PDO number<br/>(PDO1到PDO64)|
|5<sub>th</sub>逻辑设备|PDO number+256<br/>(PDO257到PDO320)|PDO number<br/>(PDO1到PDO64)|
|6<sub>th</sub>逻辑设备|PDO number+320<br/>(PDO321到PDO384)|PDO number<br/>(PDO1到PDO64)|
|7<sub>th</sub>逻辑设备|PDO number+384<br/>(PDO385到PDO448)|PDO number<br/>(PDO1到PDO64)|
|8<sub>th</sub>逻辑设备|PDO number+448<br/>(PDO449到PDO512)|PDO number<br/>(PDO1到PDO64)|


