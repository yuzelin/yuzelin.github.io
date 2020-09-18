---
title: 第5章 继承
date: 2020-09-14 21:55:52
tags:
  - reading notes
  - Java
categories:
  - Java
  - Core Java Volume Ⅰ
---

# 类、超类和子类

## 定义子类

&emsp;&emsp;使用**extends**表示继承，表明正在构造的新类派生于于一个已存在的类。这个已存在的类称为超类，新类称为子类。

## 覆盖方法

&emsp;&emsp;子类不能直接访问父类的私有字段，可以使用相应的get方法访问，此时用**super.xxx**代表访问父类的方法。super是指示编译器调用超类方法的关键字，并不是如同this一样的引用。

​	为了避免覆盖父类方法时，由于参数类型写错而未覆盖的错误，使用**@override**标记子类方法，这样在错误的时候编译器会报告错误。

&emsp;&emsp;在覆盖方法时，子类方法不能低于超类方法的可见性。

## 子类构造器

&emsp;&emsp;子类构造器第一行需要使用super关键字来调用超类的构造器，而且必须是子类构造器的第一条语句，除非该构造器第一行使用this调用另一个子类构造器（在进入另一个子类构造器后，第一条语句同样是super(...)或者this(...)，这样最终还是会首先执行父类构造器）。

&emsp;&emsp;如果子类构造器没有显式地调用超类的构造器，将自动地调用超类的无参数构造器。如果超类没有无参数构造器，则会编译错误。

## 多态

&emsp;&emsp;一个对象变量可以指示多种实际类型的现象称为多态。在运行时能够自动地选择适当的方法称为动态绑定。

&emsp;&emsp;一个超类对象变量可以引用任意一个子类对象，不能将超类的引用直接赋给子类变量。

## 阻止继承：final类和方法

&emsp;&emsp;不允许扩展的类被称为final类。类中的方法也可以被声明为final，子类不能覆盖这个方法（final类中的所有方法自动地成为final方法）。

## 强制类型转换

&emsp;&emsp;当超类的对象变量确实引用的是子类对象，可以进行强制类型转换，否则强制类型转换将会抛出异常。可以使用**instanceof**检查引用的对象是否是该类的实例。

## 抽象类

&emsp;&emsp;类中不需要实现的方法可以使用**abstract**声明为抽象方法。包含一个或多个抽象方法的类必须声明为抽象。当然，不含抽象方法也可以将类声明为抽象类。

&emsp;&emsp;抽象方法起占位作用，在子类中具体实现。扩展抽象类时，要么保留抽象方法，将子类也标记为抽象类；要么定义全部的方法。

&emsp;&emsp;抽象类不能实例化，但可以创建对象变量。

## 受保护访问

&emsp;&emsp;如果希望超类中的某个方法或字段允许子类访问，可以声明为**protected**。受保护部分对所有子类及同一个包中的类可见。

&emsp;&emsp;对Java中4个访问控制修饰符做个小结：

- private：仅对本类可见
- public：对外部完全可见
- protected：对本包和所有子类可见
- 默认（无修饰符）：对本包可见

# Object：所有类的超类

&emsp;&emsp;Object类是Java中所有类的始祖，如果没有明确指出超类，则默认扩展Object。

## Object类型的变量

&emsp;&emsp;可以使用Object类型的变量引用任何类型的对象。Java中只有基本类型不是对象，所有数组类型都是类。

## equals方法

&emsp;&emsp;编写一个完美的equals方法的建议：

1. 显式参数命名为otherObject，稍后需要将它强制转换成另一个名为other的变量
2. 检测this与otherObject是否相等：

```java
if (this == otherObject) return true;
```

3. 检测otherObject是否为null，如果为null，返回false
4. 比较this与otherObject的类。如果equals的语义可以在子类中改变，就使用getClass检测：

```java
if (getClass() != otherObject.getClass()) return false;
```

如果所有的子类都有相同的相等性语义，可以使用instanceof检测：

```java
if (!(otherObject instanceof ClassName)) return false;
```

5. 将otherObject强制转换为相应类型的变量：

```java
ClassName other = (ClassName) otherObject;
```

6. 现在根据相等性概念的要求来比较字段。使用 == 来比较基本类型字段，使用Objects.equals比较对象字段，使用Arrays.equals方法比较数组元素：

```java
return field1 == other.field1 
	&& Objects.equals(field2, other.field2) 
	&& ...;
```

如果在子类中重新定义equals，就要在其中包含一个super.equals(other)调用。

# 对象包装器与自动装箱






