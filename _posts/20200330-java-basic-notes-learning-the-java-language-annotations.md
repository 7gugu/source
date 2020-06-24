---
title: Java基础笔记--学习Java语言--注解
date: 2020-03-30 
updated: 2020-04-30
tags:
  - Java
urlname: java-basic-notes-learning-the-java-language-annotations
original: false
---
2.4 草草过一下. 
<!--more-->
# 注解Annotations
注解是元数据的一种, 提供有关程序的数据, 该数据不属于程序本身. 注解对代码本身没有直接的影响. 
## 注解格式The Format of an Annotation
一个简单的注解看起来是这样子的: 
~~~
@Entity
~~~
它用一个艾特@来告诉编译器他后面的内容是注解, 下面的注解名是Override
~~~
@Override
void myMethod()	{...}
~~~
注解可以包括元素, 比如我们使用文档注释的时候, 就会有@author注解
~~~
@Author(
   name = "my Name",
   date = "3/27/2003"
)
class MyClass() { ... }
~~~
或者
~~~
@SuppressWarnings(value = "unchecked")
void myMethod() { ... }
~~~
如果只有一个元素名称叫value, 那么名称可以省略: 
~~~
@SuppressWarnings("unchecked")
void myMethod() { ... }
~~~
如果注解没有元素, 可以像@Override一样省去括号. 
可以在声明处声明多个注解: 
~~~
@Author(name = "Jane Doe")
@EBook
class MyClass { ... }
~~~
如果注解有多个相似的类型, 那么这就是重复注解(repeating annotation): 
~~~
@Author(name = "Jane Doe")
@Author(name = "John Smith")
class MyClass { ... }
~~~
重复注解在Java SE 8版本后支持, 详情: [Repeating Annotations.](https://docs.oracle.com/javase/tutorial/java/annotations/repeating.html)
上面的Override和SuppressWarnings是Java中预定义的注解, 你自己也可以创建自己的注解. 
Java8之后注解几乎可以用在各种[奇奇怪怪的地方](https://docs.oracle.com/javase/tutorial/java/annotations/basics.html). 
## 声明一个注解类型[Declaring an Annotation Type](https://docs.oracle.com/javase/tutorial/java/annotations/declaring.html)
## 预定义的注解类型[Predefined Annotation Types](https://docs.oracle.com/javase/tutorial/java/annotations/predefined.html)
## 类型注解和可插拔类型系统[Type Annotations and Pluggable Type Systems](https://docs.oracle.com/javase/tutorial/java/annotations/type_annotations.html)
Java8之后, 注解可以用在类型上, 所以也叫类型注解. 它是为了支持对Java程序的改进分析, 更强的代码检查方式而出现的. Java8不提供类型检查框架, 它允许你编写或下载类型检查框架. 
大多数情况下你不需要手动写类型检查模块, 你可以使用[The Checker Framework](https://checkerframework.org/), 它帮你实现了防止空引用@NonNull, 互斥锁@mutex lock等模块. 
## 重复注解[Repeating Annotations](https://docs.oracle.com/javase/tutorial/java/annotations/repeating.html)

参考: [Annotations](https://docs.oracle.com/javase/tutorial/java/annotations/index.html)

