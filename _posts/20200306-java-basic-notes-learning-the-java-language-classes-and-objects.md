---
title: Java基础笔记--学习Java语言--类和对象
date: 2020-03-06 
updated: 2020-04-30
tags:
  - Java
urlname: java-basic-notes-learning-the-java-language-classes-and-objects
original: false
---
2.3类, 对象, 初始化块, 嵌套类, Lambda表达式
<!--more-->
# 类Classes
## 声明一个类
~~~
class MyClass extends MySuperClass implements YourInterface {
    // field, constructor, and
    // method declarations
}
~~~
类前面还能加public或private之类的修饰符来决定哪些类能访问它. 
## 声明成员变量
在类中的变量叫成员变量(fields).
在方法或块中的变量叫本地变量(local variables).
在方法定义时的变量叫参数(parameters).
public修饰符意味着其他对象可以可以访问这个类. 
private修饰符意味着该field只能在该类中访问. 
每个变量都需要声明类类型, 变量的命名也是有一套规则的. 
## 定义一个方法
根据约定, 方法名应该是一个以小写开头的动词或多个单词的驼峰命名方式. 
通常方法在类中有独一无二的名字, 除非方法重载(method overloading同名但不同参数). 
Java通过不同的方法签名(method signatures)来区分不同的方法, 它是根据参数数量和类型来区分的. 
## 为类提供构造方法(Constructors)
构造方法可以看成是没有返回值的和类同名的方法. 
编译器会自动给没有构造方法的类添加一个无参构造方法, 如果其父类无参, 编译器也会提供, 这里还得重点了解一下super关键字. 子类构造函数第一行必须使用super来调用父类构造方法. 

## 参数传递
可以传递任何类型的参数. 除非写构造方法和set方法, 参数名不应该和成员变量名(field)重名, 成员变量名尽量不使用类似x, y这种命名. 区分和成员变量重名的参数需要了解一下this关键字. 
### 任意数量参数传递
当你不知道传递参数的个数, 可以使用可变参数(varargs), 可变参数可以像数组一样被使用, 你可能经常看到printf函数就使用了可变参数. 可变参数一般放在参数list的最后. 它可以是0个. 
~~~
public PrintStream printf(String format, Object... args)
~~~
### 传递基础数据类型
传递基础数据类型是值传递, 在方法内修改该参数不会对原参数造成影响. 例如修改传过来的int, double类型. 
### 传递引用数据类型
传入的引用仍然引用之前的同一个对象, 如果有合适的访问权限它能改变成员变量的值. 
~~~
public void moveCircle(Circle circle, int deltaX, int deltaY) {
    // code to move origin of circle to x+deltaX, y+deltaY
    circle.setX(circle.getX() + deltaX);
    circle.setY(circle.getY() + deltaY);
        
    // code to assign a new reference to circle
    circle = new Circle(0, 0);
}
~~~
例如上面的第7行代码, 只是代表circle指向了新的对象, 而不是传入的那个对象原来的引用发生了改变. 

# 对象Objects
## 创建对象
这里有一句语句:
~~~
Point originOne = new Point(23, 94);
~~~
它由三部分组成, 一是声明, 声明变量名; 二是实例化对象, 上面的new关键词便是创建一个对象; 三是初始化, new之后会调用构造方法, 用来初始化一个新的对象. new运算符返回对象的引用(reference), 有时候使用new不一定必须分配给指定变量, 例如: 
~~~
int height = new Rectangle().height;
~~~
上面那种情况是new关键词创建了一个对象, 然后返回该对象的引用, 然后调用该对象的height()方法. 上面的代码在调用了方法后, 新的对象引用并没有变量存储它, 所以最后会被JVM回收. 变量超出范围时, 通常会删除变量包含的引用. JVM会回收没有被引用的对象, 你可以用null使一些对象不被引用. 

## 关于类
### 协变返回类型covariant return type
简单来说, 协变返回值类型就是在重写父类方法时, 正常情况下返回值是不允许你改变的, 而JavaSE1.5开始支持的协变返回类型技术允许你在方法重载时, 改变方法返回值, 但前提是改变的返回值必须是被重载方法返回值的子类. 你也可以使用接口名作为返回值, 但必须实现该接口. 

修饰符决定各成员变量访问级别
<table border="1" summary="This table defines levels of access conferred by a modifier">
<caption style="font-weight: bold" id="accesscontrol-levels">Access Levels</caption>
<tbody><tr>
    <th>Modifier</th>
    <th>Class</th>
    <th>Package</th>
    <th>Subclass</th>
    <th>World</th>
</tr>
<tr>
    <td>public</td>
    <td>Y</td>
    <td>Y</td>
    <td>Y</td>
    <td>Y</td>
</tr>
<tr>
    <td>protected</td>
    <td>Y</td>
    <td>Y</td>
    <td>Y</td>
    <td>N</td>
</tr>
<tr>
    <td style="font-style: italic">no modifier</td>
    <td>Y</td>
    <td>Y</td>
    <td>N</td>
    <td>N</td>
</tr>
<tr>
    <td>private</td>
    <td>Y</td>
    <td>N</td>
    <td>N</td>
    <td>N</td>
</tr>
</tbody></table>
为了避免别人引错包, 访问级别的使用建议: 

- 对成员变量使用严格而有意义的访问级别, 应该使用private除非你有充分合理的缘由. 
- 除了常量外, 应避免使用public. 

### static修饰符
在使用static修饰符修饰的变量和方法时, 建议使用类名来调用而不是对象名, 容易引起歧义. 静态方法经常用来访问静态成员变量. 

### 常量Constants
使用final和static组合通常定义为常量, 通常以大写和用下划线分隔多个单词来命名. 

### 编译时常量compile-time constant
如果常量是一个基本类型或者String, 那么编译时编译器会直接把常量替换为该值. 

### 初始化块Initializing Fields
我们可以给成员变量赋值的方法来初始化成员变量, 但是这种办法不能给复杂的数组赋值和异常处理, 所以初始化的工作最好在构造器里做, 这样一些复杂的初始化和错误处理就能解决了. 初始化块里的代码相当于在执行构造函数前执行的代码, 设计的本意好像是为了解决构造器代码重复的问题. 在new对象时该代码块执行一次. 它长这样: 
~~~
class Example{
 {
    // 对, 在类里就两个括号, 啥也没有
 }
}
~~~

### 静态初始化块Static Initialization Blocks
为了提供和初始化块(Initializing Fields)一样的功能, 静态初始化块就是给静态变量初始化的, 一般只执行一次, 在new对象时静态代码块都优先执行: 
~~~
class Example{
static {
    // 静态代码块
 }
}
~~~
你可以写一个给静态变量赋值的private方法: 
~~~
class Whatever {
    public static varType myVar = initializeClassVariable();
        
    private static varType initializeClassVariable() {

        // initialization code goes here
    }
}
~~~
这样做的好处是你之后可以在以后重用该初始化方法. 方法被定义为final是因为在初始化时调用非final方法可能会导致问题. 

## 嵌套类Nested Classes
嵌套类分为: 静态内部类(static nested classes), 内部类(inner classes)
~~~
class OuterClass {
    ...
    static class StaticNestedClass {
        ...
    }
    class InnerClass {
        ...
    }
}
~~~
内部类可以访问OuterClass里的其他成员变量, 尽管被声明称private. 但静态内部类不行. 内部类可以被private, public, protected修饰, 但是最外部的类不能被private修饰. 
### 为什么使用嵌套类
- 对一些只对一个类有用的类做逻辑分组, 精简项目代码
    上面的代码中, InnerClass里可以访问OuterClass里的private成员. 
- 封装性, 隐藏不让别人知道的操作
- 使代码更可读和易于维护
### 静态内部类Static Nested Classes
声明在类里面的静态类. 
静态内部类StaticNestedClass不能直接访问和使用外部类OuterClass的变量和方法. 
静态内部类通过以下方式访问和创建: 
~~~
OuterClass.StaticNestedClass

OuterClass.StaticNestedClass nestedObject = new OuterClass.StaticNestedClass();
~~~
### 内部类Inner Classes
声明在类里面的类. 
内部类可以直接访问该类外部类的变量和方法, 但自己不能创建static成员. 
内部类的实例只能存在于外部类的实例中, 并且可以直接访问封闭实例的方法和字段. 
实例化内部类的前提是外部类已经被实例化, 应该这样实例化内部类: 
~~~
OuterClass.InnerClass innerObject = outerObject.new InnerClass();
~~~
还有两种特殊的内部类: 本地类(local classes)和匿名类(anonymous classes). 
### 变量重名Shadowing
~~~
public class ShadowTest {

    public int x = 0;

    class FirstLevel {

        public int x = 1;

        void methodInFirstLevel(int x) {
            System.out.println("x = " + x); // x = 23
            System.out.println("this.x = " + this.x); // this.x = 1 使用this代表该封闭范围
            System.out.println("ShadowTest.this.x = " + ShadowTest.this.x); // ShadowTest.this.x = 0
        }
    }

    public static void main(String... args) {
        ShadowTest st = new ShadowTest();
        ShadowTest.FirstLevel fl = st.new FirstLevel();
        fl.methodInFirstLevel(23);
    }
}
~~~
在上面的例子中有三个变量重名了, FirstLevel的methodInFirstLevel方法的参数, 和内部类的成员变量, 和内部类的外部类成员变量. 看完例子就知道应该怎么区分三个变量了. 
### 序列化
序列化内部类是不建议的, 它可能产生一些问题. 
### 内部类例子
~~~
public class DataStructure {
    
    // Create an array
    private final static int SIZE = 15;
    private int[] arrayOfInts = new int[SIZE];
    
    public DataStructure() {
        // fill the array with ascending integer values
        for (int i = 0; i < SIZE; i++) {
            arrayOfInts[i] = i;
        }
    }
    
    public void printEven() {
        
        // Print out values of even indices of the array
        DataStructureIterator iterator = this.new EvenIterator();
        while (iterator.hasNext()) {
            System.out.print(iterator.next() + " ");
        }
        System.out.println();
    }
    
    interface DataStructureIterator extends java.util.Iterator<Integer> { } 

    // Inner class implements the DataStructureIterator interface,
    // which extends the Iterator<Integer> interface
    
    private class EvenIterator implements DataStructureIterator {
        
        // Start stepping through the array from the beginning
        private int nextIndex = 0;
        
        public boolean hasNext() {
            
            // Check if the current element is the last in the array
            return (nextIndex <= SIZE - 1);
        }        
        
        public Integer next() {
            
            // Record a value of an even index of the array
            Integer retValue = Integer.valueOf(arrayOfInts[nextIndex]);
            
            // Get the next even element
            nextIndex += 2;
            return retValue;
        }
    }
    
    public static void main(String s[]) {
        
        // Fill the array with integer values and print out only
        // values of even indices
        DataStructure ds = new DataStructure();
        ds.printEven();
    }
}
~~~
输出内容是: 
~~~
0 2 4 6 8 10 12 14 
~~~
上面的例子中EvenIterator类直接引用DataStructure对象的arrayOfInts实例变量. 
你可以像例子一样使用内部类去实现一些帮助类. 一般用内部类处理用户界面的事务处理. 
### 声明本地类Declaring Local Classes
声明在方法里的类. 这里有一个验证手机号码的例子: 
~~~
public class LocalClassExample {
  
    static String regularExpression = "[^0-9]";
  
    public static void validatePhoneNumber(
        String phoneNumber1, String phoneNumber2) {
      
        final int numberLength = 10;
        
        // Valid in JDK 8 and later:
       
        // int numberLength = 10;
       
        class PhoneNumber {
            
            String formattedPhoneNumber = null;

            PhoneNumber(String phoneNumber){
                // numberLength = 7;
                String currentNumber = phoneNumber.replaceAll(
                  regularExpression, "");
                if (currentNumber.length() == numberLength)
                    formattedPhoneNumber = currentNumber;
                else
                    formattedPhoneNumber = null;
            }

            public String getNumber() {
                return formattedPhoneNumber;
            }
            
            // Valid in JDK 8 and later:

//            public void printOriginalNumbers() {
//                System.out.println("Original numbers are " + phoneNumber1 +
//                    " and " + phoneNumber2);
//            }
        }

        PhoneNumber myNumber1 = new PhoneNumber(phoneNumber1);
        PhoneNumber myNumber2 = new PhoneNumber(phoneNumber2);
        
        // Valid in JDK 8 and later:

//        myNumber1.printOriginalNumbers();

        if (myNumber1.getNumber() == null) 
            System.out.println("First number is invalid");
        else
            System.out.println("First number is " + myNumber1.getNumber());
        if (myNumber2.getNumber() == null)
            System.out.println("Second number is invalid");
        else
            System.out.println("Second number is " + myNumber2.getNumber());

    }

    public static void main(String... args) {
        validatePhoneNumber("123-456-7890", "456-7890");
    }
}
~~~
本地类可以访问该类外类的成员变量, 比如例子中本地类构造方法使用了LocalClassExample.regularExpression.
本地类仅能访问定义为final的外部类变量. 如果本地类访问一个本地变量和代码块的参数, 它能捕获这个本地变量和参数, 例如在例子中的本地类可以捕获numberLength. 在Java SE8之后, 本地类能访问定义为final或者是定义后没有改变的(effectively final)外部类变量和参数, 例如上面例子中, 把final去掉程序依然可以运作, 但把修改numberLength的语句取消注释, 编译器就会提醒你该变量不数据定义后没有改变(effectively final). 在本地类中你还可以访问phoneNumber1和phoneNumber2的值. 本地类和内部类相似, 他们都不能定义静态成员. 在静态方法里面的本地类只能引用外部同是静态成员. 例如例子里的regularExpression是静态的. 但它可以声明常量(constant variables). 
### 匿名类Anonymous Classes
匿名类例子: 
~~~
public class HelloWorldAnonymousClasses {
    
    interface HelloWorld {
        public void greet();
        public void greetSomeone(String someone);
    }
  
    public void sayHello() {
        HelloWorld frenchGreeting = new HelloWorld() {
            String name = "tout le monde";
            public void greet() {
                greetSomeone("tout le monde");
            }
            public void greetSomeone(String someone) {
                name = someone;
                System.out.println("Salut " + name);
            }
        };
        frenchGreeting.greet();
        frenchGreeting.greetSomeone("Fred");
    }

    public static void main(String... args) {
        HelloWorldAnonymousClasses myApp =
            new HelloWorldAnonymousClasses();
        myApp.sayHello();
    }            
}
~~~
一个匿名类声明包括: 
- 一个new操作符
- 实现的接口名或者继承的类名
- 括号中包含构造函数的参数, 但在上面的例子中由于是实现接口所以没有构造函数参数. 
- 一个主体(body大括号), 在主体中允许方法声明但不允许使用语句. 

和本地类一样, 匿名类也能捕获在它外部类括号内的成员, 但该成员必须也是final或者是定义后未被改变的变量. 在匿名类中的变量名如果重名就参照之前写的变量重名. 
匿名类里的成员和本地类一样有声明约束: 
- 你不能定义静态初始化器(static initializers)或成员接口(member interfaces)
- 静态类可以使用常量. 

你可以定义以下内容在匿名类中: 
- 字段(Fields)
- 额外的方法(Extra methods)
- 实例初始化器(Instance initializers不是构造器(constructors), 就是那个只有括号的代码块)
- 本地类(Local classes)

## Lambda表达式(Lambda Expressions)
当接口只有一个方法时, Lambda表达式可以简化代码, 它可以把函数当作一个方法参数, 或者把代码当作数据. 使得表达单方法的实例类代码更紧凑. 用的不多, [点这里查看官方指南.](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)

参考: [Classes and Objects](https://docs.oracle.com/javase/tutorial/java/javaOO/index.html)




























