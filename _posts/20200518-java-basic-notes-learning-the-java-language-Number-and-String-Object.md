---
title: Java基础笔记--学习Java语言--Number和String对象
date: 2020-05-18
updated: 2020-05-18
tags:
  - Java
urlname: java-basic-notes-learning-the-java-language-Number-and-String-Object
original: false
---
2.6 介绍Number和String对象, 以及格式化输出
<!--more-->

# Number相关

讨论java.lang.Number类以及相关子类, 以及什么情况下你用这个类而不是基础数据类型. 
这个章节也提供PrintStream和DecimalFormat类的说明, 它们提供数字类型的格式化输出方法. 
最后讨论java.lang.Math类, 它包含数学函数补充了语言内置的运算符. 如三角函数, 指数函数等. 

## 数字类The Numbers Classes

使用数字时很多时候我们都在代码中使用基础类型. 比如: 
~~~
int i = 500;
float gpa = 3.65f;
byte mask = 0xff;
~~~
然而有时候有使用对象代替数字的原因, 而Java平台给每个基础类型提供包装类型. 这些类把基础类型包装成对象. 一般包装的过程是由编译器完成的(如果你使用基础类型但是却希望得到一个对象, 编译器就会封装一个基础类型到包装类中. 同样的, 你希望从一个对象得到基础类型, 编译器就会拆箱. ). 了解更多看[自动装箱和拆箱Autoboxing and Unboxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html)
所有数字的包装类都是抽象类Number的子类: 
![Oracle官方提供的图片](/picture/20200519-0.jpg)
注: 有4个其他Number的子类没有在这里讨论. BigDecimal和BigInteger是用来进行高精度计算的. AtomicInteger和AtomicLong是用在多线程应用的. 

这里有三个你可能使用Number对象而不是基础类型的理由: 
- 1. 作为一个需要对象方法参数的时候(当使用和操作集合的时候经常用到). 
- 2. 需要利用到类中定义的常量, 比如MIN_VALUE最小值和MAX_VALUE最大值, 它提供一个数据类型的上下限. 
- 3. 使用类方法在其他原始类型之间进行值转换, 在字符串之间进行转换以及在数字系统之间进行转换(小数, 八进制, 十六进制, 二进制). 

下面的表格列出了Number类所有子类实例实现的实例方法. 
<table><tr>
<th>方法</th>
<th>描述</th>
</tr>
<tr>
<td>
byte byteValue()<br>
short shortValue()<br>
int intValue()<br>
long longValue()<br>
float floatValue()<br>
double doubleValue()
</td>
<td>将Number对象的值转换成基础类型</td>
</tr>
<tr>
<td>int compareTo(Byte anotherByte)<br>
int compareTo(Double anotherDouble)<br>
int compareTo(Float anotherFloat)<br>
int compareTo(Integer anotherInteger)<br>
int compareTo(Long anotherLong)<br>
int compareTo(Short anotherShort)</td>
<td>将Number对象和传入的参数做比较</td>
</tr>
<tr>
<td>boolean equals(Object obj)</td>
<td>确定Number对象是否等于参数. <br>
如果参数非空, 类型和值相同那么将返回true. <br>
但对于Double类和Float类有格外的要求, API文档中有说明. </td>
</tr>
</table>
每个Number类都包含了在字符串和数字之间和数字和数字系统之间转换的方法. 下面的表列出Integer类中的方法. 其他Number类的子类是类似的: 
<table><tr>
<th>方法</th>
<th>描述</th>
</tr>
<tr>
<td>static Integer decode(String s)</td>
<td>解码String作为一个整型对象. 可以接受一些十进制, 八进制或者十六进制作为输入. </td>
</tr>
<tr>
<td>static int parseInt(String s)</td>
<td>返回一个整数对象(仅十进制). </td>
</tr>
<tr>
<td>static int parseInt(String s, int radix)</td>
<td>根据给定的String和进制解析并返回int值. </td>
</tr>
<tr>
<td>String toString()</td>
<td>返回Integer对象的String对象. </td>
</tr>
<tr>
<td>static String toString(int i)</td>
<td>返回代表指定int的String对象(10进制). </td>
</tr>
<tr>
<td>static Integer valueOf(int i)</td>
<td>下面的懒得翻译了. Returns an Integer object holding the value of the specified primitive.</td>
</tr>
<tr>
<td>static Integer valueOf(String s)</td>
<td>Returns an Integer object holding the value of the specified string representation.</td>
</tr>
<tr>
<td>static Integer valueOf(String s, int radix)</td>
<td>Returns an Integer object holding the integer value of the specified string representation, parsed with the value of radix. For example, if s = "333" and radix = 8, the method returns the base-ten integer equivalent of the octal number 333.</td>
</tr>
</table>

## 格式化数字打印输出Formatting Numeric Print Output

在之前你看到了使用print和println方法来输出字符串到标准输出(System.out)中. 因为所有数字都可以转换成字符串, 因此你可以使用这些方法来混合输出字符串和数字. Java语言提供了其他输出的方法, 它们使得数字的输出更易于控制(格式化输出数字之类). 

在java.io包中包含一个PrintStream类, 该类有两个方法可以用来替换平常使用的print和println方法. 那就是format和printf方法, 它们彼此等效. 你熟悉使用的System.out恰好是一个PrintStream对象, 所以你可以通过System.out来使用PrintStream的方法. 所以你在使用print或者println的时候也可以使用format和printf方法. 
相关使用方法就不在这里列出了. 格式化输出, 小数等. 

## 基本算法之外Beyond Basic Arithmetic

Java提供基本的加减乘除还有取余算法, java.lang.Math类中提供了高级的数学算式方法和常数. 
Math类里的方法都是静态的, 所以可以直接调用它们. 
(注: 你可以使用静态导入(static import)的语言特性来导入Math类, 使得你不必再在使用该类的静态成员时加类名. )
例如: 
~~~
import static java.lang.Math.*;
~~~

### 常量和基本方法

这样在代码中就能直接使用E(自然对数)或者PI(圆周率)来使用Math类中的两个常数了. 
Math类包括了超过40个静态方法. 
<table><tr>
<th>方法名</th>
<th>描述</th>
</tr><tr>
<td>double abs(double d)<br>
float abs(float f)<br>
int abs(int i)<br>
long abs(long lng)</td>
<td>返回绝对值. </td>
</tr>
<tr>
<td>double ceil(double d)</td>
<td>返回一个大于或等于参数的最小浮点整数.(比如100.675返回101.0) </td>
</tr>
<tr>
<td>double floor(double d)</td>
<td>返回小于或等于参数的最大浮点整数.(比如100.675返回100.0) </td>
</tr>
<tr>
<td>double rint(double d)</td>
<td>返回接近参数的整数. 比如(100.5返回100.0, 100.6返回101.0)</td>
</tr>
<tr>
<td>long round(double d)<br>
int round(float f)</td>
<td>返回一个最接近的值(四舍五入). </td>
</tr>
<tr>
<td>double min(double arg1, double arg2)<br>
float min(float arg1, float arg2)<br>
int min(int arg1, int arg2)<br>
long min(long arg1, long arg2)</td>
<td>返回两个参数中最小的数. </td>
</tr>
<tr>
<td>double max(double arg1, double arg2)<br>
float max(float arg1, float arg2)<br>
int max(int arg1, int arg2)<br>
long max(long arg1, long arg2)</td>
<td>返回两个参数中最大的数. </td>
</tr>
</table>

### 指数和对数方法

<table><tr>
<th>Method</th>
<th width="50%">Description</th>
</tr>
<tr>
<td>double exp(double d)</td>
<td>返回底数为E(2.7183...)的参数次方. (也就是: e^d)</td>
</tr>
<tr>
<td>double log(double d)</td>
<td>返回参数的自然数底数的对数值. (也就是e的几次方等于d)</td>
</tr>
<tr>
<td>double pow(double base, double exponent)</td>
<td>返回base的exponent次方. </td>
</tr>
<tr>
<td>double sqrt(double d)</td>
<td>返回一个数的平方根. </td>
</tr>
</table>

### 三角函数方法

不常用不写了. 

### 随机数字

random()方法返回一个0.0到1.0(包括0.0不包括1.0)的伪随机数. 当你需要一串随机数时可以使用java.util.Random里的方法. 

# Character相关

## Characters
很多时候如果你使用一个字符, 你会使用char基础类型, 比如: 
~~~
// 字母a
char ch = 'a';
// 使用Unicode表示Ω字符(欧米伽字符哈哈哈omega)
char uniChar = '\u03A9';
// 字符数组
char[] charArray = { 'a', 'b', 'c', 'd', 'e' };
~~~
java会自动对char与Character类进行自动装箱和拆箱. 
注: Character类是一成不变的, 所以Character对象一旦被创建就无法改变)

### 转义序列Escape Sequences

以反斜杠\开头的是转义序列, 它对编译器有特殊含义. java中的转义序列: 
<table><tr>
<th width="30%">Escape Sequence</th>
<th>Description</th>
</tr>
<tr>
<td>\t</td>
<td>插入tab</td>
</tr>
<tr>
<td>\b</td>
<td>插入退格键backspace </td>
</tr>
<tr>
<td>\n</td>
<td>插入新的一行. </td>
</tr>
<tr>
<td>\r</td>
<td>回车回车??</td>
</tr>
<tr>
<td>\f</td>
<td>换页? 没查到是啥意思formfeed </td>
</tr>
<tr>
<td>\'</td>
<td>单引号字符. </td>
</tr>
<tr>
<td>\"</td>
<td>双引号字符. </td>
</tr>
<tr>
<td>\\</td>
<td>反斜杠字符. </td>
</tr>
</table>
当在打印语句遇到转义字符时, 编译器会相应的解释它. 

# String相关

String是在Java程序中用的最广泛的的东西, 它是由一串的字符构成. 

## 创建String字符串

你可以这么写来创建一个String对象: 
~~~
String greeting = "Hello world!";
~~~
在上面的例子中被引号包裹的Hello world!就是字面量string literal, 它由一系列字符串组成. 每当编译器遇到字面量的时候就会创建一个String对象(String有字符串池的概念, 有时候不一定每次都创建新的对象), 在这个例子中是Hello world!. 
对于其他对象, 你可以使用new关键词和String的构造函数来创造来自不同源的String对象(这样new出来的对象就真的是new出来的单独的对象, 即使它和字符串池中的字符串重复). 
(注: String类是一成不变的, 这意味着创建一个String对象就不能被改变. String类有很多方法, 某些方法会在下面讨论, 它们看起来改变了String. 因为String对象的一层不变的, 那么改变了字符串的那些方法真正做的是返回一个新的, 通过操作制造的String对象)

## String的长度

获得一个对象信息的方法一般称作访问器方法accessor methods, String对象的一个访问器方法是length(), 它可以返回String对象包含的字符数量. 
~~~
String palindrome = "Dot saw I was Tod";
int len = palindrome.length();  // len为17
~~~

## 连接字符串

String类包含一个concat()方法来连接两个字符串: 
~~~
string1.concat(string2); 
~~~
它将返回一个新的String对象, 由string1+string2组成. 
String最常用的是用+操作符来连接. 
~~~
"Hello," + " world" + "!"
~~~
这个操作符可以混合连接任何String对象, 对于那些不是String的对象它会调用toString()方法来转换成对象. 
(注: Java中不支持源代码中一个字符串占用两行或以上, 比如
~~~
String str = "abcd 
         efgh";
~~~
所以对于长的String你需要在末尾添加+来连接. 例如: 
~~~
String quote = 
    "Now is the time for all good " +
    "men to come to the aid of their country.";
~~~
)

## 创建格式化字符串

上面提到过printf()和format()方法, 而String类中也有等量的格式化输出的format()方法, 它返回一个String对象而不是PrintStream对象. 
使用format()方法允许你创建一个格式化的String对象以至于你可以重用该对象. (虽然看起来没什么用)为了替代: 
~~~
System.out.printf("The value of the float " +
                  "variable is %f, while " +
                  "the value of the " + 
                  "integer variable is %d, " +
                  "and the string is %s", 
                  floatVar, intVar, stringVar);
~~~
你可以这么写: 
~~~
String fs;
fs = String.format("The value of the float " +
                   "variable is %f, while " +
                   "the value of the " + 
                   "integer variable is %d, " +
                   " and the string is %s",
                   floatVar, intVar, stringVar);
System.out.println(fs);
~~~

## Number和String之间相互转换

### String转换Number

一般情况下用户输入的都是String类型的数字数据. 包装基础类型的Number的子类提供了valueOf方法来将Stirng转换为相应的包装对象. 
官方举出来一个[例子](https://docs.oracle.com/javase/tutorial/java/data/converting.html). 
注: 每个包装基础数字型的Number类的子类都提供一个parseXXXX()的方法来将String转换成基础类型, 因为和valueOf()方法对比, parseXXXX()方法返回的是基础类型而不是对象, 所以比较直接易用. 

### Number转换String

下面是几种number转换成String的方法: 
~~~
int i;
// Concatenate "i" with an empty string; conversion is handled for you.
String s1 = "" + i;
~~~
或: 
~~~
// The valueOf class method.
String s2 = String.valueOf(i);
~~~
每个Number子类包含一个toString()方法, 它会转换基础类型到String. 
~~~
int i;
double d;
String s3 = Integer.toString(i); 
String s4 = Double.toString(d); 
~~~

## 操纵String中的字符 Manipulating Characters in a String

String类中有通过字符串查找字符和子串, 改变字符串内容等方法. 

### 通过索引获取字符和字串

你可以通过charAt()方法来访问特定索引的String字符. index从0开始到字符串的长度减一. 下面的代码将得到索引为9的字符: 
~~~
String anotherPalindrome = "Niagara. O roar again!"; 
char aChar = anotherPalindrome.charAt(9); // 输出O
~~~
![索引为9的字符](/picture/20200624-0.jpg)
如果你想获得超过一个字符, 你可以使用substring方法. 
然后官方文章介绍了很多操作String的方法, 我在这里就懒得写啦. 

## 比较String和一部分的Stirng Comparing Strings and Portions of Strings

官方列出了一些String的比较方法, 这里就不多说了. 

## StringBuilder 类

StringBuilder对象和String对象很类似, 但是它是可以修改的. 在对象内部, 它把一系列字符当成可变数组来对待. 在任何情况下, 它的长度和内容都可以通过调用方法来改变. 

一般情况下使用String也就足够了, 除非你需要做一些大量的追加String操作, 这类操作使用StringBuilder能提高效率. 

StringBuilder默认分配容量为16. 官方给出了StringBuilder的一些介绍和用法. 


# 自动装箱和拆箱

自动装箱是Java编译器对基础类型和对应的包装类的自动转换机制. 例如将int转换成Integer, double转换成Double等. 如果反过来就叫拆箱. 

下面是自动装箱的例子: 
~~~
Character ch = 'a';
~~~
下面的例子中使用到了泛型: 
~~~
List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i += 2)
    li.add(i);
~~~
在上面的代码中尽管你添加的是int基础类型, 而不是Integer到li里面, 代码还编译通过了. 因为li是一列Integer对象, 不是一列int, 所以你可能会疑惑为什么Java编译器没有抛出编译时错误. 它没有抛出错误的原因是因为它为i创建了一个Integer对象然后加到li里面去. 因此在运行时编译器转换上面的代码为: 
~~~
List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i += 2)
    li.add(Integer.valueOf(i));
~~~
把基础类型转换成对应的对象就叫自动装箱. 当基础类型应用在下面两种场景时会自动装箱: 
- 作为参数传递到方法中. 
- 分配值给对应的包装类

考虑下面的代码: 
~~~
public static int sumEven(List<Integer> li) {
    int sum = 0;
    for (Integer i: li)
        if (i % 2 == 0)
            sum += i;
        return sum;
}
~~~
因为取余%和一元加法+=操作不支持Integer对象, 你可能会疑惑为什么Java编译器编译了这段代码而没有产生错误. 编译器没有产生错误是因为它调用了intValue()方法在运行时将Integer转换成了int: 
~~~
public static int sumEven(List<Integer> li) {
    int sum = 0;
    for (Integer i : li)
        if (i.intValue() % 2 == 0)
            sum += i.intValue();
        return sum;
}
~~~
将包装类对象转换成对应的基础类型叫做拆箱. Java编译器在下面的情况下使用拆箱: 
- 作为方法参数传递时
- 分配给对应的基础类型


参考: [Numbers and Strings](https://docs.oracle.com/javase/tutorial/java/data/index.html)