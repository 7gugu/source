---
title: Java基础笔记--学习Java语言--泛型
date: 2020-07-04
updated: 2020-07-04
tags:
  - Java
urlname: java-basic-notes-learning-the-java-language-generics
original: false
---
2.7 泛型
<!--more-->

# 为什么使用泛型? 

简单来说泛型使你在定义类, 接口和方法时可以把类型作为参数. 和熟悉的方法参数一样, 类型参数也提供一种重用代码的功能. 不一样的地方在于方法参数是值, 而这个输入参数是类型. 

使用泛型的优点: 

- 编译时的强类型检查
Java编译器对泛型使用强类型检查, 如果代码违背类型安全就会产生错误. 修复编译时错误比修复运行时错误容易得多. 

- 避免类型转换
下面的代码不适用泛型需要类型转换: 
~~~
List list = new ArrayList();
list.add("hello");
String s = (String) list.get(0);
~~~
如果使用泛型重写, 那么就不需要类型转换: 
~~~
List<String> list = new ArrayList<String>();
list.add("hello");
String s = list.get(0);   // no cast
~~~

- 程序员能实现泛型算法
例如针对不同类型的集合使用相同或不同的算法, 可自定义, 类型安全和容易阅读. 

# 泛型类型Generic Types

一个泛型类型是参数在类型之上的泛型类或接口. 下面的Box类会展示一下这种概念. 

### 一个简单的Box类

从观察一个可以操作任意类型的非泛型Box类开始. 它只需要提供两种方法: set()用来添加一个对象到box中, 还有get()来取回它: 
~~~
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
~~~
由于方法接收或返回一个Object, 所以我们可以自由的传递我们想传递的对象, 除了基础类型. 但是这里没有方法在编译时验证程序如何使用这个类. 比如正常情况我们放了一个Integer进去, 那么我们想拿出来应该是个Integer, 但代码中某一部分不小心把Integer错拼成String了, 结果就导致了一个运行时错误. 

### 一个泛型版本的Box类

一个泛型类的定义格式差不多长这样: 
~~~
class name<T1, T2, ..., Tn> { /* ... */ }
~~~
在类型参数部分, 通过尖括号来限制, 然后接着类名. 它通过逗号将类型变量分隔. 

将Box类更新成使用泛型, 你需要改"public class Box"为"public class Box<T>"来创建一个泛型类型声明. 这样就引进了类型变量T, 它可以在类内任何地方使用. 

经过上面的改变, Box类变成了: 
~~~
/**
 * Generic version of the Box class.
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
~~~

你可以看到, 所有出现的Object被T取代了. 一个类型变量可以是任何你声明的非基础类型类型: 任何类类型, 任何接口类型, 任何数组类型, 或者甚至是其他类型变量. 

同样的方法可以用来创建泛型接口. 

### 类型参数命名惯例Type Parameter Naming Conventions

按照惯例, 类型参数以单个大写字母命名. 这种命名和我们之前了解的命名形成鲜明对比, 而且有好的原因: 没有这个惯例, 我们将很难判断类型变量, 普通类和接口名. 

最常用的类型参数名是: 
- E - 元素Element(广泛的用于Java集合框架)
- K - 键Key
- N - 数字Number
- T - 类型Type
- V - 值Value
- S, U, V 等等. - 第二, 第三, 第四类型

你可以看到这些名字在Java SE API每个部分几乎都用到了. 

### 调用和定义一个泛型类型Invoking and Instantiating a Generic Type

为了在你的代码中使用这个泛型Box, 你必须执行一条泛型类型调用, 就是把T用一些确定的值替代掉, 比如Integer: 
~~~
Box<Integer> integerBox;
~~~
你可以把泛型类型调用想做是一次普通的方法调用, 只是把传递参数给方法变成了传递一个类型参数. (在这个例子中是把Integer传给Box类)

注: Type Parameter 和 Type Argument 是有区别的, 但翻译的时候可能都会翻译成一个意思... 所以有时候还是看官方文档吧. 

和其他变量声明一样, 上面的代码没有真正创建一个新的Box对象. 它只是简单的说明了integerBox指向装有Integer的Box类的引用. 说明了Box<Integer>如何读取. 

为了实例化这个类, 我们使用new关键词, 和之前一样, 但是需要把<Integer>放在类名和末尾括号之间. 
~~~
Box<Integer> integerBox = new Box<Integer>();
~~~



参考: [Generics](https://docs.oracle.com/javase/tutorial/java/generics/index.html)