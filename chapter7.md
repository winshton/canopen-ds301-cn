#7 **应用层**

##7.1 **数据类型和编码规则**
###7.1.1 **数据类型和编码规则简述**
为了在网络上交换有意义的数据，必须保证数据格式为生产者和消费者所识别。本规范提供基于此概念的数据类型。

编码规则定义了数据所表达值的类型和传输语义。值以位序列的形式表达。位序列以字节为传输单元。各数据类型的编码风格均为小端模式。

应用程序通常请求的都是基本数据类型。使用复合数据类型机制，可以通过扩展可用数据类型表来实现。例如一些常用的数据类型定义如“Visible String”或“Time of Day”(参见7.1.6.3和7.1.6.5)。本文对复合数据类型的定义从技术角度一般取其应用含义，如“DEFTYPES”而不是“DEFSTRUCTS”。

###7.1.2 **数据类型定义**
数据类型定义由数据值和数据类型编码之间的关系决定。类型定义的命名中体现数据类型。数据语法和数据类型定义如下(见/EN61131-3/)。

    data_definition                ::= type_name data_name
    type_definition                ::= constructor type_name
    constructor                    ::= compound_constructor|basic_constructor
    compound_constructor           ::= array_constructor|structure_constructor
    array_constructor              ::=‘ARRAY’‘[‘length‘]’‘OF’type_name 
    structure_constructor          ::=‘STRUCT’‘OF’component_list
    component_list                 ::= component{‘,’component}
    component                      ::= type_name component_name
    basic_constructor              ::=‘BOOLEAN’|
                                      ‘VOID’bit_size|
                                      ‘INTEGER’ bit_size|
                                      ‘UNSIGNED’ bit_size|
                                      ‘REAL32’|
                                      ‘REAL64’|
                                      ‘NIL’
    bit_size                       ::=‘1’|‘2’|<...>|‘64’
    length                         ::= positive_integer
    data_name                      ::= symbolic_name
    type_name                      ::= symbolic_name
    component_name                 ::= symbolic_name
    symbolic_name                  ::= letter{[‘_’](letter|digit)}
    positive_integer               ::=(‘1’|‘2’|<...>|‘9’){digit}
    letter                         ::=‘A’|‘B’|<...>|‘Z’|‘a’|‘b’|<...>|‘z’
    digit                          ::=‘0’|‘1’|<...>|‘9’
 
不要使用递归定义。

type_definition定义使用basic(res.~compound)数据类型的结构体称为basic_constructor (res.~compound_constructor)。

###7.1.3 **位序列**
####7.1.3.1 **位序列定义**
一位的值为0或1。一个位序列b是一个包含0或更多位的有序组。如果位序列b包含多于0位，可以用$$b_j$$（j>0）表示b0,...,bn-1中的位，其中n是正整数。
 
因而有：

$$b = b_0 b_1 ... b_{n-1}$$

叫做一个长度为|b| = n的位序列。长度为0的空序列用$$\varepsilon$$表示。

例：10110100b、1b、101b等都是位序列。

对下面位序列的翻转操作(﹁)

<center>$$b = b_0 b_1 ... b_{n-1}$$</center>
位序列翻转
<center>$$﹁b = ﹁b_0 b_1 ... ﹁b_{n-1}$$</center>
这里﹁0 = 1，﹁1 = 0。

位序列的基本操作是连接。

$$a = a_0 ... a_{m-1}$$和$$b = b_0 ... b_{n-1}$$的位序列a和b连接ab表示为
$$ab = a_0 ... a_{m-1} b_0 ... b_{n-1}$$

例如：(10)(111) = 10111是10和111的连接。

以下为任意的位序列a和b的连接:

<center>$$|ab| = |a| + |b|$$</center>
和
<center>$$\varepsilon a = a\varepsilon= a$$</center>

####7.1.3.2 **位序列的传输语法**
为了跨网络传输，位序列被标记为字节序列。这里和以下的十六进制表示法用于字节表达。让b = b0 ...bn-1的位序列中n<64。k为非负整数，8(k-1)<n<8k。b在传输过程中被打包为表12的形式。 bi、i>n被忽略。
八位位组1先传输而k最后。因此位序列以下列顺序方式传输:
b7、b6、...b0、b15、...、b8、...
 


八位码	1.	2.	k.
	b7..b0	b15..b8	b8k-1..b8k-8
图12：位序列的传输语法
例如：
位9	...	位0
10b	0001b	1100b
2h	1h	Ch
		= 21Ch

位序列b = b0 .. b9 = 0011 1000 01b表示UNSIGNED10的值21Ch,转换为两个八位字节传输:
1Ch和02h。

