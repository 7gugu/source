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

~~~ java
List list = new ArrayList();
list.add("hello");
String s = (String) list.get(0);
~~~

如果使用泛型重写, 那么就不需要类型转换: 

~~~ java
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

~~~ java
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
~~~

由于方法接收或返回一个Object, 所以我们可以自由的传递我们想传递的对象, 除了基础类型. 但是这里没有方法在编译时验证程序如何使用这个类. 比如正常情况我们放了一个Integer进去, 那么我们想拿出来应该是个Integer, 但代码中某一部分不小心把Integer错拼成String了, 结果就导致了一个运行时错误. 

### 一个泛型版本的Box类

一个泛型类的定义格式差不多长这样: 

~~~ java
class name<T1, T2, ..., Tn> { /* ... */ }
~~~

在类型参数部分, 通过尖括号来限制, 然后接着类名. 它通过逗号将类型变量分隔. 

将Box类更新成使用泛型, 你需要改"public class Box"为"public class Box&lt;T>"来创建一个泛型类型声明. 这样就引进了类型变量T, 它可以在类内任何地方使用. 

经过上面的改变, Box类变成了: 

~~~ java
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

~~~ java
Box<Integer> integerBox;
~~~

你可以把泛型类型调用想做是一次普通的方法调用, 只是把传递参数给方法变成了传递一个类型参数. (在这个例子中是把Integer传给Box类)

注: Type Parameter 和 Type Argument 是有区别的, 但翻译的时候可能都会翻译成一个意思... 所以有时候还是看官方文档吧. 

和其他变量声明一样, 上面的代码没有真正创建一个新的Box对象. 它只是简单的说明了integerBox指向装有Integer的Box类的引用. 说明了Box&lt;Integer>如何读取. 

为了实例化这个类, 我们使用new关键词, 和之前一样, 但是需要把&lt;Integer>放在类名和末尾括号之间. 

~~~ java
Box<Integer> integerBox = new Box<Integer>();
~~~

### 钻石运算符The Diamond

JavaSE7之后, 构造器中的类型参数可以省略不写, 编译器会根据上下文来决定. 这种由两个尖括号组成的东西&lt;>非官方的叫法就叫钻石运算符了. 下面是例子: 

~~~ java
Box<Integer> integerBox = new Box<>();
~~~

### 多类型参数

之前好像也有提到一个泛型类可以有多个类型参数. 例如下面的OrderedPair泛型类, 实现了Pair接口:
 
~~~ java
public interface Pair<K, V> {
    public K getKey();
    public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {

    private K key;
    private V value;

    public OrderedPair(K key, V value) {
	this.key = key;
	this.value = value;
    }

    public K getKey()	{ return key; }
    public V getValue() { return value; }
}
~~~

下面的声明中创建了两个OrderedPair类的实例: 

~~~ java
Pair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8);
Pair<String, String>  p2 = new OrderedPair<String, String>("hello", "world");
~~~

上面的代码new OrderedPair&lt;String, Integer>可以看作使用String来实例了K, 使用Integer来实例化了V. 因此OrderedPair类构造器的参数类型是按顺序的String和Integer. 因为有自动装箱和拆箱, 传递String和int进去类中也是合法的. 

根据上面提及的The Diamond钻石操作符, 因为Java编译器可以从定义OrderedPair&lt;String, Integer>中获取对应的类型, 所以这些声明语句可以使用简短的钻石符号. 

~~~ java
OrderedPair<String, Integer> p1 = new OrderedPair<>("Even", 8);
OrderedPair<String, String>  p2 = new OrderedPair<>("hello", "world");
~~~

### 参数化的类型Parameterized Types

你可以把参数化过的类型类作为泛型类型使用, 例如: 

~~~ java
OrderedPair<String, Box<Integer>> p = new OrderedPair<>("primes", new Box<Integer>(...));
~~~

## 生(未分配)类型Raw Types

生类型是指没有任何类型参数的泛型类或者接口. 例如给定的Box类: 

~~~ java
public class Box<T> {
    public void set(T t) { /* ... */ }
    // ...
}
~~~

创建一个参数化过的Box&lt;T>类型, 你需要给形式参数T提供一个确切的类型参数: 

~~~ java
Box<Integer> intBox = new Box<>();
~~~

如果这个形参被省略了, 你就创建了一个Box&lt;T>的生类型: 

~~~ java
Box rawBox = new Box();
~~~

因此Box是属于泛型类型Box&lt;T>的生类型. 然而一个非泛型类或者接口不是一个生类型. 

这种生类型一般在一些遗留的代码中体现, 因为在JDK5.0之前很多API类(比如Collections类)不是泛型的. 当你使用生类型, 你将会遇到没有泛型时的问题: Box类会给你返回Object. 为了向后兼容性, 分配一个参数化过的类型给生类型是允许的: 

~~~ java
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;               // OK
~~~

但如果你分配一个生类型给一个参数化过的类型, 你将会得到一个警告: 

~~~ java
Box rawBox = new Box();           // rawBox is a raw type of Box<T>
Box<Integer> intBox = rawBox;     // warning: unchecked conversion
~~~

你使用一个生类型调用对应泛型类型的泛型方法你也会得到一个警告: 

~~~ java
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;
rawBox.set(8);  // warning: unchecked invocation to set(T)
~~~

警告显示这个生类型绕过了泛型检查, 将不安全的代码检查推迟到了运行时. 因此你应该避免在代码中使用生类型. 

### 未检查的错误信息Unchecked Error Messages

前面提到过, 当你与历史遗留的远古代码混合, 你可能会遇到一个一个类似这样的警告: 

~~~
Note: Example.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
~~~

在使用老的操作生类型的API时会产生上面的警告. 但一般都是建议不使用老的API, 这里就略了. 

## 泛型方法

泛型方法就是能引进他们自己的参数类型的方法. 这和定义泛型类型很像, 但类型参数的作用域仅限在声明的方法内. 静态和非静态泛型方法都是允许的, 泛型类构造器也一样. 

泛型方法的语法是, 在尖括号内可以包含一系列的类型参数, 这些东西要出现在方法返回类型前面. 对于静态泛型方法, 类型参数部分一定要出现在方法返回类型之前. 

下面的Util类包含一个泛型方法, compare, 它比较两个传入的Pair对象: 

~~~ java
public class Util {
    public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) &&
               p1.getValue().equals(p2.getValue());
    }
}

public class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}
~~~

调用上面内容的完整语法是这样的: 

~~~ java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.<Integer, String>compare(p1, p2);
~~~

上面的代码中, 调用方法时类型需要单独提供. 但一般情况那段代码可以不写, 编译器会知道怎么做的啦:

~~~ java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.compare(p1, p2);
~~~

这个特性叫做类型引用(type inference), 它允许你调用泛型方法和调用普通方法一样, 不需要写尖括号指定类型. 

## 限定类型参数Bounded Type Parameters

有时候你想限制泛型类或者方法的使用类型. 比如, 一个操作数字的方法只想接收Number类的实例或者它的子类. 这就是限定类型参数要做的事. 

定义一个泛型界限, 只需要在定义的泛型类型名后加extend关键字, 然后接着它的上界类型, 在这个例子中是Number. 注意了, 在这里说的extends是一个大概的意思, 也就是说其实这个类继承了, 或者实现了这个类的接口也是可以的. 

~~~ java
public class Box<T> {

    private T t;          

    public void set(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }

    public <U extends Number> void inspect(U u){
        System.out.println("T: " + t.getClass().getName());
        System.out.println("U: " + u.getClass().getName());
    }

    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<Integer>();
        integerBox.set(new Integer(10));
        integerBox.inspect("some text"); // error: this is still String!
    }
}
~~~

通过修改我们的泛型方法来引入这个限定泛型类型的功能, 编译的过程将会失败, 因为我们的inspect方法调用仍然使用着String类型, 而它所希望的是Number或者是Number的子类: 

~~~
Box.java:21: <U>inspect(U) in Box<java.lang.Integer> cannot
  be applied to (java.lang.String)
                        integerBox.inspect("10");
                                  ^
1 error
~~~

除了限定类型, 你可以实例化你的泛型类型, 也就是说限定了泛型类型之后这允许你调用界限内你可以调用的方法: 

~~~ java
public class NaturalNumber<T extends Integer> {

    private T n;

    public NaturalNumber(T n)  { this.n = n; }

    public boolean isEven() {
        return n.intValue() % 2 == 0; // 这里调用的intValue()方法是来自Integer的
    }

    // ...
}
~~~

上面的isEven方法通过n调用了定义在Integer类中的intValue方法. 

### 多重限定Multiple Bounds

上面的例子展示了使用一个限定来限定泛型参数类型, 但一个泛型参数可以被多重限定: 
~~~ java
<T extends B1 & B2 & B3>
~~~
一个被这样限定的泛型参数相当于是它们的子类型了. 如果它们其中一个是类, 那么它就必须放在前面, 例如: 

~~~ java
Class A { /* ... */ }
interface B { /* ... */ }
interface C { /* ... */ }

class D <T extends A & B & C> { /* ... */ }
~~~

如果A类没有放在前面, 那么你将会得到一个编译时错误: 
~~~
class D <T extends B & A & C> { /* ... */ }  // compile-time error
~~~

## 泛型方法和限定类型参数

限定参数类型是实现泛型算法的关键. 思考一下下面的计算数组anArray中大于指定elem的个数的方法: 

~~~ java
public static <T> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e > elem)  // compiler error
            ++count;
    return count;
}
~~~

这个方法的实现体非常简单, 但这并不能编译, 因为这个大于运算符只对基础类型short, int, double, long, float, byte和char有效. 你不能使用这个大于操作符来比较对象. 为了修复这个问题, 可以使用Comparable&lt;T>接口进行泛型参数限定: 

~~~ java
public interface Comparable<T> {
    public int compareTo(T o);
}
~~~

最终的代码将是: 

~~~ java
public static <T extends Comparable<T>> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e.compareTo(elem) > 0)
            ++count;
    return count;
}
~~~

## 泛型, 继承和子类型

你知道的, 将一个类型的对象分配给另外一个匹配的对象是可能的. 例如你可以将Integer对象分配给Object对象, 因为Object对象是Integer的其中一个超类: 

~~~ java
Object someObject = new Object();
Integer someInteger = new Integer(10);
someObject = someInteger;   // OK
~~~

在面向对象术语中, 这个叫棣属(is a)关系. 因为Integer是Object的一种所以这种分配是允许的. 但Integer也是Number的一种, 所以下面的代码也应该是合法的: 

~~~ java
public void someMethod(Number n) { /* ... */ }

someMethod(new Integer(10));   // OK
someMethod(new Double(10.1));   // OK
~~~

对泛型来说这也是成立的. 你可以执行一次泛型的类型调用, 传递Number类型作为它的类型参数, 然后今后的任何add方法的调用都是允许的, 只要参数和Number对应: 

~~~ java
Box<Number> box = new Box<Number>();
box.add(new Integer(10));   // OK
box.add(new Double(10.1));  // OK
~~~

现在考虑下面的方法: 

~~~ java
public void boxTest(Box<Number> n) { /* ... */ }
~~~

这个方法接收什么参数? 通过看他的方法签名, 你可以看到它接收一个 Box&lt;Number> 参数类型. 但是这是什么意思呢? 你允许传递 Box&lt;Integer> 或者 Box&lt;Double> 类型吗? 或者你是这么期望的? 但很可惜答案是"不", 因为 Box&lt;Integer> 和 Box&lt;Double> 不是 Box&lt;Number> 的子类类型. 

这是一个使用泛型编程时的常用的误解, 但是这是一个值得学习的重要的观念. 

![Box&lt;Integer>不是Box&lt;Number>的子类型, 尽管Integer是Number的子类型](/picture/20200720-0.png)

注: 给定两个不同的类型A和B(例如Number和Integer), MyClass&lt;A>和MyClass&lt;B> 没有关系, 不管A和B有没有关系. MyClass&lt;A> 和MyClass&lt;B>的父类都是Object. 

### 泛型类和子类型Generic Classes and Subtyping

你可以通过继承和实现来产生泛型类型的子类型. 泛型类型参数的关系是由extends和implement决定的. 

我们使用Collections类作为例子, ArrayList&lt;E>实现List&lt;E>, 而List&lt;E>继承了Collection&lt;E>. 所以ArrayList&lt;String>是List&lt;String>的子类型, 而它也是Collection&lt;String>的子类型. 

只要你不更改参数类型, 类型间的子类参数关系就一直保持这个样子. 

![Collections的结构层次](/picture/20200720-1.png)

现在想象一下我们准备定义我们自己的list接口, PayloadList, 它关联一个可选的泛型值P. 它的声明可能是这个样子的: 

~~~ java
interface PayloadList<E,P> extends List<E> {
  void setPayload(int index, P val);
  ...
}
~~~

然后下面的参数化了类型的PayloadList都是List<String>的子类哦: 
- PayloadList<String,String>
- PayloadList<String,Integer>
- PayloadList<String,Exception>

![PayloadList的结构层次](/picture/20200720-2.png)

## 类型推断Type Inference

类型推断是Java编译器根据方法调用和对应的类型声明来决定类型参数使得语句能合法执行的能力. (说着很晕大概就是你使用泛型时, 有时不用指定类型编译器会帮你做决策) 推断算法决定了后面的参数类型, 如果可用的话, 那么就会自动分配. 最后推断算法会尝试寻找到最合适的类型来匹配你没有填写的类型. 

为了说明最后一点, 在下面的例子中推断算法决定了第二个传进pick方法中的参数类型是Serializable. 因为ArrayList实现了Serializable接口所以这里没有问题. 

~~~ java
static <T> T pick(T a1, T a2) { return a2; } // 方法中声明了返回值是泛型T, 那个尖括号说明用到了泛型T
Serializable s = pick("d", new ArrayList<String>()); 
~~~

### 类型推断和泛型方法

泛型方法来给你介绍一下这个类型推断, 它使得你可以像调用普通方法一样调用泛型方法, 不需要加过多尖括号的修饰. 来看看下面的例子你就知道了: 

~~~ java
public class BoxDemo {

  public static <U> void addBox(U u, 
      java.util.List<Box<U>> boxes) {
    Box<U> box = new Box<>();
    box.set(u);
    boxes.add(box);
  }

  public static <U> void outputBoxes(java.util.List<Box<U>> boxes) {
    int counter = 0;
    for (Box<U> box: boxes) {
      U boxContents = box.get();
      System.out.println("Box #" + counter + " contains [" +
             boxContents.toString() + "]");
      counter++;
    }
  }

  public static void main(String[] args) {
    java.util.ArrayList<Box<Integer>> listOfIntegerBoxes =
      new java.util.ArrayList<>();
    BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);
    BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);
    BoxDemo.addBox(Integer.valueOf(30), listOfIntegerBoxes);
    BoxDemo.outputBoxes(listOfIntegerBoxes);
  }
}
~~~

上面的输出是: 

~~~
Box #0 contains [10]
Box #1 contains [20]
Box #2 contains [30]
~~~

上面的泛型方法addBox定义了一个泛型类型U. 一般来说Java编译器能够推断出泛型方法调用的类型参数. 因此在大多数情况下, 你不需要单独指定他. 例如上面的例子, 为了调用addBox方法你可以用尖括号指定泛型方法中的泛型类型为Integer: 

~~~ java
BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);
~~~

如果你忽略这个尖括号指定的类型, 那么Java编译器会自动推断(根据方法参数)那个类型参数是Integer: 

~~~ java
BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);
~~~

### 类型推断和实例化泛型类











参考: [Generics](https://docs.oracle.com/javase/tutorial/java/generics/index.html)