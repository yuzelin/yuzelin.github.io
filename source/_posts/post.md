---
title: 第7章 异常、断言和日志
date: 2020-10-11 17:06:22
tags:
  - Java
  - Reading notes
categories:
  - Java
  - Core Java Volume Ⅰ
---

# 处理错误

## 异常分类

&emsp;&emsp;在Java中，异常对象都是派生于Throwable类的一个实例。

&emsp;&emsp;Throwable类分为两个分支：Error和Exception。

&emsp;&emsp;Error描述了Java运行时系统的内部错误和资源耗尽错误。自己写的程序不应该抛出这种类型的错误。如果出现这种类型的错误，除了通知用户，并尽力妥善地终止程序之外，几乎无能为力（这种情况很少出现）。

&emsp;&emsp;Exception又分为两个分支：RuntimeException和IOException。一般规则是：由编程错误导致的异常属于RuntimeException；如果程序本身没有问题，但由于像I/O错误这类问题导致的异常属于其他异常。

&emsp;&emsp;Java语言规范将派生于Error类或RuntimeException类的所有异常称为非检查型（unchecked）异常，所有其他的异常称为检查型（checked）异常。编译器将检查是否为所有的检查型异常提供了异常处理器。

## 声明检查型异常

&emsp;&emsp;如果遇到无法处理的情况，方法可以抛出一个（检查型）异常。关键字：**throws**。

&emsp;&emsp;在遇到下面4种情况时会抛出异常：

- 调用了一个抛出检查型异常的方法；
- 检测到一个错误，并且利用throw语句抛出一个检查型异常；
- 程序出现错误，会抛出一个非检查型异常；
- Java虚拟机或运行时库出现内部错误。

&emsp;&emsp;如果出现前两种情况，则必须告诉调用这个方法的人可能抛出异常。一个方法必须声明所有可能抛出的检查型异常（用','隔开）。

&emsp;&emsp;如果在子类中覆盖了超类的一个方法，子类方法中声明的检查型异常不能比超类方法中声明的异常更通用（可以抛出更特定的异常或不抛出）。如果超类方法没有抛出异常，子类也不能。

&emsp;&emsp;如果方法声明它会抛出一个特定类的异常，那么这个方法抛出的异常可能属于这个类，也可能属于这个类的任意一个子类。

## 如何抛出异常

&emsp;&emsp;如果一个已有的异常类能够满足要求，抛出异常的步骤很简单：

1. 找到一个合适的异常类；
2. 创建这个类的一个对象，关键字**throw**：*throw exceptionObj;*；
3. 将对象抛出。

&emsp;&emsp;一旦方法抛出异常，这个方法就不会返回到调用者。

## 创建异常类

&emsp;&emsp;标准异常类无法满足需求时，可以创建自己的异常类。这个类需要派生于Exception或者Exception的某个子类。

# 捕获异常

## 捕获异常

&emsp;&emsp;捕获异常使用**try/catch**语句块。最简单的示例：

```
	try
	{
		...
	}
	catch (ExceptionType e)
	{
		handler for this type
	}
```

&emsp;&emsp;如果try语句块中的任何代码抛出了catch子句中指定的一个异常类，那么

1. 程序将跳过try语句块的其余代码。
2. 程序将执行catch子句中的处理器代码。

&emsp;&emsp;如果try语句块中的代码没有抛出任何异常，程序将跳过catch子句。

&emsp;&emsp;如果方法中的任何代码抛出了catch子句中没有声明的一个异常类型，这个方法就会立即退出。