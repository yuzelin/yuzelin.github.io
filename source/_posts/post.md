---
title: Java格式化输出的转换符和标志
date: 2020-09-03 14:41:01
tags:
  - Java
  - readiong notes
categories:
  - Java
  - Core Java Volume Ⅰ
---

&emsp;&emsp;Java 5沿用了C中printf方法，可以为System.out.printf提供转换符和标志控制输出的格式。

| 转换符 |              类型              |
| :----: | :----------------------------: |
|   d    |           十进制整数           |
|   x    |          十六进制整数          |
|   o    |           八进制整数           |
|   f    |           定点浮点数           |
|   e    |           指数浮点数           |
|   g    | 通用浮点数（e和f中较短的一个） |
|   a    |            十六进制            |
|   s    |                                |
|   c    |                                |
|   b    |                                |
|   h    |                                |
|   %    |                                |
|   n    |                                |