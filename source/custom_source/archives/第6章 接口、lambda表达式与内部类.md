---
title: 第6章 接口、lambda表达式与内部类
date: 2020-10-10 14:04:14
tags: 
  - Java
  - Reading notes
categories:
  - Java
  - Core Java Volume Ⅰ
---

# 接口

## 接口的概念

&emsp;&emsp;接口不是类，而是对希望符合这个接口的类的一组**需求**。关键字**interface**。

&emsp;&emsp;接口中的所有方法自动是public，因此在声明时不必提供关键字。

&emsp;&emsp;接口可以包含多个方法；接口可以定义常量，自动是public static final；接口不能有实例字段；Java 8之前接口不能实现方法。

&emsp;&emsp;让类实现接口的步骤：1. 将类声明为实现给定的接口，关键字**implements**；2. 对接口中的所有方法提供定义。在实现方法时必须声明为public，否则访问属性是包可见性，编译器会报错。

## 接口的属性

&emsp;&emsp;不能使用new实例化一个接口（接口不是类），但是可以声明接口的变量，必须引用实现了这个接口的类对象。

&emsp;&emsp;可以使用instanceof检查一个对象是否实现了某个接口。

&emsp;&emsp;可以扩展接口。

&emsp;&emsp;可以实现多个接口，用','隔开。

## 静态和私有方法

&emsp;&emsp;Java 8中允许在接口中增加静态方法。Java 9中接口的方法可以是private。

## 默认方法 & 解决默认方法冲突

&emsp;&emsp;可以为接口方法提供一个默认实现，必须用**default**修饰。默认方法可以调用其他方法。

&emsp;&emsp;在一个接口中定义一个默认方法，同时在超类或另一个接口中定义同样的方法，解决冲突的规则如下：

1. 超类优先。超类中定义了一个标签相同的方法，默认方法会被忽略（因此无法重新定义Object类的方法，基于该规则会被覆盖）；
2. 接口冲突。两个接口提供了同名方法，必须覆盖。在使用接口的方法时可以用*xxx.super.f()*。

## 对象克隆

&emsp;&emsp;如果需要使用clone方法，类必须实现Cloneable接口，重新定义clone方法并声明为public。在这里，Cloneable接口的出现与接口的正常使用没有关系，它没有指定clone方法，这个方法是从Object类继承的。这类接口称为标记接口，不包含任何方法，唯一的作用是允许在类型查询中使用instanceof。

&emsp;&emsp;所有数组类型都有一个公共的clone方法，可以用这个方法建立一个新数组。

# lambda表达式

## 为什么引入lambda表达式

&emsp;&emsp;某些时候需要将一个代码块传递到某个对象，这个代码块会在某个时刻调用。为了避免生成对象来包含代码块，可以使用lambda表达式以简单的语法来定义代码块。

## lambda表达式的语法

&emsp;&emsp;通常形式：(argument list) -> { body }

&emsp;&emsp;即使没有参数也需要小括号（类似于无参数方法）；

&emsp;&emsp;如果可以推导出参数类型，则可以忽略；

&emsp;&emsp;如果方法只有一个参数且类型可以推导出，那么可以省略小括号；

&emsp;&emsp;无须指定返回类型，返回类型总是会由上下文推导得出。

## 函数式接口

&emsp;&emsp;对于只有一个抽象方法的接口，需要这种接口的对象时，就可以提供一个lambda表达式。这种接口称为函数式接口。实际上，在Java中对lambda表达式能做的也只是转换为函数式接口。

## 方法引用

&emsp;&emsp;当lambda表达式的体只调用一个方法而不做其他操作时，可以把lambda表达式重写为方法引用。方法引用与lambda表达式类似，总是会转换为函数式接口的实例。

&emsp;&emsp;使用方法主要分三种：

1. object::instanceMethod：等价于向方法传递参数的lambda表达式；
2. Class::instanceMethod：第一个参数会成为方法的隐式参数；
3. Class::staticMethod：所有参数都传递到静态方法

&emsp;&emsp;可以在方法引用中使用this和super。

## 构造器引用

&emsp;&emsp;构造器引用与方法引用类似，方法名为new。可以为数组类型建立构造器引用，例如，int[]::new，它有一个参数，数组的长度，等价于lambda表达式x -> new int[x]。

## 变量作用阈

&emsp;&emsp;lambda表达式可以捕获外围作用域中变量的值。lambda表达式中捕获的变量必须是事实最终变量，即这个变量在初始化后就不会再赋新值。

&emsp;&emsp;在lambda表达式中使用this关键字时，是指创建这个lambda表达式的方法的this参数。

# 内部类

&emsp;&emsp;内部类是定义在另一个类中的类。使用内部类主要有两个原因：1. 内部类可以对同一个包中的其他类隐藏； 2. 内部类方法可以访问定义这个类的作用域中的数据，包括私有数据。

## 内部类的特殊语法规则

&emsp;&emsp;内部类可以直接访问外围类的数据。正规的语法是OuterClass.this表示外围类的引用。

&emsp;&emsp;在构造内部类对象实例的时候，使用outerObject.new可以设置外围类引用（默认情况下是this）。

&emsp;&emsp;在外围类的作用域之外，可以使用OuterClass.InnerClass引用内部类。

&emsp;&emsp;内部类中的所有静态字段必须是final，并初始化为一个编译时常量。内部类不能有static方法（静态内部类可以有）。

## 局部内部类

&emsp;&emsp;如果一个类只在某个方法中使用了一次，可以在方法中局部地定义这个类。声明时不能有访问说明符，它的作用域被限定在声明它的块中。它对外部世界完全隐藏，除定义它的方法外无法访问。

## 匿名内部类

&emsp;&emsp;使用局部内部类时，如果只想创建这个类的一个对象，甚至不需要为类指定名字，这样一个类称为**匿名内部类**。一般地，语法如下：

```
new SuperType(construction parameters)
{
	inner class methods and data
}
```

&emsp;&emsp;它的含义是创建一个类的新对象，这个类实现了SuperType（SuperType是接口）或扩展了SuperType（SuperType是类），大括号内是需要实现的方法、其他方法和字段。

&emsp;&emsp;由于构造器的名字必须与类名相同，而匿名内部类没有名字，故匿名内部类不能有构造器。实际上，构造参数要传递给超类构造器。如果内部类是实现一个接口，就不能有构造参数。可以提供一个初始化块。

## 静态内部类

&emsp;&emsp;如果使用内部类只是为了把一个类隐藏在另一个类的内部，并不需要内部类有外围类的一个引用，可以将内部类声明为static。

&emsp;&emsp;静态内部类可以有静态字段和方法。

&emsp;&emsp;在接口中声明的内部类自动是static。

## 服务加载器

&emsp;&emsp;不深入了解。

# 代理

&emsp;&emsp;不深入了解。