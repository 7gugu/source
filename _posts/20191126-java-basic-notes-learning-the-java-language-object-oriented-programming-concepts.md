---
title: Java基础笔记--学习Java语言--面向对象概念
date: 2019-11-26
updated: 2020-04-30
tags:
  - Java
urlname: java-basic-notes-learning-the-java-language-object-oriented-programming-concepts
original: false
---
什么是对象? 类? 继承? 接口? 包? API文档.
<!--more-->
## 什么是对象?
简单来说现实中的一个对象包括状态(state)和行为(behavior),在Java中对象的状态存在字段Fields(其他语言叫变量variables)里,对象的行为存在方法Methods(其他语言叫函数function)里,方法基于对象内部状态来操作, 用作对象间通讯的主要机制. 隐藏内部状态和并要求通过对象方法对状态进行交互叫做**数据封装**--它是面向对象程序的基本法则.

## 什么是类?
官方指南中利用自行车和自行车蓝图来对类和对象进行举例, 来说明类和对象.  一辆用自行车蓝图做出的自行车, 用面向对象的语言来说,  这辆自行车是自行车蓝图这个类的一个实例(instance). 类不是一个程序,而是供其他类调用的一个蓝图,而通过这个蓝图创建一个对象
就是属于其他类的责任了. 

## 什么是继承?
每个对象之间都有一定的共同点, 官方文档依旧拿自行车举例, 自行车有很多种, 山地车,公路自行车和双人车等, 它们有共同的特点和不同点, 可以将它们共同的特点单独抽出作为一个自行车超类(superclass), 而山地车, 公路自行车和双人车等类可以使用关键字extends继承超类(superclass). 这样就形成了一种类的等级制度, Java中一个类可以被无数个类继承,但一个子类只能继承一个类(单继承).


## 什么是接口?
在上面的例子中, 方法就是对象内部和外部世界的一个接口. 在Java中声明的接口中的方法形式上是一组没有身体的方法. 这么做的目的, 是为了让实现接口的类对它承诺提供的功能更加正式, 所以接口更像是类和外部世界的一个契约. 也就是存储一些承诺提供的功能. 

## 什么是包?
简单理解就是同类的东西放进同类的文件夹里, Java把同类的类放进相同的文件夹里, 那个文件夹就相当于是包, 因为程序会越写越大, 所以组织好各种类和接口放进包里是很有意义的. 

Java平台提供一个巨大的类库(API), 它的程序包代表编程中一些最常用的功能, [Java平台API规范](https://docs.oracle.com/javase/8/docs/api/index.html)包含了Java SE平台提供的所有软件包, 接口, 类, 字段和方法. 

来源: [Object-Oriented Programming Concepts](https://docs.oracle.com/javase/tutorial/java/concepts/index.html)








