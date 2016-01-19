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

|**CANopen设备中的逻辑设备**|**CANopen设备中的PDO编号**|**设备协议中的PDO编号**|
|---|---|---|
|1<sup>st</sup>逻辑设备|PDO number+0<br/>(PDO1到PDO64)|PDO number<br/>(PDO1到PDO64)|
|2<sup>nd</sup>逻辑设备|PDO number+64<br/>(PDO65到PDO128)|PDO number<br/>(PDO1到PDO64)|
|3<sup>rd</sup>逻辑设备|PDO number+128<br/>(PDO129到PDO192)|PDO number<br/>(PDO1到PDO64)|
|4<sup>th</sup>逻辑设备|PDO number+192<br/>(PDO193到PDO256)|PDO number<br/>(PDO1到PDO64)|
|5<sub>th</sub>逻辑设备|PDO number+256<br/>(PDO257到PDO320)|PDO number<br/>(PDO1到PDO64)|
|6<sub>th</sub>逻辑设备|PDO number+320<br/>(PDO321到PDO384)|PDO number<br/>(PDO1到PDO64)|
|7<sub>th</sub>逻辑设备|PDO number+384<br/>(PDO385到PDO448)|PDO number<br/>(PDO1到PDO64)|
|8<sub>th</sub>逻辑设备|PDO number+448<br/>(PDO449到PDO512)|PDO number<br/>(PDO1到PDO64)|

表3：PDO编号计算举例

####7.2.2.2 **传输模式**
PDO传输模式分为:
* 同步传输
* 事件驱动传输

CANopen设备的同步由同步应用定期发送同步对象(SYNC对象)来实现。SYNC对象是一个预定义的通信对象(见7.2.5)。图 16 说明了同步和事件驱动的传输原则过程。同步PDO在紧接着SYNC对象之后的同步窗中传输。

![图16：同步和事件驱动的传输](./CANopen_DS301_CN_image/16.png)

图16：同步和事件驱动的传输

PDO 传输模式由传输类型参数指定，包括出发模式。

同步TPDOs传输类型还指定了传输倍率——同步周期倍数因数。传输类型0表示消息会在触发事件后再收到SYNC（非周期）执行。1表示每个SYNC触发一次消息。n表示每隔n个SYNC触发一次消息。事件驱动的TPDOs与SYNC对象无关。

同步 RPDOs 收到数据后在随后的SYNC发生后传递给应用程序，传输类型同时指定了传输倍率。事件驱动 RPDOs 直接将数据传递给应用程序。

