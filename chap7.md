# 7 词法约定

一个ECMAScript程序的源码文本首先转换为一些输入元素序列——令牌、行终止符、注释或者空白。从左到右地扫描源码文本，选取最长可能为字符序列作为下一个输入元素，并重复这个操作。

词法有两个目标符号。 *InputElementDiv* 用于句法上下文，其中以除号（/）开头或者除号赋值（/=）操作符也是允许的。 *InputElementRegExp* 用于其他的句法上下文。

> 注：没有同时允许以除号（/）开头或者除号赋值（/=）和以 *RegularExpressionLiteral* 开头的情况。这里不会受到 xx 分号自动插入（见7.9）的影响；例如下面的例子：
>
>>	a = b
>> 
>>	/hi/g.exec(c).map(d);

在一个 *LineTerminator* 后面的非空白、非注释的首字符斜杠（/），句法上下文允许除号或除号赋值，分号不会插入在 *LineTerminator* 后。也就是说，上面的例子翻译以后如下：

> a = b / hi / g.exec(c).map(d);

**语法**

> *InputElementDiv* ::  
>
>> *WhiteSpace* 
>>
>> *LineTerminator* 
>>
>> *Comment* 
>>
>> *Token* 
>>
>> *DivPunctuator* 

> *InputElementRegExp*  ::  
>
>> *WhiteSpace* 
>>
>> *LineTerminator* 
>>
>> *Comment* 
>>
>> *Token* 
>>
>> *RegularExpressionLiteral* 

## 7.1 Unicode格式控制符

Unicode格式控制符（也就是，在Unicode字符数据库的“Cf”类目下的字符，如LEFT-TO-RIGHT标记或RIGHT-TO-LEFT标记）是当缺失高层协议时（如标记语言）用于控制文本区域格式的控制代码。

允许格式控制符在源码文本中方便的编辑和显示是非常有用的。所有的格式控制符可能在注释、字符串直接量和正则表达式直接量中使用。

在某些语言中，&lt;ZWNJ&gt; 和 &lt;ZWJ&gt; 用于在构建单词或短语时做必要的分隔。在ECMAScript源码文本中，&lt;ZWNJ&gt; 和 &lt;ZWJ&gt; 可能用于首字符后面的一个标识中。

&lt;BOM&gt; 主要用于文本开头标识它是Unicode，并允许文本编码和字节序的检测。为此目的的&lt;BOM&gt; 字符有时出现在文本的开头，例如连接合并文件。&lt;BOM&gt; 字符应该被当作空白字符处理（见7.2）。

格式控制符在注释、字符串直接量和正则表达式直接量之外的特殊处理，如下表1。

<table>
<caption>表1——格式控制符用法</caption>
<tr><th>代码单元值</th><th>名称</th><th>正式名称</th><th>用法</th></tr>
<tr><td>\u200C</td><td>零宽非连接符</td><td>&lt;ZWNJ&gt;</td><td> <i>IdentifierPart</i> </td></tr>
<tr><td>\u200C</td><td>零宽连接符</td><td>&lt;ZWJ&gt;</td><td> <i>IdentifierPart</i> </td></tr>
<tr><td>\uFEFF</td><td>字节序标记</td><td>&lt;BOM&gt;</td><td> <i>Whitespace</i> </td></tr>
</table>

## 7.2 空白符

空白是用来改善源码文本的可读性的，同时是令牌（单独的词法单元）之间的分隔，但并不是那么重要。空白字符可能出现在任何两个令牌中间，输入的开头或结尾。空白符也可能出现在一个 *StringLiteral* 或者一个 *RegularExpressionLiteral* （被认为是该直接量值中重要的字符，是它的一部分）或者一个 *Comment* 中，但绝对不能出现在其它的令牌中。

表2列出了ECMAScript中的空白符。

<table>
<caption>表2——空白符</caption>
<tr><th>代码单元值</th><th>名称</th><th>正式名称</th></tr>
<tr><td>\u0009</td><td>Tab</td><td>&lt;TAB&gt;</td></tr>
<tr><td>\u000B</td><td>垂直Tab</td><td>&lt;VT&gt;</td></tr>
<tr><td>\u000C</td><td>分页</td><td>&lt;FF&gt;</td></tr>
<tr><td>\u0020</td><td>空格</td><td>&lt;SP&gt;</td></tr>
<tr><td>\u00A0</td><td>非断行空格</td><td>&lt;NBSP&gt;</td></tr>
<tr><td>\uFEFF</td><td>字节序标记</td><td>&lt;BOM&gt;</td></tr>
<tr><td>“Zs”类目下其他</td><td>任何其他“空白分隔”</td><td>&lt;USP&gt;</td></tr>
</table>

ECMAScript实现必须识别定义在Unicode3.0中所有的空白符。较新版本的Unicode标准可能定义了其它的空白符。ECMAScript实现可能能识别较新版本Unicode标准中的空白符。

**语法**

> *WhiteSpace* ::  
>
>> &lt;TAB&gt; 
>>
>> &lt;VT&gt; 
>>
>> &lt;FF&gt; 
>>
>> &lt;SP&gt; 
>>
>> &lt;NBSP&gt; 
>>
>> &lt;BOM&gt; 
>>
>> &lt;USP&gt;

## 7.3 行结束符

就想空白符，行终止符也是用来改善源码文本的可读性，同时是令牌（单独的词法单元）之间的分隔。但，不同的是行终止符对句法行为是有一些影响的。一般来说，行终止符可能出现在任何两个令牌之间，但还有一些少数的地方是句法禁止出现的。它同样会影响分号自动插入的处理（7.9）。一个行终止符不能出现在任何 *StringLiteral* 中间，它仅可能出作为一个 *LineContinuation* 的一部分出现在一个 *StringLiteral* 中。

行终止符可以出现在一个 *MultiLineComment* （7.4）中，但不能出现在一个 *SingleLineComment* 中。

行终止符也包括在正则表达式\s匹配的空白符的集合中。

表3列出了ECMAScript中的所有行终止符。

<table>
<caption>表2——空白符</caption>
<tr><th>代码单元值</th><th>名称</th><th>正式名称</th></tr>
<tr><td>\u000A</td><td>换行</td><td>&lt;LF&gt;</td></tr>
<tr><td>\u000D</td><td>回车</td><td>&lt;CR&gt;</td></tr>
<tr><td>\u2028</td><td>分行</td><td>&lt;LS&gt;</td></tr>
<tr><td>\u2029</td><td>分段</td><td>&lt;PS&gt;</td></tr>
</table>

仅当出现在表3中的字符才是行终止符。其他新行或行中断符都被看作是空白符而不是行终止符。字符序列&lt;CR&gt; &lt;LF&gt; 通常作为一个行终止符。在统计行数时应该只当作单个字符计算。

**语法**

> LineTerminator  ::  
>
>> &lt;LF&gt; 
>>
>> &lt;CR&gt; 
>>
>> &lt;LS&gt; 
>>
>> &lt;PS&gt; 

> LineTerminatorSequence ::  
>
>> &lt;LF&gt; 
>> 
>> &lt;CR&gt;  [lookahead  ∉  &lt;LF&gt;   ]  
>>
>> &lt;LS&gt; 
>>
>> &lt;PS&gt; 
>>
>> &lt;CR&gt; &lt;LF&gt;

## 7.4 注释

## 7.5 标记符片段

## 7.6 标记符名称和标记符

### 7.6.1 保留字

### 7.6.1.1 关键字

### 7.6.1.2 未来保留字

## 7.7 标点符号

## 7.8 直接量

### 7.8.1 Null直接量

### 7.8.2 Boolean直接量

### 7.8.3 Number直接量

### 7.8.4 String直接量

### 7.8.5 RegExp直接量

## 7.9 分号自动插入

### 7.9.1 分号自动插入的规则

### 7.9.2 分号自动插入的例子