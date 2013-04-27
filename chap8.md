# 8 类型

本规范中有很多算法，这些算法操作属性值每一个都有相关的类型。这些值的类型在本章节中给出了明确的定义。从而进一步子类化为ECMAScript语言的类型或规范类型。

ECMAScript语言的类型对应于一些值，它们可以被使用ECMAScript语言的ECMAScript程序员直接操作。ECMAScript语言类型就是：Undefined，Null，Boolean，String，Number和Object。

规范类型对应于一些元数值，它们是在算法中用来描述ECMAScript语言结构和ECMAScript语言类型。这些规范类型有：Reference，List，Completion，Property Descriptor，Property Identifier，Lexical Environment和Environment Record。规范类型值是一些人造数值，它们在ECMAScript的实现中并没有必要对应于任何特定的实体。规范类型值也可能用来描述ECMAScript表达式求值的中间结果，这些中间结果并不能保存为对象的属性或ECMAScript语言变量值。 

本规范中，标记“Type(x)”表示“x的类型”，“type”即指定义在本章节中的ECMAScript语言或规范类型。

# 8.1 Undefined类型

Undefined类型只有一个值，即undefined。没有被赋值的任何变量的值即为undefined。

# 8.2 Null类型

Null类型只有一个值，即null。

# 8.3 Boolean类型

Boolean类型代表一个逻辑实体拥有两个值，即true或false。

# 8.4 String类型

String类型是有限个有顺序的零或多个16位无符号整数（“元素”）的集合。String类型常常用来表示一个正在运行的ECMAScript程序中的文本数据。这种情况下，字符串中的每个元素被看作是一个编码单元值（见第六章）。每个元素在序列中占据了一个位置。这些位置的索引从零开始。如果存在，第一个元素的位置为0，下一个元素为1，以此类推。字符串的长度就是这些元素（16位数值）的个数。空字符串的长度为零，因此它也不包含任何元素。

如果一个字符串含有实际的文本数据，每个元素会被看作一个单独的UTF-16编码单元。不管怎么样，这就是字符串实际的存储格式。所有在字符串之上的操作都将它们看作是无差别的16位无符号整数的序列；不能保证产生的字符串还是以常规的形式存在，也不保证“语言敏感”的结果。

> 注：设计背后的合理性就是要来保持字符串的实现是尽可能的简单和高效 

# 8.5 Number类型

Number类型事实上有18437736874454810627个值（也就是2^64-2^53+3个），这代表着在IEEE二进制浮点运算标准中制订的双精度64位格式的IEEE 754的值，并且排除9007199254740990（也就是2^53-2），它表示了IEEE标准中的“Not-a-Number”。在ECMAScript中它被表示为一个特定的NaN值。（注：NaN的值是由程序表达式NaN来产生。）在一些实现中，外部代码可能检测到不同的“Not-a-Number”之间的区别，但这些行为是依赖不同实现的；针对ECMAScript代码，所有NaN的值应该是无差别的。

还有两个特殊的值，叫作“正无穷”和“负无穷”。为了简洁和说明性的目的分别通过 +∞ 和−∞来指代这两个值。（注：这两个无穷数值是由程序表达式+Infinity和-Infinity来产生，“+”可以省略。）

其他的18437736874454810624个值（也就是2^64-2^53）是有限的。一半是正数，一半是负数；每个正有限数值都有一个对应的负有限数值，它们的绝对值相等。

请注意，同时存在正零和负零两个数。为了简洁和说明性的目的分别通过+0和-0来指代这两个值。（注意：这两个零也是由程序表达式+0和-0来产生，“+”可以省略。）

这些18437736874454810622个非零有限值（也就是2^64-2^53-2）可以分成两类：

其中的18428729675200069632个值（也就是2^64-2^54）可以用常规的形式表示为：

> s * m * 2^e

其中，s为+1或者-1，m为小于2^53大于等2^52的正整数，e为-1074~971区间中（含两端）的整数。

其余的9007199254740990个值（也就是2^53-2）也可以表示为：

> s * m * 2^e

其中，s为+1或者-1，m为小于2^52的正整数，e为-1074。

请注意，所有绝对值小于等于2^53的整数都可以表示为Number类型（整数0甚至可以表示为+0和-0）。

非零的有限数值有奇数个有效数字，表示为整数m为奇数。否则，为偶数。

在本规范中，短语“x的数值”中的x表示一个非零实数（可能是无理数如PI）就意味着按照下面的方式选取这个数值。考虑Number类型的所有的有限值的集合，去掉-0，增加两个额外的值（它们不能用Number类型所表示），也就是说2^1024（即+1 * 2^53 * 2^971）和-2^1024（即-1 * 2^53 * 2^971）。在这些数值的集合中选择一个最接近x的值。如果集合中有两个值都同等接近，则选择有偶数个有效数字的数值；出于这个目的，两个确切的数值2^1024和-2^1024看作为偶数个有效数字。最后，如果选择了2^1024，则用+∞代替；如果选择了-2^1024，则用-∞代替；如果选择了+0，当且仅当x小于零时，则用-0代替；其他情况下选择的值不做变化。结果就得到了x的数值。（该过程也正好是IEEE 754中“就近取整”模式的行为。）

有一些ECMAScript操作仅仅处理在-2^31~2^31-1区间中（含两端）的整数，或在0~2^32-1区间中（含两端）的整数。这些操作接受Number类型的任何数值，但它们首先将每个值都转换为2^32个数值中的一个。可分别参考9.5和9.6章节的ToInt32和ToUnit32操作。

# 8.6 Object类型

Object是属性的一个集合。一个属性可能是一个数据属性，或者是一个访问器，或者是内部属性：

* 数据属性与一个ECMAScript语言值名字以及一组Boolean特征值相关联。
* 访问器属性与一个或多个访问器函数名字以及一组Boolean特征值相关联。访问器函数存放着或者获取一个与之关联的ECMAScript语言值。
* 内部属性没有名字，也不能通过ECMAScript语言操作符直接访问到。内部属性纯粹是为了技术规范说明目的而存在的。

属性（非内部）的访问方式有两种： *get* 和 *put* ，分别对应于获取和赋值两个操作。

## 8.6.1 属性的特征值

特征值在本规范中是用来定义和解释属性的所处的状态。数据属性关联的一些特征值如下表5。

<table>
<caption>表5——数据属性的特征值</caption>
<tr><th>特征名</th><th>值域</th><th>描述</th></tr>
<tr><td>[[Value]]</td><td>任何ECMAScript语言类型</td><td>读取属性来获取该值。</td></tr>
<tr><td>[[Writable]]</td><td>Boolean</td><td>如果是false：使用ECMAScript代码通过[[Put]]修改属性的[[Value]]将会失败。</td></tr>
<tr><td>[[Enumerable]]</td><td>Boolean</td><td>如果是true：该属性可以通过for-in枚举到。否则，它不可枚举。</td></tr>
<tr><td>[[Configurable]]</td><td>Boolean</td><td>如果是false：试图删除该属性，或者改变属性为访问器属性，或者修改它的特征值都将失败。不影响修改它的[[Value]]。</td></tr>
</table>

访问器属性关联的一些特征值如下表6。

<table> 
<caption>表6——访问器属性的特征值</caption>
<tr><th>特征名</th><th>值域</th><th>描述</th></tr>
<tr><td>[[Get]]</td><td>对象或者undefined</td><td>如果该值是对象，必须为Function对象。每次该属性上操作一次get访问，该函数的内部方法[[Call]]就会被调用一次（参数列表为空），它的返回值就是该属性值。</td></tr>
<tr><td>[[Set]]</td><td>对象或者undefined</td><td>如果该值是对象，必须为Function对象。每次该属性上操作一次set访问，该函数的内部方法[[Call]]就会被调用一次，参数列表就是唯一用来赋予的值。该内部方法“可能但不一定”对该属性的[[Get]]内部方法返回的值造成影响。</td></tr>
<tr><td>[[Enumerable]]</td><td>Boolean</td><td>如果是true：该属性可以通过for-in枚举到。否则，它不可枚举。</td></tr>
<tr><td>[[Configurable]]</td><td>Boolean</td><td>如果是false：试图删除该属性，或者改变属性为数据属性，或者修改它的特征值都将失败。</td></tr>
</table>

如果一个特征值没有在本规范中显式地指定，表7中给出了默认的特征值定义。

<table>
<caption>表7——默认特征值</caption>
<tr><th>特征名</th><th>默认值</th></tr>
<tr><td>[[Value]]</td><td>undefined</td></tr>
<tr><td>[[Get]]</td><td>undefined</td></tr>
<tr><td>[[Set]]</td><td>undefined</td></tr>
<tr><td>[[Writable]]</td><td>false</td></tr>
<tr><td>[[Enumerable]]</td><td>false</td></tr>
<tr><td>[[Configurable]]</td><td>false</td></tr>
</table>

## 8.6.2 对象的内部属性和方法

本规范使用了各种各样的内部属性来定义对象值的语义。这些内部属性并不是ECMAScript语言的一部分。它们在本规范中定义纯粹是为了说明的目的。ECMAScript的一种实现的行为必须同这里描述相一致的方式来产生和操作这些内部属性。这里的内部属性用双方括号“[[ ]]”包围表示。每当一个算法需要使用一个对象的内部属性时，如果该对象没有实现该内部属性，程序将抛出一个 **TypeError** 异常。

表8总结了本规范中使用的内部属性，它们在所有ECMAScript对象中得到应用。表9总结的内部属性在本规范使用，但只在某些ECMAScript对象中得到应用。这些表格中的描述是用来说明原生ECMAScript对象的行为，除非特别说明，否则针对特殊类型的原生ECMAScript对象的说明限定在本文档中。

宿主对象也可能支持这些内部属性，前提条件是它的依赖实现的行为和本文档中陈述的宿主对象限制规范相一致。

下表中的“值类型域”这一列定义了与内部属性相关的值得类型。类型名指的就是第8章中定义的类型，再加下面一些额外的名字。“any”表示可以是任何ECMAScript语言类型。“primitive”表示 Undefined、Null、Boolean、String或者Number。“SpecOp”表示该内部属性是一个内部方法，它由一个实现所产出，由一个抽象操作规范所定义。“SpecOp”紧跟其后的是描述性形参名。如果形参名和一个类型名相同，那么这个参数就是这个类型。如果一个“SpecOp”有一个返回值，参数在符号“→”后面列出，以及返回值的类型。

<table>
<caption>表8——对所有对象通用的内部属性</caption>
<tr><th>内部属性</th><th>值类型域</th><th>描述</th></tr>
<tr><td>[[Prototype]]</td><td>对象或者null</td><td>该对象的属性。</td></tr>
<tr><td>[[Class]]</td><td>String</td><td>表示对象分类具体定义的一个字符串值。</td></tr>
<tr><td>[[Extensible]]</td><td>Boolean</td><td>如果是true：自有属性可以添加对象上。</td></tr>
<tr><td>[[Get]]</td><td>SpecOp(propertyName) → any</td><td>返回对应名字的属性值。</td></tr>
<tr><td>[[GetOwnProperty]]</td><td>SpecOp (propertyName) → undefined或者属性描述符</td><td>返回该对象上“自有”属性中对应名字的属性描述符，如果不存在就返回undefined。</td></tr>
<tr><td>[[GetProperty]]</td><td>SpecOp (propertyName) → undefined或者属性描述符</td><td>返回该对象上所有“根植”属性中对应名字的属性描述符，如果不存在就返回undefined。</td></tr>
<tr><td>[[Put]]</td><td>SpecOp (propertyName, any, Boolean)</td><td>将对应名字属性的值设置为第二个参数的值。标记位控制了失败处理句柄。</td></tr>
<tr><td>[[CanPut]]</td><td>SpecOp (propertyName) → Boolean</td><td>返回一个Boolean值，表示对应名字的属性上是否可以进行[[Put]]操作。</td></tr>
<tr><td>[[HasProperty]]</td><td>SpecOp (propertyName) → Boolean</td><td>返回一个Boolean值，表示给定名字的属性是否在对象中存在。</td></tr>
<tr><td>[[Delete]]</td><td>SpecOp (propertyName, Boolean) → Boolean</td><td>从对象中删除指定名字的自有属性。标记位控制了失败处理句柄。</td></tr>
<tr><td>[[DefaultValue]]</td><td>SpecOp (Hint) → primitive</td><td>Hint是一个字符串。为对象返回默认值。</td></tr>
<tr><td>[[DefineOwnProperty]]</td><td>SpecOp (propertyName, PropertyDescriptor, Boolean) → Boolean</td><td>创建或修改对应名字的自有属性，从而拥有对应属性描述符表示的状态。标记位控制了失败处理句柄。</td></tr>
</table>

# 8.7 引用类型


## 8.7.1 GetValue(V)
## 8.7.2 PutValue(V,W)


# 8.8 列表规范类型


# 8.9 自动规范类型


# 8.10 属性描述符和属性标记符规范类型


## 8.10.1 IsAccessorDescriptor(Desc)
## 8.10.2 IsDataDescriptor(Desc)
## 8.10.3 IsGenericDescriptor(Desc)
## 8.10.4 FromPropertyDescriptor(Desc)
## 8.10.5 ToPropertyDescriptor(Obj)


# 8.11 词法环境和环境记录规范类型


# 8.12 Object内部方法的算法


## 8.12.1 \[[GetOwnProperty]](P)
## 8.12.2 \[[GetProperty]](P)
## 8.12.3 \[[Get]](P)
## 8.12.4 \[[CanPut]](P)
## 8.12.5 \[[Put]](P, V, Throw)
## 8.12.6 \[[HasProperty]](P)
## 8.12.7 \[[Delete]](P, Throw)
## 8.12.8 \[[DefaultValue]](hint)
## 8.12.9 \[[DefineOwnProperty]](P, Desc, Throw)
