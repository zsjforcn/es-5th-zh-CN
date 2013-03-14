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

String类型是有限个有顺序的零或多个16位无符号整数（“元素”）的集合。String类型常常用来表示一个正在运行的ECMAScript程序中的文本数据。

# 8.5 Number类型


# 8.6 Object类型


## 8.6.1 属性的特征
## 8.6.2 对象的内部属性和方法


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
