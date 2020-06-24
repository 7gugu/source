---
title: Java基础笔记--学习Java语言--语法基础
date: 2019-11-27 
updated: 2020-04-30
tags:
  - Java
urlname: java-basic-notes-learning-the-java-language-language-basics
original: false
---
2.2变量, 命名规则, 基础数据类型, 字面量, 数组, 运算符, 语句. 
<!--more-->
# 变量Variables
## 变量类型:
Java程序语言定义了下面几种变量:
### 1.实例变量Instance Variables(非静态字段Non-Static Fields)
也就是对象存储它们个体变量的字段, 它们对每个对象来说是不一定相同的, 比如每辆自行车的当前速度是不一样的. 
### 2.类变量Class Variables(静态字段Static Fields)
标记了静态static的字段是所有实例化该类对象共享的, 比如说自行车的齿轮变量标记了static, 那么通过这个类实例化出来的自行车就应该是有相同的齿轮数量, 如果再加上final关键字, 那么该字段将无法被修改. 
### 3.局部变量Local Variables
也就是方法里的变量, 基本不需要修饰, 只能在该方法里使用, 其他类无法访问的变量. 
### 4.参数Parameters 
在之前的Hello World程序的main方法里的args就是一个String数组类型的参数. 最重要的一点是, 参数(Parameters)是属于变量(variables)而不是字段(fields).

在接下来的教程里, 字段fields通常不包括局部变量Local Variables和参数Parameters, 如果谈到"上述所有"则代表变量. 成员member则代表类型的字段fields, 方法methods和嵌套类型nested types的集合.

## 命名规则:
不建议使用美元符号$和下划线_开头, 尽量使用有意义的命名, 避免使用[Java的关键词和保留字](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/_keywords.html),
如果名字为单个单词则用小写, 多个单词用驼峰命名法, 如果声明为常量则每个字母大写并用
下划线隔开. 

## 原始数据类型:
Java语言支持8种原始数据类型:
### 1.byte
8位带符号的二进制补码整数, 范围[-128到127], 可以用在大数组中节约内存, 可以替换int. 
### 2.short
16位带符号的二进制补码整数, 范围[-32,768到32,767], 像byte一样可以用来节约内存. 
### 3.int
32位带符号的二进制补码整数, 范围[-2^31到(-2^31)-1], 在Java SE8以后你也可以用int
来代表32位无符号整数,  具体查看Number类. 一些无符号运算方法已经添加到Integer类里了. 
### 4.long
64位带符号的二进制补码整数, 范围[-2^63到(-2^63)-1], 和int差不多. 
### 5.float
单精度32位浮点型, 在[Floating-Point Types, Formats, and Values](https://docs.oracle.com/javase/specs/jls/se7/html/jls-4.html#jls-4.2.3)
中指定了该类型, 如果学过计算机组成原理的话你会很容易理解浮点型的存储方式. 对double
来说float可以用来节约内存, 但永远不要用float和double来用于精确值, 比如货币. 如果
需要你可以使用java.math.BigDecimal类来代替, 在[Numbers and Strings章节](https://docs.oracle.com/javase/tutorial/java/data/index.html)有具体的介绍. 
### 6.double
双精度64位浮点型, 默认的小数类型, 和float一样不能用于精确值, 比如货币. 
### 7.boolean
它只有true或者false值, 虽然能用一位就能表示它, 但它的大小不是精确定义的. 
### 8.char
char数据类型是单个16位Unicode字符, 它的范围'/u0000'或者说是(0), 最大值'/uffff'
或者说是(65,535).

Java对字符串String类型通过java.lang.String提供特殊支持, String不是Java的原始数据类型, Java会默认以用两个引号包括起来的字符串来创建一个Stirng对象, 但这种对象是一成不变的. 详情可以参照[Numbers and Strings章节](https://docs.oracle.com/javase/tutorial/java/data/index.html). 

## 默认值:
声明但未被初始化的字段Fields会被编译器compiler设置一个合理的数值, 通常是0或者null, 通常认为依赖于默认值是一种不好的编程风格. 
但是局部变量编译器compiler并不会对它设置合理数值, 访问未初始化的局部变量会导致编译时错误. 

## 字面量Literals:
原始类型是特殊的类型, 因此你会发现不需要new一个对象来给它赋值, 一个字面量用源代码直接表示为一个固定值. 像true, false, 'A', 100等. 

### 整型字面量Integer Literals
一个以L或者l结尾的整型字面量类型为long, 通常用L而不是l因为l和数字1很像. byte, short, int和long都可以用int字面量来创建, 超出int部分的long可以用L结尾形式创建, 整型字面量还可以表达二进制, 十六进制, 十进制等. 
~~~
// 用十进制表示数字26
int decVal = 26;
//  用十六进制表示数字26
int hexVal = 0x1a;
// 用二进制表示数字26
int binVal = 0b11010;
~~~

### 浮点型字面量Floating-Point Literals
一个以F或者f结尾的浮点型字面量类型为float, 否则就是double, double的结尾D或者d是可选的. 
一个浮点型类型可以使用E或者e结尾, 它表示科学计数法. 
~~~
double d1 = 123.4;
// 用科学计数法表示和d1一样的值
double d2 = 1.234e2;
float f1  = 123.4f;
~~~

### 字符和字符串字面量Character and String Literals
Java支持一些特殊字符的转译, Java中的null一般用来测试值是否存在和对象是否可以使用. 

最后还有一类叫类字面量的特殊字面量类型, 一般在类后面加上.class就是了, 例如String.class就代表这个对象它自己的类型. 

### 在数字字面量中使用下划线字符
当一个二进制或者十六进制非常长的时候, 可以使用下划线让它们隔开以增加可读性, 比如
官方给出的例子: 
~~~
long creditCardNumber = 1234_5678_9012_3456L;
long socialSecurityNumber = 999_99_9999L;
float pi =  3.14_15F;
long hexBytes = 0xFF_EC_DE_5E;
long hexWords = 0xCAFE_BABE;
long maxLong = 0x7fff_ffff_ffff_ffffL;
byte nybbles = 0b0010_0101;
long bytes = 0b11010010_01101001_10010100_10010010;
~~~
规则是你只能把下划线放在数字之间. [参阅Using Underscore Characters in Numeric Literals](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)

## 数组
数组是一个保存固定长度值的单个类型的容器对象, 数组里的每一项叫做元素element, 元素可以通过数组下标访问numerical index, 下标通常是0开始的. 

### 定义一个数组变量
和其他变量类型一样, 定义一个数组变量也要两部分, 数组类型和数组名字. 数组类型写成类型加两个方括号type[]; 数组名字就是变量名字. 创建了一个数组变量并不意味着创建了数组, 这只是告诉编译器这个变量将保存指定类型的数组. 建议的命名方式: 
~~~
byte[] anArrayOfBytes;
short[] anArrayOfShorts;
long[] anArrayOfLongs;
float[] anArrayOfFloats;
double[] anArrayOfDoubles;
boolean[] anArrayOfBooleans;
char[] anArrayOfChars;
String[] anArrayOfStrings;
~~~
括号也可以放在变量后面, 但是不鼓励这种形式. 括号作为数组类型的标识应该定义在类型里. 
~~~
float anArrayOfFloats[];
~~~
### 创建, 初始化, 和访问数组
一种方式去真正创建一个数组是使用new关键字, 如果没有这个声明, 编译器会抛出数组变量没有被初始化的错误. 
~~~
anArray = new int[10]; // 创建
anArray[0] = 100; // 初始化第一个元素
~~~
### 多维数组
Java中的多维数组和C语言Fortran语言不一样, 多维数组通过一维数组组装产生, 可以通过数组内置的length属性查看数组的大小. 
~~~
anArray.length // 一维数组长度
anArray[0].length // 二维数组第一个数组长度
~~~
### 复制数组
System类提供了一个arraycopy方法来高效率复制一个数组. 也可以使用java.util.Arrays类里的copyOfRange方法, 后者会返回一个数组. 

java.util.Arrays类里一些有用的方法:
1.搜索特定值获取索引(binarySearch方法)
2.比较两个数组是否相等(equals方法)
3.用特定值填充数组(fill方法)
4.排序(使用sort方法, 在多处理器排序大数组时parallelSort方法比顺序排序更有优势)

## 变量总结
Java使用字段fields和变量variables两种术语, 实例变量Instance variables是每个实例类都可以不一样的, 而类变量Class variables是每个实例的类共有的. 
8种数据类型是:byte, short, int, long, float, double, boolean, and char. 而java.lang.String类代表字符串. 编译器会自动分配合理的默认值给上面的类型. 编译器不会给本地变量赋值, 字面量literal是固定值的源代码表示. 数组是存储单一类型的固定长度容器. 数组在创建后大小是固定的. 

参考: [Variables](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html)

# 运算符
在联合使用运算符时需要注意运算符优先级问题. 
## 赋值, 运算, 一元运算符
### 赋值
除了简单的赋值, 还能用来赋值对象的引用. 
### 运算符
加, 减, 乘, 除, 取余
加还能用来连接字符串. 
### 一元运算符
正负标识符, 自增自减, 取反. 
区分前缀自增自减和后缀自增自减区别. 
## 等于, 关系和条件运算符
### 等于和关系运算符
等于, 不等于, 大于, 大于等于, 小于, 小于等于
### 条件运算符
与, 或
还有一个常用条件运算符是 condition ? value1 : value2 , 这个是一个三元运算符. 
### 类型比较运算符
instanceof 运算符将对象与指定类型进行比较, 你可以用它来测试对象是否是类的实例, 是否是子类的实例, 或者一个类的实例是否实现特定的接口. 注意null不是实例. 
## 按位和移位操作
补数运算符, 左移或者右移运算符第二个值为移动的位数, 无符号左移是左边补0, 否则补符号. 
按位与, 按位或, 异或

## 表达式, 语句和块 Expressions, Statements, and Blocks
### 表达式Expressions
一条表达式由变量, 运算符, 或者说带回返回值的方法组成. 
主要是避免歧义, 例子:
~~~
x + y / 100    // 含糊的
(x + y) / 100  // 不含糊
x + (y / 100) // 不含糊
~~~
### 语句Statements
有些表达式通过结尾加分号变成语句叫做表达语句expression statements, 
语句分为声明语句(declaration statements)和控制流语句(control flow statements).
声明语句:
~~~
// declaration statement
double aValue = 8933.234;
~~~
### 块Blocks
一个语句块是指由0或者多个语句组成的代码块.
~~~
class BlockDemo {
     public static void main(String[] args) {
          boolean condition = true;
          if (condition) { // begin block 1 
               System.out.println("Condition is true.");
          } // end block one
          else { // begin block 2
               System.out.println("Condition is false.");
          } // end block 2
     }
}
~~~
## 控制流语句Control flow statement
简单来说就是控制代码块的执行顺序, 使他们不仅仅是从上到下执行. 
if/switch/while/do-while/for

用switch写日数判断:
~~~
class SwitchDemo2 {
    public static void main(String[] args) {
        int month = 2;
        int year = 2000;
        int numDays = 0;
        switch (month) {
            case 1: case 3: case 5:
            case 7: case 8: case 10:
            case 12:
                numDays = 31;
                break;
            case 4: case 6:
            case 9: case 11:
                numDays = 30;
                break;
            case 2:
                if (((year % 4 == 0) && 
                     !(year % 100 == 0))
                     || (year % 400 == 0))
                    numDays = 29;
                else
                    numDays = 28;
                break;
            default:
                System.out.println("Invalid month.");
                break;
        }
        System.out.println("Number of Days = "
                           + numDays);
    }
}
~~~

for可以迭代集合(Collections)和数组(arrays)

没有标签的break会结束最内层的switch, for, while, or do-while语句. 给break加上标签可以中断指定的语句. continue同理. 
例如: 
~~~
class BreakWithLabelDemo {
    public static void main(String[] args) {

        int[][] arrayOfInts = { 
            { 32, 87, 3, 589 },
            { 12, 1076, 2000, 8 },
            { 622, 127, 77, 955 }
        };
        int searchfor = 12;

        int i;
        int j = 0;
        boolean foundIt = false;

    search:
        for (i = 0; i < arrayOfInts.length; i++) {
            for (j = 0; j < arrayOfInts[i].length;
                 j++) {
                if (arrayOfInts[i][j] == searchfor) {
                    foundIt = true;
                    break search;
                }
            }
        }

        if (foundIt) {
            System.out.println("Found " + searchfor + " at " + i + ", " + j);
        } else {
            System.out.println(searchfor + " not in the array");
        }
    }
}
~~~

来源: [Lesson: Language Basics](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/index.html)



























