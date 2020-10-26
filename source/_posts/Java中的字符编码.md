---
title: Java中的字符编码
date: 2020-09-02 13:16:57
tags:
  - Character encoding
  - Unicode Transformation Formats(UTF)
  - UTF-16
categories:
  - Fundamental Knowledge of CS
---

&emsp;&emsp;在看*Core Java*时，看到char类型一节，发现Java中的char与C中的char完全不同，它与Unicode编码有关系。为了弄清楚这个问题，查看了一些资料，记录如下。

&emsp;&emsp;首先是编码问题，通过以下两篇文章有了比较清晰的认识：

{% link 字符编码笔记：ASCII，Unicode 和 UTF-8 https://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html true 字符编码笔记：ASCII，Unicode 和 UTF-8 %}<br>

{% link 程序员必备：彻底弄懂常见的7种中文字符编码 https://zhuanlan.zhihu.com/p/46216008 true 程序员必备：彻底弄懂常见的7种中文字符编码 %}

所以不再赘述。

&emsp;&emsp;再说Java和Unicode的关系。Java采用的是{%link UTF-16 https://en.wikipedia.org/wiki/UTF-16 true UTF-16 %}编码方式。顾名思义，UTF-16的基本“单元”是16 bits，称之为代码单元（code unit）。而Unicode中的字符对应的代码值称为码点（code point）。Unicode的码点可以分成17个代码平面（{% link code plane https://en.wikipedia.org/wiki/Plane_(Unicode) true Plane(Unicode) %}）。第一个代码平面称为基本多语言平面（Basic Multilingual Plane, BMP），码点范围为U+0000-U+FFFF，每个字符只用16 bits即可表示；其余的平面称为辅助平面（Supplementary Planes），码点范围为U+10000-U+10FFFF，用两个连续的代码单元对（surrogate pair）表示。如何知道一个代码单元代表一个码点，还是某个码点的一半（surrogate）呢？

&emsp;&emsp;在Unicode标准中，BMP平面的U+D800-U+DFFF是没有分配字符的，留给UTF-16编码辅助平面中的字符。U+D800-U+DBFF用于high surrogate，U+DC00-U+DFFF用于low surrogate。具体的算法是（{%link wikipedia: UTF-16, 2.2 https://en.wikipedia.org/wiki/UTF-16#Code_points_from_U+010000_to_U+10FFFF true UTF-16 %}）：

- 码点U减去0x10000得U'（0x0-0xFFFFF），表示为20 bits；
- U'的高10 bits（0x0-0x3FF）加到0xD800上作为high surrogate。可以看到这样操作下，high surrogate的范围正好是0xD800–0xDBFF；
- U‘的低10bits加到0xDC00上作为low surrogate，范围为0xDC00-0xDFFFF。

这样，在UTF-16中，就能简单的区分一个代码单元是BMP中码点还是surrogate。Java中的char类型正是描述了UTF-16编码中的一个代码单元。String在JDK 8及以前使用char[]实现，从JDK 9起使用byte[]实现。不管哪一种实现，String中的length方法返回的是代码单元数量。如果包含两单元字符的话，需要使用codePointCount方法得到码点的数量。使用字符𝕆（U+1D546; \uD835\uDD46）测试：

```java
String s = "𝕆";
System.out.println(s.length() + " " +
        s.codePointCount(0, s.length()));
```

结果为：2 1