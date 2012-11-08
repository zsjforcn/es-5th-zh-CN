# 5 标记约定

## 5.1 句子和词汇语法

### 5.1.1 上下文无关语法

每个“上下文无关”的语法是由若干部件构成。每个部件有一个叫做“非终结符”的抽象符号作为其左值，以及一系列零个或多个“非终结符”和“终结符”作为其右值。对每个语法，终结符来自一个指定的字母表。

从一个简单的句子入手，它由单个清晰的非终结符构成，叫做“目标符号”。给定的一个上下文无关的语法规定了一种语言，也就是说，使用一个右值反复地替换序列中的任何非终结符会产生出所有可能的终结符序列的集合（也许有无限多）。【译注：译文可能不正确】

### 5.1.2 词汇和正则语法

ECMAScript的词法在第7条中给出。该语法的终结符（ Unicode 代码单元）符合第6条中定义 *SourceCharacter* 的规则。它定义了一组部件，从目标符号 *InputElementDiv* 或 *InputElementRegExp* 入手，它描述了这些字符是如何翻译成一系列的输入元素的。

除了空白和注释以外输入元素形成了ECMAScript句法终结符，即所谓的ECMAScript令牌。这些令牌分别是保留字、标识符、直接量和ECMAScript语言中使用的标点符号。此外，行终止符虽然不是令牌，但也是输入元素流的一部分，它控制着分号自动插入的处理流程（7.9）。简单的空白和单行注释虽然有所描述，但并没有在句法的输入元素流中出现。 *MultiLineComment* （即“/\*...\*/”形式的注释无论占用一行还是多行都会被忽略），如果它没有包含行终止符，同样也只有简单的描述；但 *MultiLineComment* 如果包含一个或多个行终止符，它会被替换成一个简单的行终止符，这样就变成了句法输入元素流中一部分。

ECMAScript 的正则（ *RegExp* ）语法在第15.10中给出。该语法的终结符定义在 *SourceCharacter* 中。它定义了一组部件，从目标符号 *Pattern* 入手，它描述了这些字符是如何翻译成一系列的正则表达式模式的。

词法部件和正则语法的区分标点符号是两个冒号“::”。词法和正则语法也共享了一些部件。

### 5.1.3 数字字符串语法

将字符串翻译成数值使用了另外一套语法。这个语法类似词法处理数字直接量和它自己的 *SourceCharacter* 终结符。该语法在9.3.1中给出。

数字字符串语法部件的区分标点符号是三个冒号“:::”。

### 5.1.4 句子语法

ECMAScript的句法在第11、12、13和14条中给出。该语法是由词法定义的ECMAScript令牌作为其终结符（5.1.2）。它定义了一组部件，从目标符号 *Program* 入手，它描述了一系列的令牌是如何组成正确句法规则的ECMAScript程序。

当字符流解析为一个ECMAScript程序时，首先它被通过反复地应用词法转换为一个输入元素流；这个输入元素流被一个句法应用程序解析。如果输入元素流中的令牌都使用完毕还是无法解析为目标非终结 *Program* 的单个实例，程序是句法错误的。

句法部件的区分标点符号是一个冒号“:”。

在第11、12、13和14条中给出句法令牌序列可以被接受为正确的ECMAScript程序，但实际上并不完整。也就是说，某些额外的令牌序列也可以被接受。这些令牌序列仅当分号插入到序列中的某些位置（如行终止符之前）时将会被语法所描述。此外，语法描述的某些令牌序列，如果在某些“尴尬的”地方出现了行终止符，它们将被看作是不可接受的。

### 5.1.5 JSON语法

JSON语法是将描述一组ECMASCript对象集合的一个字符串翻译成实际的对象。JSON语法在第15.12.1条中给出。

JSON语法由JSON词法和句法组成。JSON词法用于将字符序列翻译成令牌，与部分ECMASCript词法类似。JSON句法描述了来自JSON词法的令牌序列是如何构成句法正确的JSON对象的。

JSON词法部件的区分标点符号是两个冒号“::”。它使用了ECMASCript词法中的一些部件。JSON句法与部分 ECMASCript句法类似，它的部件的区分标点符号是一个冒号“:”。

### 5.1.6 语法符号

词法与字符串的终结符，以及一些句法的终结符，用 **等宽** 的字体显示，在语法部件和本规范中只要文本中直接引用了这样的终结符都如此显示。这些确实出现在一个程序中。按照这种方式指定的所有终结符被理解为ASCII码段中合适的Unicode字符，而不是其他Unicode码段中看上去类似的字符。

非终结符用 *斜体* 显示。一个非终结符的定义是以一个名字紧跟一个或多个冒号开始。（冒号的多少取决于属于哪个语法。）紧随其后的几行，可以为非终结符选择一个或多个右值。一个例子：

> *WhileStatement* : 
>
>> **while** ( *Expression* ) *Statement*

例子说明非终结符 *WhileStatement* 表示了令牌 **while** 跟着一个左圆括号、一个 *Expression* 、一个右圆括号、一个 *Statement* 。上例中出现的 *Expression* 和 *Statement* 本身也是非终结符。另一个例子：

> *ArgumentList* : 
>
>> *AssignmentExpression* 
>>
>> *ArgumentList* , *AssignmentExpression*

例子说明 *ArgumentList* 可能是就一个 *AssignmentExpression* ，也可能是一个 *ArgumentList* 跟着一个逗号、一个 *AssignmentExpression* 。在这里， *ArgumentList* 的定义是递归的。结果是 *ArgumentList* 可能包含了任何正整数个用逗号分隔的参数，每个参数表达式是一个 *AssignmentExpression* 。这种递归定义的非终结符是很常见的。

下标“opt”可能在一个终结符或非终结符的后面出现，它表示一个可选的符号。包含可选符号的项目实际指定了两个右值，一个有该标记，另一个没有该标记。即：

> *VariableDeclaration* : 
>
>> *Identifier* *Initialiser* <sub>opt</sub>

它的另一种便捷的表示法为：

> *VariableDeclaration* : 
>
>> *Identifier*
>>
>> *Identifier* *Initialiser*

并且： 

> *IterationStatement*  : 
>
>> **for** ( *ExpressionNoIn* <sub>opt</sub> ; *Expression* <sub>opt</sub> ; *Expression* <sub>opt</sub> ) *Statement* 

也就是：

> *IterationStatement*  : 
>
>> **for** ( ;  *Expression* <sub>opt</sub> ;  *Expression* <sub>opt</sub> )  *Statement*  
>>
>> **for** ( *ExpressionNoIn* ;  *Expression* <sub>opt</sub> ;  *Expression* <sub>opt</sub> )  *Statement* 

也就是：

> *IterationStatement*  : 
>
>> **for** ( ; ;  *Expression* <sub>opt</sub> )  *Statement*  
>>
>> **for** ( ;  *Expression* ;  *Expression* <sub>opt</sub> ) *Statement*  
>>
>> **for** ( *ExpressionNoIn* ; ;  *Expression* <sub>opt</sub> ) *Statement*  
>>
>> **for** ( *ExpressionNoIn* ;  *Expression* ;  *Expression* <sub>opt</sub> ) *Statement* 

也就是：

> *IterationStatement*  : 
>
>> **for** ( ; ; ) *Statement* 
>>
>> **for** ( ; ;  *Expression* )  *Statement*  
>>
>> **for** ( ;  *Expression* ; )  *Statement*  
>>
>> **for** ( ;  *Expression* ;  *Expression* )  *Statement*  
>>
>> **for** ( *ExpressionNoIn*  ; ; ) *Statement*  
>> 
>> **for** ( *ExpressionNoIn*  ; ; *Expression* )  *Statement*  
>>
>> **for** ( *ExpressionNoIn*  ; *Expression* ; )  *Statement*  
>> 
>> **for** ( *ExpressionNoIn*  ; *Expression* ;  *Expression* )  *Statement*

所以，非终结符 *IterationStatement* 实际上有8个可选的右值。

如果短语“[empty]”作为一个部件的右值出现，它表示右值中不包含终结符或非终结符。

如果短语“[lookahead ∉ set]”在一个部件的右值中出现，它表示如果紧随其后的输入令牌是给定 *set* 的成员，该部件可能不会被用到。 *set* 可以写成是用花括号包裹起来的一些终结符的列表。为了方便，该集合可以写成一个非终结符，它是可展开的，里面表示了终结符的集合。例如，给定了如下定义：

> *DecimalDigit* :: **one of**
>
>> 0  1  2  3  4  5  6  7  8  9 

> *DecimalDigits*  ::  
>
>> *DecimalDigit* 
>>
>> *DecimalDigits* *DecimalDigit* 

又有如下定义，

> *LookaheadExample*  ::  
>
>> n  [lookahead ∉ { 1 ,  3 ,  5 ,  7 ,  9 }]  *DecimalDigits* 
>>
>> *DecimalDigit*  [lookahead ∉ *DecimalDigit* ]  

能够匹配到字母n后面是一个或多个偶数数字，也能够匹配到一个数字紧随其后是个非数字的情况。

如果短语“[no *LineTerminator* here]”在该句法的一个部件的右值中出现，它表示为受限的部件：如果一个 *LineTerminator* 正好出现在输入流中标识的位置上，那么该部件就不能用。例如：

> *ReturnStatement*   : 
>
>> **return** [no *LineTerminator* here]  Expression <sub>opt</sub> ;

表示如果程序中一个 *LineTerminator* 正好出现在 **reture** 令牌和 *Expression* 之间，该部件就不能用。

除非一个 *LineTerminator* 被一个限制的部件禁止出现，在输入元素流中任意两个令牌之间都可能出现任意数量的 *LineTerminator* ，这些都不会影响到程序句法的接受度。

在一个语法定义中，当单词“ **one of** ”跟在冒号后面时，它表明后面一行或多行的每个终结符都是一个可选的定义。例如，ECMAScript的词法包含了一个部件：

> *NonZeroDigit*  ::  **one of**  
>
>> 1 2  3  4  5  6  7  8  9

另一个便捷的写法：

> *NonZeroDigit*  ::
> 
>> 1
>>
>> 2 
>>
>> 3  
>>
>> 4  
>>
>> 5  
>>
>> 6  
>>
>> 7  
>>
>> 8  
>>
>> 9

当词法或数字字符串语法的部件中的一个可选项是多个字符的令牌时，它表示了组成该令牌的字符序列。

如果使用短语“but not”，一个部件的右值可能指定了不允许某种展开，也就是排除某种展开。例如：

> *Identifier* ::  
>
>> *IdentifierName* **but not** *ReservedWord* 

表示该非终结符 *Identifier* 可能被能够替换 *IdentifierName* 的任何字符序列所替换，但相同的字符序列不能替换 *ReservedWord*。

最后，少数用sans-serif字体显示的非终结符使用了一些描述性的短语解释，因为把所有的可选项都列出来是不现实的：

> *SourceCharacter*   ::  
>
>> any Unicode code unit 

## 5.2 算法约定

本规范经常在一个算法中使用一个数字编码的列表。这些算法用来精确地指定ECMAScript语言构建所必须的场景。算法并没暗示使用任何特定的实现技术。事实上，也许有更高效的算法可选来实现给定的功能。

为了方便在本规范中的多个部分中使用，有些算法，所谓的抽象操作，命名和书写为参数化函数的形式，因此它们在其它算法中可以通过名字来引用。

当一个算法会产生一个返回值时，使用指令“return x”来表示算法的返回值为x，然后算法终止。Result(n)用来表示“step n的结果”。

为了更清晰地表达，算法步骤可能被分解成一些有序的子步骤。子步骤是缩进的，它们自身可能又被分解成一些有序的缩进的子步骤。大纲式的数字编码约定用来标识子步骤，子步骤的第一级使用小写字母按字母顺序标记，第二级子步骤使用小写的罗马数字标记。如果多于三级，从第四级开始就使用数字标记来重复这些规则。例如：

> 1\. Top-level step 
>
>> a. Substep. 
>>
>> b. Substep  
>>
>>> i. Subsubstep. 
>>>
>>> ii. Subsubstep. 
>>>
>>>> 1\. Subsubsubstep 
>>>>
>>>>> a. Subsubsubsubstep

一个步骤或子步骤可能书写成一个“if”判断，作为它的子步骤的条件。这种情况下，子步骤只在判断是true的时候得到应用。如果一个步骤或子步骤是以单词“else”开始的，是前面同级步骤的“if”的否定判断。

一个步骤可能重复迭代指定应用其子步骤。

本条中后面定义的如加、减、取负、相乘、相除等数学操作和数学函数应该总是被理解为在数学实数之上计算确切的数学结果，它不包含无穷数，也不包含负数零（相对的是正零）。本规范中的算法建模，包含一些显式的步骤浮点来处理无限数和有符号的零，以及取整操作。如果一个数学操作或函数应用在一个浮点数之上，它应该被理解为应用在该浮点数表示的确切的数值上；这个浮点数必须是有限的数，如果是+0或-0对应的数值就是简单的0而已。

数学函数abs(x)得到x的绝对值，如果x是负数（小于0）则为-x，否则为x本身。

数学函数sign(x)得到，如果x为正数则为1，如果x为负数则为-1。本规范中sign函数不适用x为零的情况。

符号“x modulo y”（y必须为有限的数且非零）是计算（找到）一个与y相同符号的k值，满足abs(k) < abs(y)且x - k = q x y，q为整数。

数学函数floor(x)得到不大于x的最大整数。

> 注：floor(x) = x - (x modulo 1)

如果一个算法定义为“抛出异常”，那么执行算法将会终止也没有返回结果。调用的它的算法也将终止，直到有一个算法步骤显式地处理了该异常，例如使用术语“如果抛出了一个异常...”。一旦遇到这样的算法步骤，该异常将会被忽略。

【译注：终结符是语言中用到的基本元素，一般不能再被分解。】
