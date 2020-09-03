---
title: 第3章 Java的基本程序设计结构
date: 2020-09-01 16:06:10
tags:
  - reading notes
  - Java
categories:
  - Java
  - Core Java Volume Ⅰ
---

这是一些零碎的知识点，记录如下：

{% img [基本程序设计结构] /custom_source/images/基本程序设计结构.png %}

对其中的unsigned部分，做一个小的记录。以下以8位的byte为例。

首先是二进制算数运算，计算机内部数字是采用补码的方式存储的，因此，byte变量可以存储0x0-0xff的数，只不过若直接输出，只会输出有符号的形式。对0xff（255），若视为有符号的byte，是-1的补码，则会输出-1：

{% img [测试255] /custom_source/images/测试255.png %}

所以只要结果不溢出，即大于255，可以将其视为无符号数并进行加减乘的运算。若要得到它的无符号值，可以使用toUnsignedxxx方法；除此之外，可以使用var & 0xff的方法，原理是在进行&操作时，两个操作数隐式的转换到int，0xff的高3字节全部填充0，经过&把var的高3字节置0了，结果将var的补码视为一个正数的原码（正数原码等于补码），起到unsigned的效果。

通过查看Byte.toUnsignedInt方法的源码，可以看到做法正是这样：

{% img [Byte.toUnsignedInt] /custom_source/images/Byte.toUnsignedInt.png %}

通过源码的注释可以知道，这样操作后，负数相当于加了0x100(2<sup>8</sup>)。

有关char类型和String：{% post_link Java中的字符编码 %}

TODO：System.out.printf的格式化参数