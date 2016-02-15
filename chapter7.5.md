##7.5 **通信协议规范**
###7.5.1 **对象及其条目说明规范**
对象字典对象的条目结构描述遵守以下方式：所有设备协议、接口协议和应用协议都基于通信协议所使用的对象及其条目说明规则，见表52和表53。
<center/>表52：对象描述格式

**对象描述**

|索引|协议定义的索引号|
|---|---|
|名称|参数名称|
|对象代码|变量的分类|
|数据类型|数据类型分类|
|类别|可选的或强制性的区分|
对象代码是表52描述的对象定义的成员。考虑可读性，对象描述要附带对象的名称。

<center/>表53：对象值的描述格式

**条目说明**

|子索引|子项编号|
|---|---|
|描述|描述子索引名称(字段只用于数组、记录和结构)|
|数据类型|数据类型分类(字段只用于记录和结构)|
|条目类别|表明对象条目是可选、强制还是条件的|
|访问权限|只读(ro)或读/写(rw)或只写(wo)|
|PDO映射|如果该对象可映射至PDO则定义该项。说明：<br/>可选：对象可以映射到PDO<br/>默认值：对象是默认映射的一部分(请参见设备协议或应用协议)<br/>TPDO：对象可被映射到TPDO不应被映射到RPDO<br/>RPDO：对象可被映射到RPDO不应该映射到TPDO<br/>无：对象不应映射到PDO|
|取值范围|可能的取值范围，或数据类型的全部取值范围|
|默认值|无：无默认值<br/>协议指定：由协议指定默认值<br/>造商指定：默认值由CANopen设备制造商指定<br/>值：CANopen设备初始化后的默认值|
简单的变量带有值定义即可，无须格外的条目类别定义。复合数据类型其值定义应包括每个元素(子索引)。
###7.5.2 **通信协议对象的详细规范**
####7.5.2.1 **对象1000h：设备类型**
此对象提供有关设备类型的信息。该对象描述了逻辑设备类型及其功能。它由两个16位域组成，一个描述所用设备协议或应用协议，另一个给出逻辑设备的附加功能信息。附加的信息参数为设备协议和应用协议所指定。其说明不属于本文范围，定义于相应的设备协议和应用协议。  
**值定义**  
该值为0000h表示逻辑设备不遵守标准设备协议。在这种情况下附加的信息应为0000h(如果没有更多的逻辑设备)或FFFF<sub>h</sub>(如果还有其它逻辑设备)。  
多逻辑设备其附加信息应为FFFF<sub>h</sub>且其设备协议应为对象字典中第一逻辑设备。所有其他逻辑设备模块的协议标识于对象67FF<sub>h</sub> + x * 800<sub>h</sub>且x = 逻辑设备内部编号(从1到8)减去1。这些对象将描述逻辑设备的设备类型，与对象1000<sub>h</sub>具有相同的值定义。  
 图52：设备类型参数结构
32	16    15	0
MSB	LSB
图52：设备类型参数结构
对象描述

索引	1000h
名称	设备类型
对象代码	VAR
数据类型	UNSIGNED32
类别	强制
条目说明
子索引	00h
访问权限	ro
PDO 映射	否
取值范围	请参阅 值定义
默认值	协议或制造商指定
