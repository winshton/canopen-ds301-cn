#7 **应用层**

##7.1 **数据类型和编码规则**
###7.1.1 **数据类型和编码规则简述**
<span id="jump">为</span>了在网络上交换有意义的数据，必须保证数据格式为生产者和消费者所识别。本规范提供基于此概念的数据类型。

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

<center>$$b = b_0 b_1 ... b_{n-1}$$</center>

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

为了跨网络传输，位序列被标记为字节序列。这里和以下的十六进制表示法用于字节表达。让$$b = b_0 ...b_{n-1}$$的位序列中n<64。k为非负整数，8(k-1)<n<8k。b在传输过程中被打包为表12的形式。 $$b_i、i\geq n$$被忽略。
八位位组1先传输而k最后。因此位序列以下列顺序方式传输:
<center>$$b_7、b_6、...b_0、b_15、...、b_8、...$$</center>

| **八位码** | **1.** | **2.** | **k.** |
| -- | -- | -- | -- |
|  | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>8k-1</sub>...b<sub>8k-8</sub> |

图12：位序列的传输语法

例如：

位9     ...     位0

10<sub>b</sub>  0001<sub>b</sub>   1100<sub>b</sub>

2<sub>h</sub>      1<sub>h</sub>      C<sub>h</sub>

 = 21Ch

位序列b = b<sub>0</sub> .. b<sub>9</sub> = 0011 1000 01b表示UNSIGNED10的值21Ch,转换为两个八位字节传输:
1C<sub>h</sub>和02<sub>h</sub>。

###7.1.4 **基本数据类型**
####7.1.4.1 **简述**
基本数据类型“type_name”与其创建字符相同(aka Symbolic_name)，例如：

BOOLEAN  BOOLEAN

是BOOLEAN数据类型的的类型定义。
####7.1.4.2 **NIL**
基本数据类型NIL表示$$\varepsilon$$。
####7.1.4.3 **Boolean**
基本数据类型BOOLEAN的值为TRUE或FALSE。该值表示的位序列的长度为1。值为TRUE(res.FALSE)代表的位序列是1(res.0)。
####7.1.4.4 **Void**
基本数据类型VOIDn表示长度为n的位序列。类型为VOIDn的数据值是未定义的。类型为VOIDn的位序列数据中的位要么显式指定，要么标记为“随意”。

数据类型VOIDn常用于保留字段和复合结构数据对齐的字节边界。

####7.1.4.5 **Unsigned Interger**
基本数据类型UNSIGNEDn取值非负整数。范围0,...,2<sup>n</sup>-1。该数据表示长度为n的位序列。位序列
<center>$$b  = b_0...b_{n-1}$$</center>
配置值
<center>$$UNSIGNEDn(b) = b_{n-1}2^{n-1} +...+ b_12^1+b_02^0$$</center>
请注意，位顺序从左边最低字节开始。
例如：值266 = 10A<sub>h</sub>数据类型为UNSIGNED16，在总线上以两个八位字节传输，0A<sub>h</sub>然后是01<sub>h</sub>。
UNSIGNEDn的数据类型传输定义于图13。

| **八进制数** | **1.** | **2.** | **3.** | **4.** | **5.** | **6.** | **7.** | **8.** |
| -- | -- | -- | -- | -- | -- | -- | -- | -- |
| UNSIGNED8 | b<sub>7</sub>...b<sub>0</sub> |  |  |  |  |  |  |  |
| UNSIGNED16 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> |  |  |  |  |  |  |
| UNSIGNED24 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>23</sub>...b<sub>16</sub> |  |  |  |  |  |
| UNSIGNED32 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>23</sub>...b<sub>16</sub> | b<sub>31</sub>...b<sub>24</sub> |  |  |  |  |
| UNSIGNED40 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>23</sub>...b<sub>16</sub> | b<sub>31</sub>...b<sub>24</sub> | b<sub>39</sub>...b<sub>32</sub> |  |  |  |
| UNSIGNED48 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>23</sub>...b<sub>16</sub> | b<sub>31</sub>...b<sub>24</sub> | b<sub>39</sub>...b<sub>32</sub> | b<sub>47</sub>...b<sub>40</sub> |  |  |
| UNSIGNED56 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>23</sub>...b<sub>16</sub> | b<sub>31</sub>...b<sub>24</sub> | b<sub>39</sub>...b<sub>32</sub> | b<sub>47</sub>...b<sub>40</sub> | b<sub>55</sub>...b<sub>48</sub> |  |
| UNSIGNED64 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>23</sub>...b<sub>16</sub> | b<sub>31</sub>...b<sub>24</sub> | b<sub>39</sub>...b<sub>32</sub> | b<sub>47</sub>...b<sub>40</sub> | b<sub>55</sub>...b<sub>48</sub> | b<sub>63</sub>...b<sub>56</sub> |

图13：数据类型 UNSIGNEDn的传输规则

####7.1.4.6 **Signed Integer**
基本数据类型INTEGERn值为整数。取值的范围是-2<sup>n-1</sup>~2<sup>n–1</sup>–1.该数据表示长度为n的位序列。位序列<center>b = b0..b<sub>n-1</sub></center>
配置值
<center>INTEGERn(b) = b<sub>n-2</sub>2<sup>n-2</sup>+ ...+b<sub>1</sub>2<sup>1</sup>+b<sub>0</sub>2<sup>0</sup> 如果b<sub>n-1</sub> = 0</center>
并执行双补运算
<center>INTEGERn(b) = -INTEGERn(^b)-1	如果b<sub>n-1</sub> = 1</center>
请注意，位顺序从左侧最低位开始。

例如：值-266 = FEF6<sub>h</sub>数据类型NTERGER16在总线上以两个八位字节传输，F6<sub>h</sub>然后是FE<sub>h</sub>。
INTEGERn的数据类型被定义见图14。

| **八进制数** | **1.** | **2.** | **3.** | **4.** | **5.** | **6.** | **7.** | **8.** |
| -- | -- | -- | -- | -- | -- | -- | -- | -- |
| INTEGER8 | b<sub>7</sub>...b<sub>0</sub> |  |  |  |  |  |  |  |
| INTEGER16 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> |  |  |  |  |  |  |
| INTEGER24 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>23</sub>...b<sub>16</sub> |  |  |  |  |  |
| INTEGER32 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>23</sub>...b<sub>16</sub> | b<sub>31</sub>...b<sub>24</sub> |  |  |  |  |
| INTEGER40 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>23</sub>...b<sub>16</sub> | b<sub>31</sub>...b<sub>24</sub> | b<sub>39</sub>...b<sub>32</sub> |  |  |  |
| INTEGER48 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>23</sub>...b<sub>16</sub> | b<sub>31</sub>...b<sub>24</sub> | b<sub>39</sub>...b<sub>32</sub> | b<sub>47</sub>...b<sub>40</sub> |  |  |
| INTEGER56 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>23</sub>...b<sub>16</sub> | b<sub>31</sub>...b<sub>24</sub> | b<sub>39</sub>...b<sub>32</sub> | b<sub>47</sub>...b<sub>40</sub> | b<sub>55</sub>...b<sub>48</sub> |  |
| INTEGER64 | b<sub>7</sub>...b<sub>0</sub> | b<sub>15</sub>...b<sub>8</sub> | b<sub>23</sub>...b<sub>16</sub> | b<sub>31</sub>...b<sub>24</sub> | b<sub>39</sub>...b<sub>32</sub> | b<sub>47</sub>...b<sub>40</sub> | b<sub>55</sub>...b<sub>48</sub> | b<sub>63</sub>...b<sub>56</sub> |

图14：数据类型INTEGERn传输规则

####7.1.4.7 **浮点数**
基本数据类型REAL32和REAL64值为实数。

数据类型REAL32表示32位长度的位序列。编码遵守/IEEE754/。传输语法见图15。

数据类型REAL64表示64位长度的位序列。编码遵守/IEEE754/。

32位的位序列有值(有限非0实数、±0、±_)或NaN(not-a-number)。位序列
<center>b = b<sub>0</sub>...b<sub>31</sub></center>
可配值为(有限的非零数)
<center>REAL32(b) = (-1)<sup>S</sup>2<sup>E-127</sup>(1+F)</center>
这里
S = b<sub>31</sub>是符号。

E = b<sub>30</sub>2<sup>7</sup>+...+b<sub>23</sub>2<sup>0</sup>, 0<E<255、为无偏(un-biase)指数。

F = 2<sub>-23</sub>(b<sub>22</sub>2<sup>22</sup> +...+b<sub>1</sub>2<sup>1</sup>+b<sub>0</sub>2<sup>0</sup>)表示小数部分。

E = 0用于表达±0。E=255用于表示无穷大和NaN’s.请注意，位顺序从左侧最低位开始。

例如：

6.25 = 2<sup>E-12</sup>(1+F)

E = 129 = 2<sup>7</sup>+2<sup>0</sup>和

F = 2<sup>-1</sup>+2<sup>-4</sup> = 2<sup>-23</sup>(2<sup>22</sup>+2<sup>19</sup>)项数表示为：

| S | E | F |
| -- | -- | -- |
| b<sub>31</sub> | b<sub>30</sub>.b<sub>23</sub> | b<sub>22</sub>.b<sub>0</sub> |
| 0 | 100 0000 1<sub>b</sub> | 100 1000 0000 0000 0000 0000<sub>b</sub> |

6.25 =b<sub>0</sub>..b<sub>31</sub> = 0000 0000 0000 0000 0001 0011 0000 0010<sub>b</sub>

转换顺序：

| **八进制数** | **1.** | **2.** | **3.** | **4.** |
| -- | -- | -- | -- | -- |
| REAL32 |00<sub>h</sub> | 00<sub>h</sub> | C8<sub>h</sub> | 40<sub>h</sub> |
|  | b<sub>7</sub>..b<sub>0</sub> | b<sub>15</sub>..b<sub>8</sub> | b<sub>23</sub>..b<sub>16</sub> | b<sub>31</sub>..b<sub>24</sub> |

图15：数据类型 REAL32的传输规则

###7.1.5 **复合数据类型**
复合数据类型展开为包含基本数据类型的独立表单类型定义。相应地，复合数据类型´type_name´命名规则是由基本类型´basic_type_i´表单组成的复合数据名´component_name_i´确定。

复合数据类型由ARRAY 和 STRUCT OF构建。
    
    STRUCT OF
        basic_type_1    component_name_1,
        basic_type_2    component_name_2,
            …                   …
        basic_type_N    component_name_N
    type_name

    ARRAY[length] OF basic_type type_name

复合数据类的位序列由组成复合数据的各数据的位序列连接而成。

假设组件´component_name_i´表达的位序列为

                b(i), 其中 i = 1,…,N
然后复合数据的位序列连接为
<center>b<sub>0</sub>(1)..b<sub>n-1</sub>(1)..b<sub>n-1</sub>(N).</center>
例如：
如下数据类型

    STRUCT OF
        INTEGER10   x,
        UNSIGNED5   u
    NewData
假定x = –423 = 259<sub>h</sub>，u = 30 = 1E<sub>h</sub>。让b(x)和b(u)表示位序列x和u。然后：

b(x)        =  b<sub>0</sub>(x)..b<sub>9</sub>(x)  = 1001101001<sub>b</sub>

b(u)        =  b<sub>0</sub(u)..b<sub>4</sub>(u) = 01111<sub>b</sub>

b(xu)            = b(x)b(u)     = b<sub>0</sub>(xu)..b<sub>14</sub>(xu) = 1001101001 01111<sub>b</sub>

该值的结构被转换为两个八位字节，59<sub>h</sub>然后是7A<sub>h</sub>。

###7.1.6 **扩展数据类型**
####7.1.6.1 **简述**
扩展数据类型分为基本数据类型和复合数据类型，分别在以下小节定义。
####7.1.6.2 **八进制字符串**
数据类型OCTET_STRIN长度定义如下，length：八进制节字符串的长度。

    ARRAY[length] OF UNSIGNED8 OCTET_STRINGlength
####7.1.6.3 **可显示字符串**
数据类型VISIBLE_STRINGlength定义如下，VISIBLE_CHAR类型数据取值 0h和20h~7Eh。数据由ISO 
646-1973(E)7位编码的字符解释。length：可见字符串长度。

    UNSIGNED8	VISIBLE_CHAR
    ARRAY[length]  OF VISIBLE_CHAR	VISIBLE_STRINGlength
无需0h作为字符串结束标志。
####7.1.6.4 **Unicode字符串**
数据类型UNICODE_STRINGlength定义如下；length unicode字符串长度。
    
    ARRAY[length] OF UNSIGNED16	UNICODE_STRINGlength
####7.1.6.5 **时间**
数据类型TIME_OF_DAY表示绝对时间。根据这一定义和编码规则，TIME_OF_DAY由48位的位序列表达。

成员ms是午夜起算的毫秒计数。成员days是自1984年1月1日以来的天数。

    STRUCT OF
        UNSIGNED28      ms,
        VOID4           reserved,
        UNSIGNED16      days
    TIME_OF_DAY
####7.1.6.6 **时间差**
数据类型TIME_DIFFERENCE表示时间差。根据这一定义和编码规则,时间差由48位的位序列表达。

ms表示毫秒数。days表示天数。

    STRUCT OF
        UNSIGNED28      ms,
        VOID4           reserved,
        UNSIGNED16      days
    TIME_DIFFERENCE
####7.1.6.7 **域**
域用于从客户端到服务器传输任意大的数据块，反之亦然。数据块内容由应用程序指定不在本文范围。

[页首](#jump)
