---
title: Class文件长什么样
comments: true
toc: true
date: 2019-09-25 19:55:16
categories: JVM
tags: JVM
---

# 定义

<font color=#f07c82>Class</font>文件是Java语言实现跨平台的原材料，<font color=#f07c82>JVM(Java Vritual Machine)</font>是实现跨平台的机器。

机器 + 原材料 = 跨平台。

不同的平台有自己的JVM，但是Class文件是一样的。

Class文件由java文件编译产生。

<font color=#f07c82>Class文件是由8位字节构成的二进制流，用类似于C语言结构体的伪结构来存储数据。</font>

Class文件由<font color=#f07c82>无符号数和表</font>组成。

**无符号数**用<font color=#f07c82>u1 u2 u4 u8</font>来表示1 2 4 8个字节的无符号数。它用来表示数量值、数值、索引引用、按照UTF-8编码的字符串。

**表**是一种特殊的数据结构，它由表及无符号数组成，习惯表以<font color=#f07c82>_info</font>结尾。

**Class文件就是一张表，用来描述唯一确定的类或接口。**

下面是从《深入了解JVM》中摘的Class文件的表结构。

{% asset_img Class_info.PNG %}

**<center>图1 Class表结构</center>**

# 进入Class这张表

首先我们写一个简单的类，叫做<font color=#f07c82>HelloClass</font>。

```java
public class HelloClass {
    private int name;

    public int addName() {
        return name + 1;
    }
}
```

然后编译生成 <font color=#f07c82>HelloClass.class</font>文件，我们用文本编辑器打开它，如下：

<center>{% asset_img class2.png %}</center>

**<center>图2 编译后的class文件</center>**

## 最复杂的结构，常量池

由于Class本身就是一张表，而<font color=#f07c82>常量池</font>是嵌套在Class中的表，其结构最复杂。

常量池用来存储**类和接口的全限定名、字段的名称和描述符、方法的名称和描述符。**

如**图2**所示，前四个字节的无符号数 <font color=#f07c82>u4</font>表示 Magic Number，是用来区分文件格式的，这样比文件名后缀更安全。

**<font color=#f07c82>CAFE BABE</font>**是Java的Class文件的魔数，很魔性。

接着的<font color=#f07c82>u4</font>是版本号，即**图1**中的 **00 00 00 34**。

然后就是常量池了，先看常量池的项目类型：

<center>{% asset_img fig1.PNG %}</center>
<center>{% asset_img fig.2.PNG %}</center>

**<center>图3 常量池的项目类型</center>**

每个项目类型的第一位都是项目标志，唯一标识该类型。

常量池开始是容量说明，<font color=#f07c82>u2</font>类型，及 **00 16**，表示容量为 16+6=22-1=21，因为常量池容量计算是从1开始，0表示为空。

之后一个<font color=#f07c82>u1</font>类型是常量类型标志符，为 **0A**，为 10。查找**图2**表示该常量为 **CONSTANT_Methodref_info**，由两个<font color=#f07c82>u2</font>无符号类型组成，第一个指向 **00 04** 即第4个常量，为 **CONSTANT_Class_info** 类型；第二个指向 **00 12** 即第 16+2=18 个常量， 为 **CONSTANT_NameAndType** 类型。

依次类推，直到翻译完常量池。

我们可以通过**javap**工具的 **-verbose** 输出字节码内容，如下：

<center>{% asset_img javap.png %}</center>

## 访问标志

**访问标志(access_flags)**用于识别类和接口的访问信息，诸如是否是public及抽象类等。

常量池后面是访问标志。

<center>{% asset_img access_flags.png %}</center>

不同访问标志求 <font color=#f07c82>|</font> 运算得到。

## 类索引、父类索引、接口索引

分别是一个 **u2、u2**和一组**u2**，因为java单继承，可实现多个接口。

各自指向一个 **CONSTANT_Class_info** 的类描述常量。

## 字段表集合

**<font color=#f07c82>字段表(field_info)</font>用来描述接口或类中声明的变量，字段包括类级别变量以及实例级别变量。**但不包括方法内部声明的局部变量。

字段需要用**访问标志描述**，<font color=#f07c82>包括作用域(public、protected、default、private)、实例变量还是类变量(static)、是否为final、并发可见性(volatile)、是否可序列化(transient)、类型(基本类型、对象及数组)以及字段名称。</font>

<center>{% asset_img ziduan.PNG %}</center>

字段表的结果除了访问标志描述，还有字段的简单名称 **name_index** 及方法的描述符 **discriptor_index**、属性表统计 **attributes_count** 及属性表 **attributes**。

<center>{% asset_img attributes.png %}</center>

字段的简单名称及方法描述符都是对常量池引用。

**描述符是用来描述字段与方法的类型与返回参数的。**

**<font color=#f07c82>由于这类信息不能跟访问标志描述一样，可以用固定长度来表示，所以需要在常量池中表示后然后引用。</font>**

基本数据类型用相应大写字母表示，对象类型用大写字母 **L**加对象全限定名表示。

<center>{% asset_img L.PNG %}</center>

其中数组类型用前面 **[**加类型表示，如 String[][],就表示为 **[[Ljava/lang/String;**。

**描述方法时，先参数列表，后返回类型。**如 int i(), 表示为 **（）I**

## 方法表集合

与字段表类似,只不过并发可见性(volatile)、是否可序列化(transient)不能修饰方法。

<center>{% asset_img fangfa.PNG %}</center>

方法中的代码描述，由编译器编译成字节指令后，存放在属性表**attributes** 的 **<font color=#f07c82>Code</font>**属性里。

## 属性表

<font color=#f07c82>属性表(attributes_info)</font>在Class文件、属性表与方法表内都有出现，用于描述场景特殊信息。

属性表其它数据项目严格要求的顺序、长度和内容，它**不再要求各个属性有严格的顺序，且只要不与已有属性名重复，任何实现的编译器都可向属性表中写入自己定义的属性信息；**Java虚拟机在运行时会忽略掉不认识的属性。

《Java虚拟机规范(Java SE 7)》中，预定义的属性为：

<center>{% asset_img predefine1.PNG %}</center>
<center>{% asset_img predefine2.PNG %}</center>

其中每个属性表首先根据 <font color=#f07c82>attributes_name_index</font> 去常量池引用 <font color=#f07c82>CONSTANT_Class_info</font> 类型的名称，然后用一个 <font color=#f07c82>u4</font> 长度的属性说明属性表占用的长度 <font color=#f07c82>attribute_length</font> 即可。属性表中描述的信息单位是 <font color=#f07c82>u1</font>，共<font color=#f07c82>attribute_length</font>个。

属性表的结构如下：

<center>{% asset_img attributes_info.PNG %}</center>

### Code属性

**java程序方法体内的代码通过编译成字节码后，存在Code属性中。**

Code属性存储在**方法表的属性表集合**中。

Code属性是Class文件中**最重要的**属性，是对代码的描述。其它信息可以看做是对元数据（类型、字段、方法及其他信息）的描述。

Code的属性表如下：

<center>{% asset_img code_info.PNG %}</center>

各部分的含义如下：

类型 |名称 | 含义
:-:|:-:|:-: 
u2 |attributes_name_index | 指向 CONSTANT_Class_info 常量索引，表示该属性表名称
u1 | attribute_length | 属性表长度。由于属性名称索引 u2 加长属性名长度 u4，所以属性表长度为整个属性表长度减去 **6** 字节。
u2 | max_stack | 栈的最大深度。**Java虚拟机为该方法体分配栈针的操作栈深度**。
u2 | max_locals | 局部变量的最大存储空间。**方法参数、显示异常处理的参数及方法体中定义的局部变量都需用局部变量表表示。**局部变量中统计单位是 **Slot**。小于32位(boolean、short、char、int、float、returnAddress)的变量用一个 Slot 表示，double与long两个64位的用两个Slot表示。
u4 | code_length | 字节码长度。实际是 u2 长度，即16个字节， 65535条字节码，超过改长度虚拟机将拒绝编译。
u1 | code | 一些类的字节码指令，用于描述编译后的方法体内方法。
u2 | exception_table_lenght | 显示异常处理表长度。
excessption_info | exception_table | 显示异常处理表
u2 | attributes_count | 属性表数量
attributes_info | attributes | 属性表

# 参考

[深入理解java虚拟机](https://book.douban.com/subject/6522893/)