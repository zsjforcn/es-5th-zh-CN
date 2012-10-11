#《ECMAScript语言规范》

ECMA-262 标准——第五版/2009年12月

## 目录

* 0 介绍

* 1 作用域

* 2 一致性

* 3 标注化参考

* 4 综述
	* 4.1 Web（脚本）编程
	* 4.2 语言综述
		* 4.2.1 对象
		* 4.2.2 ECMAScript的Strict变体
	* 4.3 定义

* 5 标记约定
	* 5.1 句子和词汇语法
		* 5.1.1 上下文无关语法
		* 5.1.2 词汇和正则语法
		* 5.1.3 数字字符串语法
		* 5.1.4 句子语法
		* 5.1.5 JSON语法
		* 5.1.6 语法符号
	* 5.2 算法约定
 
* 6 源码文本
 
* 7 词汇约定
	* 7.1 Unicode格式控制符
	* 7.2 空白符
	* 7.3 行结束符
	* 7.4 注释
	* 7.5 标记符片段
	* 7.6 标记符名称和标记符
		* 7.6.1 保留字
	* 7.7 标点符号
	* 7.8 字面量
		* 7.8.1 Null字面量
		* 7.8.2 Boolean字面量
		* 7.8.3 Number字面量
		* 7.8.4 String字面量
		* 7.8.5 RegExp字面量
	* 7.9 分号自动插入
		* 7.9.1 分号自动插入的规则
		* 7.9.2 分号自动插入的例子

* 8 类型
	* 8.1 Undefined类型
	* 8.2 Null类型
	* 8.3 Boolean类型
	* 8.4 String类型
	* 8.5 Number类型
	* 8.6 Object类型
		* 8.6.1 属性的特征
		* 8.6.2 对象的内部属性和方法
	* 8.7 引用类型
		* 8.7.1 GetValue(V)
		* 8.7.2 PutValue(V,W)
	* 8.8 列表规范类型
	* 8.9 自动规范类型
	* 8.10 属性描述符和属性标记符规范类型
		* 8.10.1 IsAccessorDescriptor(Desc)
		* 8.10.2 IsDataDescriptor(Desc)
		* 8.10.3 IsGenericDescriptor(Desc)
		* 8.10.4 FromPropertyDescriptor(Desc)
		* 8.10.5 ToPropertyDescriptor(Obj)





## ECMAScript 语言规范

### 1 范围

本标准定义了ECMAScript脚本语言。

### 2 一致性

对ECMAScript一致的实现“必须”提供和支持所有类型、值、对象、属性、函数和程序语法以及本规范中所描述的各种场景。

对本国际化标准一致的实现“应该”使用一致的方式翻译字符：Unicode 3.0或更高版本标准，采用UCS-2或UTF-16编码形式级别3实现的ISO/IEC 10646-1。如果采用的ISO/IEC 10646-1的子集为定义，默认是BMP子集，集合300。同理，编码方式默认为UTF-16。

对ECMAScript一致的实现“允许”提供超越本规范描述的额外的类型、值、对象、属性和函数。特别地，对本规范所描述的对象，“允许”实现提供的属性和属性值可以不在本书的描述之内。

对ECMAScript一致的实现“允许”支持本规范中未描述的程序和正则语法。特别地，“允许”支持本规范中7.6.1.2章节描述的“将来保留词”的程序语法。


### 3 标准化参考

下面的参考文档。。。

### 4 综述

本章节只包含了ECMAScript的非规范性的概况。

ECMAScript是一种面向对象的编程语言，它在宿主环境中运行计算，操作计算的对象。在这里定义的ECMAScript没想过要独立自主地运行计算，事实上，本规范不提供外部数据输入或计算结果输出的规范。相反，希望ECMAScript的运行环境除了提供本规范中描述的对象和其他设施以外，还提供运行环境的宿主对象等。它们是指可以从ECMAScript程序中访问的某些属性和某些函数，关于这些行为和具体描述超出了本规范范围。

脚本是指用来操作、自定义和自动化已存在系统的设备的一种编程语言。在这样的系统中，通过用户接口一些很有用的功能已经存在了，脚本语言是把已有的功能暴露给程序控制的一种机制。就这样，给定系统就称为宿主环境，它提供对象和设备，并完成对脚本语言的兼容。脚本语言是同时为专业和非专业的程序员所准备的。

ECMAScript起初被设计为Web脚本语言，它提供一种机制，让浏览器中Web页面更加生动，并运行基于页面的客服端/服务器（C/S）架构的计算。ECMAScript能为各式各样的宿主环境提供核心的脚本兼容，因此本规范中的核心脚本语言和特定的宿主环境没有任何关系。

ECMAScript中的一些设施和其他编程语言中用到的类似，尤其是Java™，Self和Scheme，在如下参考中有提到：

Gosling, James, Bill Joy and Guy Steele. The Java™ Language Specification. Addison Wesley Publishing Co., 1996. 

Ungar, David, and Smith, Randall B. Self: The Power of Simplicity. OOPSLA '87 Conference Proceedings, pp. 227–241, Orlando, FL, October 1987. 

IEEE Standard for the Scheme Programming Language. IEEE Std 1178-1990.

#### 4.1 Web（脚本）编程

浏览器为客户端计算提供一个ECMAScript宿主环境，例如包括表示窗口、菜单、弹出框、对话框、文本块集合、锚点集合、框架集合、历史记录、cookies和输入/输出的各种对象。高级情况，宿主环境还提供把代码绑定到事件的方法，例如焦点变化、页面和图片的加载/卸载/出错/中断、选区、表单递交和鼠标动作等。脚本代码出现在HTML中，而被显示的页面是一个由各种用户界面元素、固定的和计算出来的文本和图片所组成的有机整体。脚本代码响应用户交互而并不需要一个主程序。

Web服务器为服务端计算提供一个不同的宿主环境，包括表示请求、客户端和文件等对象，以及锁定和共享数据等操作。通过同时使用浏览器端和服务器端脚本，假设为一个基于页面的应用提供自定义的用户接口，在客户端和服务器之间分布计算就成为了可能。

每个支持ECMAScript的浏览器和服务器实现了它自己的宿主环境，完成ECMAScript的执行环境。

#### 4.2 语言综述 

以下叙述并非正式ECMAScript概述——并非语言的所有部分都会提到。此综述非标准文档的一部分。

ECMAScript是基于对象的：基础语言和宿主设备通过对象提供的，ECMAScript程序是一个各种互相通信的对象簇。每个ECMAScript对象是一个属性集合，每个属性有零个或多个特性值组成，它们决定了当属性被使用时程序是如何表现的。例如，当一个属性的可写（Writable）特性值设置为false时，执行的代码试图修改这个属性的值时将会失败。属性是用来保存其他对象、原始值或函数的容器。每个原始值是如下一种内置类型的实例：Undefined、Null、Boolean、Number和String。每个对象是剩下的内置类型Object的实例。每个函数是一个可调用的对象，通过对象的属性关联起来的函数称之为方法。

在ECMAScript实体外网又定义一个内置对象组成的集合，包括全局对象、Object、Function、Array、String、Boolean、Number、Math、Date、RegExp、JSON和各种出错如Error、EvalError、RageError、ReferenceError、SyntaxError、TypeError和URIError等等对象。

ECMAScript也定义了一组内置操作符，包括各种一元、乘法、加法、位移、关系、相等、二进制按位、二进制逻辑、赋值和逗号等操作符。

ECMAScript语法有意和Java语法类似，它更从容使得它成为易用的脚本语言。例如，变量无需类型声明，属性也和类型没有关联，函数定义也无需在调用之前。

##### 4.2.1 对象

ECMAScript不像C++、Smalltalk或Java一样使用类，取而代之，它的对象可能通过各种其他方法来创建，包括文本字面量或构造器——创建对象并执行代码对它们的属性进行全部或部分赋值的方式。每个构造器是一个以“prototype”命名的函数，它用来实现基于原型的继承和共享属性。通过构造器创建对象是在new表达式中实现的，例如，new Date(2009,11)创建了一个Date对象。没有使用new而调用构造器有不同的例程，这取决于构造器本身。例如，Date()产生了一个表示当前日期和时间的字符串而不是一个对象。

每一个由构造器创建的对象都有一个指向构造器的“prototype”属性值的隐式引用，称之为对象的原型。此外，一个原型可能也会有一个指向它自己的原型的非空的隐式引用，以此类推。这就是所谓的原型链。当一个对象中的属性是一个引用时，该引用是在原型链中存在的第一个以该名字命名的对象的属性。换句话说，首先检验提到的对象是否有这个属性：如果有，那个属性就是所指向的引用；如果没有，接着检验该对象的原型；以此类推。

一般来说，在基于类的面向对象的语言中，实例承载状态，类承载方法，继承的只是结构和行为。在ECMAScript中，对象承载了状态和方法，结构、行为和状态都可以继承。

不直接包含特定的属性的所有对象都共享了原型链的属性和值。如图1所示：
【图1】
CF是一个构造器（也是一个对象）。使用new表达式创建了五个对象分别是：cf1、cf2、cf3、cf4和cf5。每个对象包含了名字为q1和q2的属性。点线表示隐式原型关系，故，例如cf3的原型是CFp。构造器CF自己有两个属性P1和P2，这两个属性对CFp、cf1、cf2、cf3、cf4或cf5是不可见的。在CFp中的属性CFP1除了对CF以外，对cf1、cf2、cf3、cf4和cf5是共享的。在CFp的隐式原型链上还有很多非q1、q2和CFP1命名的属性。注意，在CF和CFp之间没有隐式的原型关系。

不像基于类的面向对象语言，ECMAScript可以通过动态赋值的方式来为对象添加属性。也就是说，构造器没必要给所有或任何对象（所需）的属性命名或赋值。在上图中，每个人都可以为CFp添加一个新的属性让cf1、cf2、cf3、cf4和cf5共享。

##### 4.2.2 ECMAScript的Strict变体

ECMAScript语言意识到了一些可能性，有些用户也许希望限制使用语言中的某些特性。
