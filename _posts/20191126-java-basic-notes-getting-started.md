---
title: Java基础笔记--入门
date: 2019-11-26 
updated: 2020-04-30
tags:
  - Java
urlname: java-basic-notes-getting-started
original: false
---
关于Java技术, 官方给出的Hello World解读.
<!--more-->
最近迷茫一些事情, 虽然自学了SpringBoot之类的Java框架, 但是对Java语言的理解还是不算深入, 所以打算重拾一下Java.
## 关于Java技术
Java程序都是由.java源代码编译成.class字节码文件后运行在JVM虚拟机上的. 一些不同平台的JVM可能提供一些格外步骤来提高程序性能. Java平台是一个运行在其他基于硬件平台上的纯软件的平台, 它包括JVM和API. JVM是Java平台的基础, 并且移植到了很多基于硬件的平台上了. API是大量现成的软件组件的集合, 这些组件提供了许多有用的功能. Java平台是平台无关的环境, 所以会比本地代码慢一点点, 随着编译器和虚拟机技术进步而使得其性能更接近本地代码. 
## 官方给出的Hello World解读
它包括三部分, 源代码注释(source code comments), HelloWorldApp类定义和主方法. 
Java支持三种类型的注释:
~~~
/* 注释一 */
/** 文档注释 */
// 注释三
~~~
每个Java应用程序都以类定义开头:
~~~
class name{
    ...
}
~~~
主方法是程序的入口点, 习惯上主方法上的public顺序要先于static. 
~~~
public static void main(String[] args) {
    ...
}
~~~
主方法接收一个String数组, 作为程序接收用户命令的参数. 
方法里使用System类里标准输出"Hello World!". 

来源: [Getting Started](https://docs.oracle.com/javase/tutorial/getStarted/index.html)








