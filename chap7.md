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

注释有的是单行，有的是多行。多行的注释不能嵌套。

单行注释可以包含除了 *LineTerminator* 以外的任何字符，一个令牌的一般性规则总是尽可能的符合要求，单行注释是由“//”标记开始到该行结束之间的所有字符构成。因此，该行结束的 *LineTerminator* 并不属于该单行注释的一部分；它只被看作词法中的分隔，是句法输入元素流中的一部分而已。这一点非常重要，因为它暗示了单行注释存在与否都不影响分号自动插入的处理流程（见7.9）。

注释的行为就像空白符，会被直接丢弃。不过如果 *MultiLineComment* 包含一个行终止符，那么为了被句法解析，整个注释块被看作是一个 *LineTerminator* 。

**语法**

> *Comment* ::  
>
>> *MultiLineComment* 
>>
>> *SingleLineComment* 

> *MultiLineComment* ::  
>
>> /\* *MultiLineCommentChars* <sub>opt</sub> \*/ 

> *MultiLineCommentChars* ::  
>
>> *MultiLineNotAsteriskChar* *MultiLineCommentChars* <sub>opt</sub>
>> 
>> \* *PostAsteriskCommentChars* <sub>opt</sub>
 
> *PostAsteriskCommentChars* ::  
>
>> *MultiLineNotForwardSlashOrAsteriskChar* *MultiLineCommentChars* <sub>opt</sub>
>>
>> \* *PostAsteriskCommentChars* <sub>opt</sub> 

> *MultiLineNotAsteriskChar* ::  
>
>> *SourceCharacter* **but not** *asterisk*  \* 

> *MultiLineNotForwardSlashOrAsteriskChar* ::  
>
>> *SourceCharacter*  **but not** *forward-slash* / **or**  *asterisk*  \* 

> *SingleLineComment* ::  
>
>> // *SingleLineCommentChars* <sub>opt</sub>
 
> SingleLineCommentChars  ::  
>
>> *SingleLineCommentChar* *SingleLineCommentChars* <sub>opt</sub> 

> *SingleLineCommentChar* ::  
>
>> *SourceCharacter*   **but not** *LineTerminator*

## 7.5 令牌

**语法**
  
> *Token*  ::  
>
>> *IdentifierName* 
>>
>> *Punctuator* 
>>
>> *NumericLiteral* 
>>
>> *StringLiteral*

> 注： *DivPunctuator* 和 *RegularExpressionLiteral* 部件定义了令牌，但是它们没有包含在 *Token* 部件中。

## 7.6 标记符名称和标记符

标记符名称是按照Unicode标准第5章的“标记符”小节给出的语法（有一些微小的修改）翻译出来的令牌。 *Identifier* 就是非 *ReservedWord* （见7.6.1）的 *IdentifierName* 。Unicode标记符语法是基于Unicode标准指定的规范和信息字符类目。这些字符在Unicode标准版本3.0的指定类目中，它们必须被所有遵守ECMAScript的实现所视作出自这些类目。

本规范指定的特殊字符还允许有：美元符（$）和下划线（_）可以在 *IdentifierName* 中的任何位置。

*IdentifierName* 中还允许有Unicode转义字符序列，它们只是形成 *IdentifierName* 中的单个字符， *UnicodeEscapeSequence* 计算为字符值（见7.8.4）。 *UnicodeEscapeSequence* 前面的“\”不是该 *IdentifierName* 的一部分。如果 *UnicodeEscapeSequence* 是非法字符，就不能将其放入 *IdentifierName* 中。也就是说，如果 \ *UnicodeEscapeSequence* 序列用 *UnicodeEscapeSequence* 的字符值替换以后的结果仍然应该是有效的 *IdentifierName* ，前后两个 *IdentifierName* 应该是完全一样的。本规范中对所有标记符的解释都是基于它们实际的字符串而不管是否有转义序列来代替某个特别的字符。

按照Unicode标准两个完全相等的 *IdentifierName* 如果表示它们的编码单元序列完全相同才相等（也就是说，所有遵守ECMAScript的实现只被要求进行标记符名称的按位比较）。意图就是进来的源码文本在到达编译器之前已经被转换成常规化表格C。

ECMAScript实现可能能识别定义在较新本班的Unicode标准中的标记字符。如果需要考虑可移植性，程序员应该只使用定义在Unicode3.0中的标记字符。

**语法**

> *Identifier*  ::  
>
>> *IdentifierName* **but not** *ReservedWord* 

> *IdentifierName* ::  
>
>> *IdentifierStart* 
>>
>> *IdentifierName* *IdentifierPart*

> *IdentifierStart*  ::  
>
>> *UnicodeLetter* 
>>
>> $ 
>>
>> _ 
>>
>> \ *UnicodeEscapeSequence* 

> *IdentifierPart*  ::  
>
>> *IdentifierStart* 
>>
>> *UnicodeCombiningMark* 
>>
>> *UnicodeDigit* 
>>
>> *UnicodeConnectorPunctuation* 
>>
>> &lt;ZWNJ&gt; 
>>
>> &lt;ZWJ&gt; 

> *UnicodeLetter* 
>
>> 在Unicode类目“大写字母（Lu）”、“小写字母（Ll）”、“标题大写字母（Lt）”、“修正符字母（Lm）”、“其他字母（Lo）”或“数字字母（Nl）”中的所有字符。

> *UnicodeCombiningMark* 
>
>> 在Unicode类目“非空格标记（Mn）”或“组合间距标记（Mc）”中的所有字符。 

> *UnicodeDigit* 
>
>> 在Unicode类目“十进制数（Nd）”中的所有字符。

> *UnicodeConnectorPunctuation* 
>
>> 在Unicode类目“连接标点符（Pc）”中的所有字符。 

> *UnicodeEscapeSequence* 
>
>> 见7.8.4。

### 7.6.1 保留字

保留字是不能用作 *Identifier* 的 *IdentifierName* 。

**语法**

> *ReservedWord*  ::  
>
>> *Keyword* 
>>
>> *FutureReservedWord* 
>>
>> *NullLiteral* 
>>
>> *BooleanLiteral*

### 7.6.1.1 关键字

以下单词是ECMAScript关键字，在ECMAScript程序中可能不会用作 *Identifiers* 。

**语法**

> *Keyword*  ::  **one of** 
<table>
<tr><td>break</td><td>do</td><td>instanceof</td><td>typeof</td></tr>
<tr><td>case</td><td>else</td><td>new</td><td>var</td></tr>
<tr><td>catch</td><td>finally</td><td>return</td><td>void</td></tr>
<tr><td>continue</td><td>for</td><td>switch</td><td>while</td></tr>
<tr><td>debugger</td><td>function</td><td>this</td><td>with</td></tr>
<tr><td>default</td><td>if</td><td>throw</td></tr>
<tr><td>delete</td><td>in</td><td>try</td></tr>
</table>

### 7.6.1.2 未来保留字

以下单词作为关键词中提出的扩展，因此保留，以便未来通过这些扩展的可能性。

**语法**
 
> *FutureReservedWord*  ::  **one of**  
<table>
<tr><td>class</td><td>enum</td><td>extends</td><td>super </td></tr>
<tr><td>const</td><td>export</td><td>import</td></tr>
</table>

以下关键词如果出现在strict模式的代码中时将被看作是 *FutureReservedWords* 。在任何上下文的strict模式代码中任何 
*FutureReservedWords* 出现会产生一个错误的地方出现以下这些令牌时也必须产生一个同等的错误：
<table>
<tr><td>implements</td><td>let</td><td>private</td><td>public</td><td>yield </td></tr>
<tr><td>interface</td><td>package</td><td>protected</td><td>static</td></tr>
</table>

## 7.7 标点符号

**语法**

> *Punctuator*  ::  **one of**  
<table>
<tr><td>{</td><td>}</td><td>(</td><td>)</td><td>[</td><td>]</td></tr>
<tr><td>.</td><td>;</td><td>,</td><td>&lt;</td><td>&gt;</td><td>&lt;=</td></tr>
<tr><td>&gt;=</td><td>==</td><td>!=</td><td>===</td><td>!==</td></tr>
<tr><td>+</td><td>-</td><td>*</td><td>%</td><td>++</td><td>--</td></tr>
<tr><td>&lt;&lt;</td><td>&gt;&gt;</td><td>&gt;&gt;&gt;</td><td>&</td><td>|</td><td>^</td></tr>
<tr><td>!</td><td>~</td><td>&&</td><td>||</td><td>?</td><td>:</td></tr>
<tr><td>=</td><td>+=</td><td>-=</td><td>*=</td><td>%=</td><td>&lt;&lt;=</td></tr>
<tr><td>&gt;&gt;=</td><td>&gt;&gt;&gt;=</td><td>&=</td><td>|=</td><td>^=</td></tr>
</table>

> *DivPunctuator* ::  **one of**  
<table>
<tr><td>/</td><td>/=</td></tr>
</table>

## 7.8 直接量

**语法**

> *Literal*  ::  
>
>> *NullLiteral* 
>>
>> *BooleanLiteral* 
>>
>> *NumericLiteral* 
>>
>> *StringLiteral*  
>>
>> *RegularExpressionLiteral* 

### 7.8.1 Null直接量

**语法**

> *NullLiteral*  ::  
>
>> null

**语义**

空直接量值 **null** 是Null类型唯一一个值，即 **null** 。

### 7.8.2 Boolean直接量

**语法**

> *BooleanLiteral* ::  
>
>> true 
>>
>> false

**语义**

Boolean直接量 **true** 是Boolean类型值，即 **true** 。
Boolean直接量 **false** 是Boolean类型值，即 **false** 。

### 7.8.3 Number直接量

**语法**

> *NumericLiteral* ::  
>
>> *DecimalLiteral* 
>>
>> *HexIntegerLiteral* 

> *DecimalLiteral* ::  
>
>> *DecimalIntegerLiteral* . *DecimalDigits* <sub>opt</sub>  *ExponentPart* <sub>opt</sub>
>>
>> . *DecimalDigits* *ExponentPart* <sub>opt</sub>
>>
>> *DecimalIntegerLiteral* *ExponentPart* <sub>opt</sub>
 
> *DecimalIntegerLiteral*  ::  
>
>> 0 
>>
>> *NonZeroDigit* *DecimalDigits* <sub>opt</sub> 

> *DecimalDigits* ::  
>
>> *DecimalDigit* 
>>
>> *DecimalDigits* *DecimalDigit* 

> *DecimalDigit* ::  **one of**  
>
>> 0  1  2  3  4  5  6  7  8  9

> *NonZeroDigit*  ::  **one of**  
>
>> 1  2  3  4  5  6  7  8  9 

> *ExponentPart*  ::  
>
>> *ExponentIndicator* *SignedInteger* 

> *ExponentIndicator*  ::  **one of** 
>
>> e  E

> *SignedInteger*  ::  
>
>> *DecimalDigits* 
>>
>> + *DecimalDigits* 
>>
>> - *DecimalDigits* 

> *HexIntegerLiteral* ::  
>
>> 0x *HexDigit* 
>>
>> 0X *HexDigit* 
>>
>> *HexIntegerLiteral* *HexDigit*

> *HexDigit*  ::  **one of**    
>
>> 0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f  A  B  C  D  E  F

紧跟在一个 *NumericLiteral* 之后的源码字符必须不是 *IdentifierStart* 或 *DecimalDigit* 。

> 注：例如，
>
>> 3in
>
> 错误，并不是两个输入元素3和in。

**语义**

数字直接量表示Number类型的值。分两步决定该值：首先，从字面上推断一个数值（MV）；然后，根据以下规则处理数值：

- *NumericLiteral* :: *DecimalLiteral* 的MV就是 *DecimalLiteral* 的MV。
- *NumericLiteral* :: *HexIntegerLiteral* 的MV就是 *HexIntegerLiteral* 的MV。
- *DecimalLiteral* :: *DecimalIntegerLiteral* . 的MV就是 *DecimalIntegerLiteral* 的MV。
- *DecimalLiteral* :: *DecimalIntegerLiteral* . *DecimalDigits* 的MV就是 *DecimalIntegerLiteral* 的MV加上 *DecimalDigits* MV的 10<sup>-n</sup>倍， n 是 *DecimalDigits* 的字符数。
- *DecimalLiteral* :: *DecimalIntegerLiteral* . *ExponentPart* 的MV就是 *DecimalIntegerLiteral* MV的 10<sup>e</sup>倍， e 就是 *ExponentPart* 的MV。
- *DecimalLiteral* :: *DecimalIntegerLiteral* . *DecimalDigits* *ExponentPart* 的MV就是 *DecimalIntegerLiteral* 的MV加上 *DecimalDigits* MV的 10<sup>-n</sup>倍，然后再10<sup>e</sup>倍，其中 n 是 *DecimalDigits* 的字符数，e 就是 *ExponentPart* 的MV。
- *DecimalLiteral* :: . *DecimalDigits* 的MV就是 *DecimalDigits* MV的 10<sup>-n</sup>倍，n 是 *DecimalDigits* 的字符数。
- *DecimalLiteral* :: . *DecimalDigits* *ExponentPart* 的MV就是 DecimalDigits MV的10<sup>e-n</sup>倍，其中 n 是 *DecimalDigits* 的字符数，e 就是 *ExponentPart* 的MV。
- *DecimalLiteral* :: *DecimalIntegerLiteral* 的MV就是 *DecimalIntegerLiteral* 的MV。
- *DecimalLiteral* :: *DecimalIntegerLiteral* *ExponentPart* 的MV就是 DecimalIntegerLiteral MV的10<sup>e</sup>倍，e 就是 *ExponentPart* 的MV。
- *DecimalIntegerLiteral* :: 0 的MV是 0。
- *DecimalIntegerLiteral* :: *NonZeroDigit*  *DecimalDigits* 的MV就是 *NonZeroDigit* MV的10<sup>n</sup>倍，加上 *DecimalDigits* 的MV，n 就是 *DecimalDigits* 的字符数。
- *DecimalDigits* :: *DecimalDigit* 的MV就是 *DecimalDigit* 的MV。
- *DecimalDigits* :: *DecimalDigits*  *DecimalDigit* 的MV就是 DecimalDigits MV的10倍，加上 *DecimalDigit* 的MV。
- *ExponentPart* :: *ExponentIndicator* *SignedInteger* 的MV就是 *SignedInteger* 的MV值。
- *SignedInteger* :: *DecimalDigits* 的MV就是 *DecimalDigits* 的MV。
- *SignedInteger* :: + *DecimalDigits* 的MV就是 *DecimalDigits* 的MV。
- *SignedInteger* :: - *DecimalDigits* 的MV就是 *DecimalDigits* MV的负数。
- *DecimalDigit* :: 0 或者 *HexDigit* :: 0 的MV是 0。
- *DecimalDigit* :: 1 或者 *NonZeroDigit* :: 1 或者 *HexDigit* :: 1 的MV是 1。
- *DecimalDigit* :: 2 或者 *NonZeroDigit* :: 2 或者 *HexDigit* :: 2 的MV是 2。
- *DecimalDigit* :: 3 或者 *NonZeroDigit* :: 3 或者 *HexDigit* :: 3 的MV是 3。
- *DecimalDigit* :: 4 或者 *NonZeroDigit* :: 4 或者 *HexDigit* :: 4 的MV是 4。
- *DecimalDigit* :: 5 或者 *NonZeroDigit* :: 5 或者 *HexDigit* :: 5 的MV是 5。
- *DecimalDigit* :: 6 或者 *NonZeroDigit* :: 6 或者 *HexDigit* :: 6 的MV是 6。
- *DecimalDigit* :: 7 或者 *NonZeroDigit* :: 7 或者 *HexDigit* :: 7 的MV是 7。
- *DecimalDigit* :: 8 或者 *NonZeroDigit* :: 8 或者 *HexDigit* :: 8 的MV是 8。
- *DecimalDigit* :: 9 或者 *NonZeroDigit* :: 9 或者 *HexDigit* :: 9 的MV是 9。
- *HexDigit* :: a 或者 *HexDigit* :: A 的MV是 10。
- *HexDigit* :: b 或者 *HexDigit* :: B 的MV是 11。
- *HexDigit* :: c 或者 *HexDigit* :: C 的MV是 12。
- *HexDigit* :: d 或者 *HexDigit* :: D 的MV是 13。
- *HexDigit* :: e 或者 *HexDigit* :: E 的MV是 14。
- *HexDigit* :: f 或者 *HexDigit* :: F 的MV是 15。
- *HexIntegerLiteral* :: 0x *HexDigit* 的MV就是 *HexDigit* 的MV。
- *HexIntegerLiteral* :: 0X *HexDigit* 的MV就是 *HexDigit* 的MV。
- *HexIntegerLiteral* :: *HexIntegerLiteral*  *HexDigit* 的MV就是 *HexIntegerLiteral* MV的16倍，加上 *HexDigit* 的MV。

一旦数字直接量确切的MV确定下来以后，就将其处理为一个Number类型的数值。如果MV是0，结果即为+0；否则，结果必须为该MV的Number数值（见8.5），除非是 *DecimalLiteral* 直接量，并且它的长度超过了20个有效数字，该Number数值可能是第20位后面的数字全被替换成0，或者第20位后面的数字全被替换成0并且将第20位有效数字加1。判断某个数字是否有效，它不该属于 *ExponentPart* 一部分，并且满足：

- 不为0
- 它左边有非零数字，右边有非零的不属于 *ExponentPart* 一部分的数字。

一个遵守规范的实现，在处理strict模式代码时（见10.1.1），它必须如B.1.1所描述的那样不能将 *NumericLiteral* 的语法包含 *OctalIntegerLiteral* 。

### 7.8.4 String直接量

### 7.8.5 RegExp直接量

## 7.9 分号自动插入

某些ECMAScript语句（空语句，变量语句，表达式语句， **do-while** 语句， **continue** 语句， **break** 语句， **return** 语句， **throw** 语句）必须以分号终止。分号可能总是显示地在源代码文本中出现的。尽管如此，在某些场合为了方便，分号可以没有。这些场合就是被描述为所谓的分号自动插入到源代码令牌流中的那些情况。

### 7.9.1 分号自动插入的规则

有三个基本的分号插入的规则：

1. 程序自左向右解析，遇到一个令牌（叫作冒犯令牌）【译注：无效令牌】任何部件的语法都不允许的时候，那么在该无效令牌前自动插入一个分号。要满足以下两个条件之一为真（true）：
	- 该无效令牌和前面的令牌之间至少有一个 *LineTerminator* 隔开。
	- 该无效令牌是“}”。
2. 程序自左向右解析，令牌输入流结束，解析器遇到的并非是一个完整的ECMAScript程序，那么在输入流结束自动插入一个分号。
3. 程序自左向右解析，遇到一个令牌在某些部件的语法中是允许的，但该部件恰好是一个受限的部件，在其中该令牌（因此这样的令牌叫受限的令牌）如果是第一个紧随“[no *LineTerminator* here]”之后的为了终结或非终结的令牌，并且该受限的令牌和前面的令牌之间至少有一个 *LineTerminator* 隔开，那么在该受限令牌之间自动插入一个分号。

但是，还有一个额外的覆盖性条件凌驾于处理规则之上：如果要插入的分号将被解析为一个空语句或者该分号可能成为 **for** 语句（见12.6.3）的前两个分号之一，那么该分号不会被自动插入。

> 注：以下是仅有语法中的受限部件
>
> *PostfixExpression* : 
>
>> *LeftHandSideExpression*  [no  *LineTerminator* here]  ++ 
>>
>> *LeftHandSideExpression*   [no  *LineTerminator* here]  --

> ContinueStatement  : 
>
>> **continue** [no  *LineTerminator* here]  *Identifier* <sub>opt</sub>; 

> BreakStatement : 
>
>> **break**  [no  *LineTerminator* here]  *Identifier* <sub>opt</sub>; 

> *ReturnStatement*  : 
>
>> **return** [ no *LineTerminator* here]  *Expression* <sub>opt</sub>; 

> ThrowStatement  : 
>
>> *throw*  [no  *LineTerminator* here]  *Expression*  ; 

这受限部件的实际影响如下：

当遇到一个 ++ 或 -- 令牌，解析器可能将其看作为后缀操作符。在前面的令牌与该 ++ 或 -- 令牌中间遇到至少一个 *LineTerminator* ，那么分号自动插入到 ++ 或 -- 令牌之前。

当一个 **continue** 、 **break** 、 **return** 或 **throw** 令牌遇到下个令牌之前就遇到一个 *LineTerminator* ，那么分号自动插入到上述令牌之后。

针对ECMAScript程序员的最佳实践是：

- 后缀 ++ 或 -- 操作符必须和操作数在同一行。
- 在一个 **return** 或 **throw** 语句中的 *Expression* 必须与该 **return** 或 **throw** 令牌在同一行。
- 在一个 **break** 或 **continue** 语句中个 *Identifier* 必须与该 **break** 或 **continue** 令牌在同一行。

### 7.9.2 分号自动插入的例子

源代码：

	{ 1 2 } 3

在ECMAScript语法中不是有效的句子，甚至在分号自动插入以后也是无效的。对比，以下源代码：

	{ 1 
	2 } 3

也是一个无效的ECMAScript句子，但是经过分号自动插入的处理，如下：

	{ 1 
	;2 ;} 3;

它就转换成了一个有效的ECMAScript句子。

源代码：

	for (a; b 
	)

它是一个无效的ECMAScript句子，但是分号自动插入不修改这个句子，因为分号在 for 语句中的头部是有其用处的。分号自动插入不会给 for 语句的头部插入它所需的两个分号的任何一个。

源代码：

	return
	a + b

它经过分号自动插入的转换，如下：

	return;
	a + b;

> 注：表达式 a + b 不会被 return 语句看作是需要返回的值，因为在 **return** 之后有一个 *LineTerminator* 将两者隔开了。

源代码：

	a = b
	++c

它经过分号自动插入的转换，如下：

	a = b;
	++c;

> 注：令牌 ++ 没有被看作是变量 b 的后缀操作符，因为 b 和 ++ 之间有一个 *LineTerminator* 将两者隔开了。

源代码：

	if (a > b)
	else c = d

它是一个无效的ECMAScript句子，但是分号自动插入不修改这个句子，在这一点上没有语法部件可以应用得上，因为分号自动插入将会变成一个空语句，所以不能插入。

源代码：

	a = b + c
	(d + e).print()

分号自动插入不会修改这个句子，因为第二行开始的圆括号表达式可以被解析为 c 这个函数的参数：

	a = b + c(d + e).print()

在有以左圆括号开始的赋值语句的情况下，最好在前面的语句的结束显式地书写一个分号，而不是寄希望于分号自动插入。
