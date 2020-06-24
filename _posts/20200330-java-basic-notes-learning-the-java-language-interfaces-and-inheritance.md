---
title: Java基础笔记--学习Java语言--接口和继承
date: 2020-03-30 
updated: 2020-04-30
tags:
  - Java
urlname: java-basic-notes-learning-the-java-language-interfaces-and-inheritance
original: false
---
2.5 接口, 继承, 方法的覆盖和隐藏, 多态, 
<!--more-->

# 接口Interfaces

## Java中的接口

Java中接口是引用类型, 和类很类似, 它只能包含常量, 方法名, 默认方法, 静态方法和嵌套类型. 只有默认方法和静态方法有方法体. 接口不能被实例化, 只能被类实现或者其他接口继承. 默认方法举例: 
~~~
public interface InterfaceTest {
    default void test() {
        // do something
    }
}
~~~

### 定义一个接口

和类继承不同, 一个接口可以继承多个接口: 
~~~
public interface GroupedInterface extends Interface1, Interface2, Interface3 { 
    // interface body
}
~~~
接口可以包含抽象方法, 默认方法, 静态方法. 抽象方法以分号结束(因为抽象方法不需要包含实现). 所有抽象方法, 默认方法, 静态方法在接口里都隐式的声明为public, 因此在声明时可以忽略public修饰符. 同样, 接口里定义的常量都隐式的声明为public, static和final. 在接口里只有默认方法和静态方法是需要接口实现的, 其余的方法在类实现该接口时都要实现. 

### 接口的发展Evolving Interfaces

如果你想在已经被类实现的接口中扩充方法, 你可以新建一个接口继承至该接口, 或者直接在接口里添加default方法. 

### 默认方法

[这篇文章](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)用时区, 模拟扑克牌例子来说明了如何使用默认方法来扩展程序, 在扑克牌例子中还使用了lambda表达式和方法引用. 

# 继承Inheritance

## Java中的继承

当你想创建一个新类时, 已经有一个类包含了你想要的代码, 你可以继承这个已经存在的类. 继承之后你可以复用一些父类的成员变量而不需要重新写. 子类继承所有父类字段(成员变量, 成员方法, 嵌套类等, 前提是父类的成员被设计为public或者protected, 如果不在同一包下, private字段是无法被继承的), 而构造器并不属于这个范围, 但子类可以调用父类构造器. 

### 对象转换

假设我们有这么一个类结构: Father类继承Object类, Son类继承Father类. 我们可以: 
~~~
Object obj = new Son();
~~~
此时Object类型变量obj指向Son类对象的引用(隐式转换), 此时obj既是Object又是Son, 也是Father. 
如果此时我们写: 
~~~
Son son = obj;
~~~
编译器会抛出一个运行时错误, 除非你告诉编译器要强转: 
~~~
Son son = (Son)obj;
~~~
你也可以使用instanceof来测试一下特定的对象是否是某个类, 它可以避免运行时错误: 
~~~
    if(obj instanceof Son) {
        Son son = (Son) obj;
    }
~~~

### 关于类的多重继承

类和接口的重大区别在于类可以拥有自己的字段. 你可以通过类直接创建一个对象而接口不能. 我们之前说过, 对象可以在字段中保存在类中定义过的状态. 一个Java语言为什么不允许类多继承的原因是避免对象的状态问题, 也就是类继承多个类字段的能力. 例如: 假设你能够定义一个能继承多个类的类. 当你实例化这个类时, 这个对象会继承父类的所有字段, 那么如果来自多个父类的方法和构造函数如何初始化同名的字段呢? 那个方法或者构造函数会执行? 因为接口不包含字段, 所以你不需要担心多继承的对象状态问题. 

Java支持类型的多继承, 也就是一个类能实现多接口的能力. 一个对象可以有多种类型, 一个是它自己的类型还有它实现的接口类型. 这也就意味着如果一个变量声明为接口类型, 那么它就可以引用实现了该接口的对象. 如果不同接口的方法冲突, 继承的类可以选择使用来自那个接口的方法. 

## 方法覆盖和方法隐藏Overriding and Hiding Methods

### 实例方法

子类方法如果和父类方法的方法签名(方法名加上同参数数量和类型)和返回值与父类一样, 那么子类的方法就会覆盖父类的方法. 

子类覆盖父类方法的能力允许它继承一个行为相近的父类, 并根据需要重写该方法. 被覆盖的方法在子类的返回值可以是原返回值的子类, 这个子类型叫协变返回类型. 

每当覆盖一个方法时, 你需要使用@Override注解来告诉编译器你想覆盖一个父类方法. 如果编译器检查发现父类没有这个方法就会产生一个编译错误. (有时候使用eclipse时覆盖不写@Override也行是因为Java语言等级(language level)的问题. 每一代JDK都有它的新特性, Java语言等级可以理解成编译器检查时的最低要求, @Override注解在不同语言等级之间可能不是强制的, 为了方便理解最好还是写上)

### 静态方法

如果子类定义了一个方法签名和父类一样的静态方法, 那么子类的方法会隐藏父类的方法. 

区分静态方法的隐藏和实例方法的覆盖的区别具有重要的含义: 
- 覆盖的实例方法的调用是取决于你实例出来的对象. 
- 隐藏的静态方法的调用是取决于你从父类还是子类去调用. 

官方指南写了一个例子来说明两者间的区别, 首先有一个Animal类: 
~~~
public class Animal {
    public static void testClassMethod() {
        System.out.println("The static method in Animal");
    }
    public void testInstanceMethod() {
        System.out.println("The instance method in Animal");
    }
}
~~~
Cat类继承Animal类: 
~~~
public class Cat extends Animal {
    public static void testClassMethod() {
        System.out.println("The static method in Cat");
    }
    public void testInstanceMethod() {
        System.out.println("The instance method in Cat");
    }

    public static void main(String[] args) {
        Cat myCat = new Cat();
        Animal myAnimal = myCat;
        Animal.testClassMethod();
        myAnimal.testInstanceMethod();
    }
}
~~~
Cat类覆盖了Animal类的实例方法和隐藏了Animal的静态方法. 在main方法里创建了一个Cat对象分别调用两个方法, 输出结果: 
~~~
The static method in Animal
The instance method in Cat
~~~
所以方法隐藏指的是当父类类型引用子类对象时, 调用了子类覆盖的方法那么就会调用到子类的方法, 父类的则被隐藏了. 当然这是不好的编程习惯. 

### 接口方法

接口中的默认方法和抽象方法的继承类似于实例方法. 然而, 当超级类或者接口提供多个方法签名相同的方法时, Java编译器会根据一下两个原则来解决方法名冲突的问题: 
- 1. 实例方法要优先于接口默认方法
官方举例: 
~~~
public class Horse {
    public String identifyMyself() {
        return "I am a horse.";
    }
}
public interface Flyer {
    default public String identifyMyself() {
        return "I am able to fly.";
    }
}
public interface Mythical {
    default public String identifyMyself() {
        return "I am a mythical creature.";
    }
}
public class Pegasus extends Horse implements Flyer, Mythical {
    public static void main(String... args) {
        Pegasus myApp = new Pegasus();
        System.out.println(myApp.identifyMyself());
    }
}
~~~
方法Pegasus.identifyMyself()会返回I am a horse.

- 2. 已经被其他子类覆盖的方法将会被忽略. 这通常发生在实现的多个接口拥有共同的祖先. 
官方举例: 
~~~
public interface Animal {
    default public String identifyMyself() {
        return "I am an animal.";
    }
}
public interface EggLayer extends Animal {
    default public String identifyMyself() {
        return "I am able to lay eggs.";
    }
}
public interface FireBreather extends Animal { }
public class Dragon implements EggLayer, FireBreather {
    public static void main (String... args) {
        Dragon myApp = new Dragon();
        System.out.println(myApp.identifyMyself());
    }
}
~~~
方法Dragon.identifyMyself()会返回I am able to lay eggs.
如果两个或者多个独立定义的默认方法冲突, 或者默认方法和抽象方法冲突, 你必须明确覆盖该方法否则编译器会产生错误. 
官方举例: 
~~~
public interface OperateCar {
    // ...
    default public int startEngine(EncryptedKey key) {
        // Implementation
    }
}
public interface FlyCar {
    // ...
    default public int startEngine(EncryptedKey key) {
        // Implementation
    }
}
~~~
如果一个类实现上面两个接口则必须覆盖startEngine方法, 你可以用super关键词来指定调用哪个接口的默认方法. 
~~~
public class FlyingCar implements OperateCar, FlyCar {
    // ...
    public int startEngine(EncryptedKey key) {
        FlyCar.super.startEngine(key);
        OperateCar.super.startEngine(key);
    }
}
~~~
在关键词super之前必须指明需要使用的父接口. 该关键词也可以用在类中, 你也可以直接用super而前面不加父接口名, 这样会调用到父类的方法而不是接口的方法. 
从类继承实例方法会覆盖接口的抽象方法, 官方例子: 
~~~
public interface Mammal {
    String identifyMyself();
}
public class Horse {
    public String identifyMyself() {
        return "I am a horse.";
    }
}
public class Mustang extends Horse implements Mammal {
    public static void main(String... args) {
        Mustang myApp = new Mustang();
        System.out.println(myApp.identifyMyself());
    }
}
~~~
方法Mustang.identifyMyself 会返回I am a horse. 
注: 接口的静态方法不能被继承. 

### 修饰符Modifiers

被覆盖的方法的修饰符的能见度应该不小于被覆盖方法的修饰符. 例如一个父类protected方法在被覆盖后可以被修饰为public, 但不能修饰为private. 

### 总结

下面的表格总结了当你定义了一个父类同方法签名方法时会发生什么. 
<table>
<tr>
<th>&nbsp;</th>
<th>Superclass Instance Method</th>
<th>Superclass Static Method</th>
</tr>
<tr>
<th>Subclass Instance Method</th>
<td>Overrides</td>
<td>Generates a compile-time error</td>
</tr>
<tr>
<th>Subclass Static Method</th>
<td>Generates a compile-time error</td>
<td>Hides</td>
</tr>
</table>
注: 在子类, 你可以重载父类方法, 重载的方法既不覆盖也不隐藏父类的方法, 他们是独一无二的新的方法. 

## 多态Polymorphism

字典里的多态性定义源于一个生物原理, 一个生物或者物种可以具有不同的形式或者阶段. 这一原则也可以适用于面向对象编程和语言. 一个类的子类可以定义自己的独特行为, 也可以共享父类的某些相同功能. 

官方用[自行车的例子](https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html)来展示了Java中的多态. 

他通过山地车和公路自行车都继承了自行车类, 并根据各自不同的特点来改写输出状态的方法. 然后在测试类中用自行车类型来引用不同的对象, 调用同样的方法输出不同的结果. 

他解释到JVM会根据变量中引用的对象来调用合适的方法. 它不是调用变量类型定义的方法. 这种行为称作虚拟方法调用(virtual method invocation). 

## 隐藏字段Hiding Fields

在一个类中, 有与父类相同字段名的字段会隐藏父类的字段, 尽管类型并不相同. 在子类, 父类的字段不能直接用简单的名字来引用, 必须通过super关键词来访问. 一般来说是不建议隐藏字段的, 因为这会使得代码难以阅读. 

## 使用super关键词

### 访问父类成员

如果你覆盖了父类的方法, 你可以在子类用super调用父类被覆盖的方法. 同样你也可以用它来访问你覆盖的字段名. 
官方举例: 
~~~
public class Superclass {
    public void printMethod() {
        System.out.println("Printed in Superclass.");
    }
}
~~~
这是子类, 覆盖了父类的方法, 并在覆盖的方法中调用了父类被覆盖的方法:  
~~~
public class Subclass extends Superclass {

    // overrides printMethod in Superclass
    public void printMethod() {
        super.printMethod();
        System.out.println("Printed in Subclass");
    }
    public static void main(String[] args) {
        Subclass s = new Subclass();
        s.printMethod();    
    }
}
~~~
输出结果: 
~~~
Printed in Superclass.
Printed in Subclass
~~~

### 子类构造器Subclass Constructors

子类构造器在调用父类构造方法时必须写在构造器第一行, 语法是: 
~~~
super();
~~~
或者带参数的构造方法: 
~~~
super(parameter list);
~~~
在super()里, 父类的无参构造器会被调用, 而有参的构造会调用父类匹配上的有参构造. 
注: 如果构造器没有显示调用父类构造器, 编译器会自动的给你插入一个调用父类无参构造. 如果父类没有无参构造, 那么我们就会得到一个编译时错误. 而Object对象是有这么一个空构造函数的. (虽然看源代码好像没有显式的写出来)

如果一个子类构造器调用一个父类构造器, 不管显式还是隐式调用, 你可能会觉得这像是一条构造器链, 无论那条路最终都会调用到Object类的构造器. 官方说事实就是这样的...这个叫构造函数链(constructor chaining), 当这条链比较长时你就要注意一下啦. 

## 作为父类的Object

在java.lang包里的Object类, 坐在所有类层次树的顶端. 每个类都是他的后代, 直接或者不直接. 每个你用或者写的类, 都继承Object的方法. 你不需要使用任何这些方法, 但是如果你选择使用它, 那么你可能需要在你的特定类覆盖它. 在本章将讨论继承自Object的方法. 
- protected Object clone() throws CloneNotSupportedException
创建和返回一个对象复制品
- public boolean equals(Object obj)
表明对象是否"equal to"另外一个对象
- protected void finalize() throws Throwable
当垃圾收集器garbage collection 确定没有引用这个对象时, 该对象的这个方法被垃圾收集器调用
- public final Class getClass()
返回一个对象的运行时类
- public int hashCode()
返回该对象的hash code值
- public String toString()
返回一个对象的字符串表示

对象的notify, notifyAll, 和wait方法都参与同步程序的线程活动, 它们将会在后面的章节讨论而不是这里. 他们有五个这种方法: 
- public final void notify()
- public final void notifyAll()
- public final void wait()
- public final void wait(long timeout)
- public final void wait(long timeout, int nanos)
注: 这些方法都有微妙的方面, 特别是clone方法. 

### clone()方法

如果一个类或者它其中一个父类实现了Cloneable接口, 你可以使用clone()方法来给现存的对象创建一个复制品. 创建一份克隆内容, 使用: 
~~~
aCloneableObject.clone();
~~~
这个方法的对象实现检查调用clone()方法的对象是否实现Cloneable接口. 如果对象没有, 方法则抛出一个CloneNotSupportedException异常. 异常处理会在以后的内容介绍. 目前, 如果你需要重写clone()方法, 你只需要知道clone()必须声明为: 
~~~
protected Object clone() throws CloneNotSupportedException
~~~
或者
~~~
public Object clone() throws CloneNotSupportedException
~~~
如果调用clone()方法的对象实现了Cloneable接口, 那么clone()方法的对象实现会创建一个和原类相同的对象, 初始化新对象的成员变量对应的值和调用的对象的值一样. 
最简单的方式让你的类可克隆就是添加实现Cloneable的接口到你的类声明上. 然后该类创建的对象就能调用clone()方法. 
对于某些类, Object类的克隆方法的默认行为可以正常工作. 如果, 一个对象包含一个外部对象引用, 假设叫ObjExternal, 你可能需要覆盖clone()方法使其正常工作. 否则对ObjExternal对象的更改在另一个克隆对象也会可见. (也就是浅克隆, ObjExternal对象并没有再克隆一份, 原克隆对象和克隆对象引用同一个ObjExternal) 这意味着原对象和它克隆出的对象都不是独立的. 如果要解耦它们, 你必须重写clone()然后它才克隆对象和ObjExternal. 然后原对象引用一个ObjExternal, 克隆出的对象引用不同的ObjExternal对象. 然后对象和克隆才算是真正独立了. 

### equals()方法(注意这里末尾有个小s)

equals()方法比较两个对象是否相等, 如果相等返回true. 在Object里提供的equals是使用==操作符来决定两个对象是否相等. 对于原始数据类型这将给出正确的结果. 但对于对象来说不是这样的. 对于对象来说equals()方法是比较两个引用是否相同, 如果相同那么就是引用同一个对象. 
想要知道两个对象(对象内的信息也相同)是否相同, 你必须重写equals()方法. 这里有一个Book类覆盖equals()方法: 
~~~
public class Book {
    ...
    public boolean equals(Object obj) {
        if (obj instanceof Book)
            return ISBN.equals((Book)obj.getISBN()); 
        else
            return false;
    }
}
~~~
思考一下代码, 它测试两个Book类的实例是否相等: 
~~~
// Swing Tutorial, 2nd edition
Book firstBook  = new Book("0201914670");
Book secondBook = new Book("0201914670");
if (firstBook.equals(secondBook)) {
    System.out.println("objects are equal");
} else {
    System.out.println("objects are not equal");
}
~~~
这个程序显示"objects are equal"尽管两个对象引用并不相等. 它们被认为是平等的因为两个对象包含同样的ISBN号码. 
你应该总是覆盖你的equals()方法, 如果==操作对你的类不合适. 
注: 如果你覆盖了equals(), 你最好也覆盖hashCode(). 

### finalize()方法

Object对象提供一个回调函数, finalize()对就是这个玩意, 它将会在一个将被回收的对象调用. Object的finalize()方法默认啥也不干, 你可以覆盖他来做一些清理工作, 比如释放资源. 
finalize()方法也可能自动的被系统调用, 但是什么时候调用, 甚至是调用了都是不确定的. 因此你不应该以来这个方法去做一些清洁工作. (真想捶死出教程这个人前面还说可以的) 例如, 在你执行文件I/O操作之后没有关闭文件描述符, 你期望finalize()去关闭它, 你可能就用完了文件描述符. (文件描述符file descriptors简单来说就是操作系统会创建一个实体来代表文件并存储这个打开了的文件的信息. 你打开100个系统就创建100个, 通常创建在系统核心. 通常实体会有非负的整数, 比如(100, 101, 102...)这些实体数字就是文件描述符. 同样当你打开一个网络套接字network socket也有相应的叫套接字描述符Socket Descriptor的东西)

### getClass()方法

你不能覆盖getClass. 

这个getClass()方法返回一个Class对象, 你可以拿这个Class对象获得一些类的信息, 例如getSimpleName()获得类名, getSuperclass()获得父类, getInterfaces()获得它实现的接口. 例如, 下面的方法得到和显示一个对象的类名: 
~~~
void printClassName(Object obj) {
    System.out.println("The object's" + " class is " +
        obj.getClass().getSimpleName());
}
~~~
这个在java.lang包中的Class类由非常多的方法(大于50). 例如, 你可以使用isAnnotation()测试看看这个类是不是注解, 用isInterface()测试是不是接口, 或者isEnum()是不是枚举. 你可以看这个对象的字段是啥getFields(), 方法有啥getMethods(), 等等. 

### hashCode()方法

由hashCode()方法返回的值是对象的hash code, 是对象在内存中的地址(十六进制). 
根据定义, 如果两个对象是相等的, 那么他们的hash code必须相同.(它的意思是对象内容相同地址不同也算对象相同, 说了是两个对象嘛) 如果你覆盖了equals()方法, 更改了两个对象的相等方式, 那么hashCode()方法Object的实现不再有效. 因此你覆盖了equals()方法, 你必须也覆盖hashCode()方法. (扩展Hash Table和Hash Map)

### toString()方法

你在类中应该总是考虑一下覆盖toString()方法. 
对象的toString()方法返回一个String代表这个对象, 这个对于debug非常有用. 对象的String表示完全依赖于对象, 这就是为什么你需要覆盖toString()的原因. 
你可以随着System.out.println()使用toString来显示一个对象的文字代表. 例如Book的一个实例: 
~~~
System.out.println(firstBook.toString());
~~~
你应该在toString写一些有用的对象内容比如: 
~~~
ISBN: 0201914670; The Swing Tutorial; A Guide to Constructing GUIs, 2nd Edition
~~~

## 写Final类和方法

你可以声明一些或者所有类的方法为final. 你使用final修饰一个方法代表它不能被子类覆盖. Object类里有一些final方法. 
你可能希望使用final来使一个方法不被改变并且保持严格的一致状态. 例如, 你可能想做一个getFirstPlayer 在ChessAlgorithm 类中: 
~~~
class ChessAlgorithm {
    enum ChessPlayer { WHITE, BLACK }
    ...
    final ChessPlayer getFirstPlayer() {
        return ChessPlayer.WHITE;
    }
    ...
}
~~~
被构造器调用的方法通常最好定义成final. 如果一个构造器调用一个非final方法, 子类可能重新那个方法, 但可能会有一些意想不到的后果. 
你可以定义一个实体类为final, 但是这种类无法被子类继承. 这挺有用的, 比如当创建一个像String类一样不变的类时. 

## 抽象方法和类

一个抽象类是一个被abstract修饰符修饰的类, 它可能包括或者不包括抽象方法. 抽象类无法被实例化但是可以被继承. 
一个抽象方法是定义了然后没有实现的方法. (没有大括号, 只是后面跟着一个分号)比如: 
~~~
abstract void moveTo(double deltaX, double deltaY);
~~~
如果一个类有抽象方法, 那么类自身必须定义为抽象的, 比如: 
~~~
public abstract class GraphicObject {
   // declare fields
   // declare nonabstract methods
   abstract void draw();
}
~~~
当子类继承一个抽象类, 通常子类必须实现所有的父类抽象方法. 如果它没有完全实现, 那也得定义为abstract. 
注: 在接口里的方法没有定义为default或者static都是隐式abstract的, 所以abstract修饰符在定义接口方法时不是很必要. 你可以用但是不需要. 

### 抽象类和接口的比较

抽象类和接口非常像. 你不能实例化它们, 而且包含了声明了但没有实现或者实现了的方法. 然而, 对于抽象类你可以定义不是静态和final的字段, 而且还能定义public, protected, private的具体方法. 对于接口, 所有字段默认是public, static和final, 而且所有方法都是public的. 此外, 你只能继承一个方法, 不管他是否被abstract修饰, 而你可以实现任何数量的接口. 
我们应该使用哪一个呢? 抽象类还是接口? (疑问脸)
- 如果符合下面条件可以考虑用抽象类: 
  - 你想在几个紧密相关的类中共享代码. 
  - 你希望扩展抽象类由非常多常见的方法或字段, 或者需要访问修饰符不是public(例如protected和private)
  - 你想定义非静态或非final字段. 这个使你能够定义方法来修改一些属于对象自己的状态. 
- 如果符和下面这些条件可以考虑用接口: 
  - 你希望不相干的类能实现你的接口. 例如接口Comparable 和 Cloneable被很多不想关的类实现. 
  - 你想指定特定数据类型的行为, 但不关心谁实现这些行为. 
  - 你希望利用多继承. 
  
一个在JDK里的抽象类例子是AbstractMap, 它是集合框架中的一部分. 它的子类(包括HashMap, TreeMap, ConcurrentHashMap)共享了很多AbstractMap定义的方法(包括get, put, isEmpty, containsKey, containsValue). 
一个JDK中实现了很多接口的类是HashMap, 实现了Serializable, Cloneable, 和Map<K, V>. 根据这些接口你可以判断一个HashMap(不管哪个开发者或者公司实现这个类. 其实java有标准, 然后有很多种java实现比如Oracle的或者OpenJDK)的实例是可以克隆的, 可序列化的(意思是可以转成字节流), 具有map功能. 此外, Map<K, V> 接口已经通过许多默认方法得到了增强, 比如merge和forEach, 而曾经实现了这个接口的旧类就不需要定义啦. 
注意很多软件库抽象类和接口都是用. 那个HashMap类实现很多接口, 但也继承抽象类AbstractMap. 

### 一个抽象类例子

在一个面向对象的画图应用中, 你可以画圆, 矩形, 线, 贝兹曲线Bezier curves和非常多的图形对象. 他们都有确定的状态(例如: 位置, 方向, 线条颜色, 填充色)和共同行为(比如: moveTo, 旋转, 改变大小, 绘画). 其中一些状态和行为对于图像对象来说是一样的. (例如: 位置, 填充色和倒向moveTo?) 其他需要不同的实现. (比如重置大小或绘画) 所有GraphicObject应该必须能够重置大小或绘画. 它们不同的地方是如何做. 这是抽象父类理想的情况. 你可以利用相同点定义在一个对象中, 并让子类继承这个抽象对象. 例如下图的GraphicObject. 
![GraphicObject结构图](/picture/20200505-0.jpg)
首先你定义一个抽象类, GraphicObject, 来提供一些所有子类都应该共有的一些方法和成员变量. 而且也提供一些应该由不同子类以不同方式实现的抽象方法. 这个对象很可能是这样的: 
~~~
abstract class GraphicObject {
    int x, y;
    ...
    void moveTo(int newX, int newY) {
        ...
    }
    abstract void draw();
    abstract void resize();
}
~~~
每个非抽象的子类必须提供draw()和resize()的实现, 例如下面的: 
~~~
class Circle extends GraphicObject {
    void draw() {
        ...
    }
    void resize() {
        ...
    }
}
class Rectangle extends GraphicObject {
    void draw() {
        ...
    }
    void resize() {
        ...
    }
}
~~~

### 当一个抽象类实现了一个接口

在前面的章节我们知道了实现了接口的类必须实现接口里所有的方法. 这是可能的情况, 然而如果一个类没有实现所有的接口方法, 那这个类需要被定义为抽象的. 
~~~
abstract class X implements Y {
  // implements all but one method of Y
}

class XX extends X {
  // implements the remaining method in Y
}
~~~
在上面的例子中X必须为abstract因为它没有完全实现Y里面的方法. 但是类XX完成了实现Y中方法的使命. 

### 类成员

一个抽象类可能含有静态字段和静态方法. 你可以使用类名加静态方法和其它类一样直接调用该方法. (比如: AbstractClass.staticMethod())

## 总结

除了Object类之外, 所有类都有它的直接父类. 一个类继承父类的所有字段和方法无论直接或者间接. 子类可以覆盖它继承的方法或者隐藏它. (隐藏字段通常是不好的编程习惯)
Object类是整个类层级的顶部. 所有类都是Object类的子孙后代并且继承它的方法. 来自Object类中常用的方法包括toString(), equals(), clone(), 和 getClass().
你可以使用final来避免一个类被继承. 同样的你可以使用final来避免一个方法在子类被重写. 
抽象类仅能被继承不能直接被实例化. 一个抽象类可以包含抽象方法--那些声明了但没有实现的方法. 子类提供抽象方法的实现. 

参考: [Interfaces and Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/index.html)