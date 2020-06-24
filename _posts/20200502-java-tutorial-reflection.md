---
title: Java教程--反射
date: 2020-05-02
updated: 2020-05-02
tags:
  - Java
urlname: java-tutorial-reflection 
original: false
---
简单翻译过一下Java官方的反射讲解. 
<!--more-->

# 0.反射API

## 反射的使用

反射经常被程序用于检查或修改运行在JVM中的程序的行为. 这是一项相对先进的功能, 仅适用于对JAVA语言有较深把握的开发者们. 注意事项: 反射是一种强大的技术, 可以让程序执行原本不可能执行的操作. 

### 扩展功能

一个程序可以通过fully-qualified names利用到外部或者用户定义的类来创建一个可扩展性对象实例. (fully-qualified names在计算机编程里指的是不含糊的名称, 它唯一指定一个程序中的对象, 函数或者变量.[维基百科Fully qualified name](https://en.wikipedia.org/wiki/Fully_qualified_name))

### 类浏览器(Class Browsers)和可视化开发环境

一个类浏览器需要能够去枚举类中的成员. 可视化开发环境可以从反射中的类型信息受益, 从而去帮助开发者编写正确的代码. 

### 调试器和测试工具Debuggers and Test Tools

调试器需要能够去检查类中的私有成员. 测试工具可以利用反射系统地调用定义在类中的可发现的API集合, 来确保在测试套件中的高级的代码覆盖率. 

## 反射的缺点

反射很强大但是不应该随便的使用. 如果能够不使用反射来执行同样的功能, 那么最好避免使用反射. 使用反射时应该记住下面的内容. 

### 性能开销

因为反射涉及类型是动态解析的, 某些JVM优化可能不会执行. 因此, 反射的操作性能比没有反射的操作性能低, 和应该避免使用在对性能敏感的应用程序经常调用的代码段中. 

### 内部暴露

因为反射允许执行一些在正常代码中不合法的操作, 例如可以直接访问私有字段和方法, 使用反射可能会导致不可预估的副作用, 它可能会使代码功能失调和破坏可移植性. 反射打破了抽象, 因此可能随着平台的升级它的行为会改变. 

# 1.类Classes

该课程会展示获取一个类对象的多种方式和使用它来检查一个类的属性, 包括声明和内容. 
Java中所有类型不是引用就是基本类型. 类Classes, 枚举enums, 数组arrays(继承java.lang.Object的)以及接口interface都是引用类型reference types. 引用类型的例子包括java.lang.String, 所有基础类型的包装类比如java.lang.Double, 接口java.io.Serializable和枚举javax.swing.SortOrder. 下面是一组固定的原始类型: boolean, byte, short, int, long, char, float, 和double. 
对于所有类型的对象, JVM会实例化一个java.lang.Class类型的不变实例(immutable instance), 它能提供方法来检查运行中对象的属性, 包括方法和类型信息. 它也提供创建新的类和对象的能力. 最重要的是它是所有反射API的入口. 下面将展示类中最常用的一些反射操作: 
- 检索类对象Retrieving Class Objects, 描述得到一个类Class的方式
- 检查类的修饰符和类型, 展示如何访问修饰符信息. 
- 发现类成员, 展示如何列出类中的构造器, 字段, 方法和嵌套类. 
- 故障排除, 描述当使用Class类时遇到的常见错误. 

## 检索类对象Retrieving Class Objects

所有反射操作的入口是java.lang.Class. 除了java.lang.reflect.ReflectPermission类, java.lang.reflect里的所有类都没有public的构造函数. 我们有必要调用Class类中合适的方法来得到一个类. 下面有几种获取Class的方式, 它基于代码是否有权访问一个对象, 类名, 类型, 或者一个存在的类. 

### Object.getClass()

如果有对象实例可用, 最简单的方式就是直接调用Object.getClass()来获得一个Class. 当然这只适用于继承了Object的对象. 一些例子: 
~~~
Class c = "foo".getClass();
~~~
返回String的Class
~~~
Class c = System.console().getClass();
~~~
这是和虚拟机关联的独一无二的控制台, 它是通过static方法System.console()返回的. 通过getClass()返回的值是对应于java.io.Console的Class. (FIXME我在测试的时候报了空指针异常...)
~~~
enum E { A, B }
Class c = A.getClass();
~~~
A是枚举类型E的实例, 因此getClass()返回的Class对应于枚举类型E. 
~~~
byte[] bytes = new byte[1024];
Class c = bytes.getClass();
~~~
由于数组arrays是一个对象, 所以可以在一个实例中调用getClass(), 它的返回值对应类型byte. 
~~~
import java.util.HashSet;
import java.util.Set;

Set<String> s = new HashSet<String>();
Class c = s.getClass();
~~~
在这个例子中, java.util.Set是一个java.util.HashSet类的接口. 所以getClass()的返回值class是对应java.util.HashSet. 

### .class语法

如果类型可用但是没有实例方法, 那可以通过在类型名字后附加".class"来获取一个Class. 这也是获取原始类型Class最简单的方式. 
~~~
boolean b;
Class c = b.getClass();   // 编译时错误

Class c = boolean.class;  // 正确
~~~
注意上面的语句, boolean.getClass()会产生一个编译错误, 因为boolean是原始类型不能被取消引用dereferenced. (FIXME我也不是很清楚dereferenced的真正含义. ) .class语法返回对应boolean的Class. 
~~~
Class c = java.io.PrintStream.class;
~~~
变量c会是一个对应java.io.PrintStream类的类型. 
~~~
Class c = int[][][].class;
~~~
它会返回给定类型的多维数组Class.
 
### Class.forName()

如果一个类的fully-qualified name可用, 那么可以通过静态方法Class.forName()来获取对应的Class. 它不能用于基础类型. 为数组类名称的使用的方法是Class.getName(). 这个语法适用于引用类型和基础类型. 
~~~
Class c = Class.forName("com.duke.MyLocaleServiceProvider");
~~~
改语句会根据fully-qualified name创建一个类. 
~~~
Class cDoubleArray = Class.forName("[D");

Class cStringArray = Class.forName("[[Ljava.lang.String;");
~~~
变量cDoubleArray会获得一个对应基础类型double的Class. (比如和double[].class获取的一样). 而变量cStringArray获得一个对应二维String数组的Class. (比如和String[][].class相同). 

### 为基础类型包装的类型字段TYPE Field

.class语法是非常方便和推荐的获取一个基础类型Class的方式, 但他还有其他方法. 每个基础类型包括void在java.lang包下都有包装类, 它是用来把基础类型封装成引用类型的. 每个包装类包含一个TYPE的字段名, 它等于被包装原始类型的Class. 
~~~
Class c = Double.TYPE;
~~~
这是一个java.lang.Double类, 每当请求一个对象的时候会自动把double包装成它. 它的值等同于doublic.class. 
~~~
Class c = Void.TYPE;
~~~
Void.TYPE等同于void.class. 

### 返回类Class的方式

反射API中有很多返回类的方法, 但前提是它们已经直接或间接获得了Class. 
- Class.getSuperclass()
返回给定类的父类Class. 
~~~
Class c = javax.swing.JButton.class.getSuperclass();
~~~
javax.swing.JButton的父类是javax.swing.AbstractButton. 
- Class.getClasses()
返回该类所有public修饰的类, 接口和枚举成员, 包括继承的成员. 
~~~
Class<?>[] c = Character.class.getClasses();
~~~
Character包含两个成员Class, Character.Subset and Character.UnicodeBlock. (我测试的时候其实还有Character.UnicodeScript, 应该是后面版本新增的啦)
- Class.getDeclaredClasses()
返回类中所有显示声明的类接口和枚举. 
~~~
Class<?>[] c = Character.class.getDeclaredClasses();
~~~
跟上面的Class.getClasses()方法相比它还列出了私有成员Character.CharacterCache. 
- Class.getDeclaringClass()
- java.lang.reflect.Field.getDeclaringClass()
- java.lang.reflect.Method.getDeclaringClass()
- java.lang.reflect.Constructor.getDeclaringClass()
返回这些成员字段被定义的那个Class, 也就是定义在哪个Class里. 匿名类的声明不会有声明类declaring class但是会有封闭类enclosing class. 
~~~
import java.lang.reflect.Field;

Field f = System.class.getField("out");
Class c = f.getDeclaringClass();
~~~
成员字段out定义在System. 
~~~
public class MyClass {
    static Object o = new Object() {
        public void m() {} 
    };
    static Class<c> = o.getClass().getEnclosingClass();
}
~~~
匿名类的声明类declaring class由o定义的是null. 
- Class.getEnclosingClass()
返回该类的直接封闭类. (应该是谁包裹这个类的类的意思吧)
~~~
Thread.State.class.getEnclosingClass();
~~~
封闭类Thread.State是Thread. 也就是直接包裹Thread.State的是Thread类. 
在上面的MyClass类中, 用o定义的匿名类是封闭enclosed by在MyClass中的, 所以使用getEnclosingClass()方法得到封闭它的类MyClass. 

## 检查类Class修饰符和类型

一个类可能定义一个或多个修饰符, 它影响运行时的行为. 
- 访问修饰符: public, protected, private
- 要求覆盖修饰符: abstract
- 限制一个实例的修饰符: static
- 禁止值修改的修饰符: final
- 强制严格的浮点行为修饰符: strictfp(JDK 1.2版本中引入, 确保浮点数计算出的结果不会随着平台的不同而不同[ref: geeksforgeeks: strictfp keyword in java](https://www.geeksforgeeks.org/strictfp-keyword-java/))
- 注解

不是所有的修饰符在类中都是允许使用的, 比如接口不能被final修饰和枚举类型不能被abstract修饰. java.lang.reflect.Modifier包含了所有修饰符可能的声明. 它也包含给Class.getModifiers()返回的修饰符集合解码的方法.  
下面的ClassDeclarationSpy例子中展示了如何获得类的声明组件包括修饰符, 泛型参数, 实现的接口, 继承路径. 自从Class实现了java.lang.reflect.AnnotatedElement接口, 所以也可以查询运行时的注解. 
~~~
import java.lang.annotation.Annotation;
import java.lang.reflect.Modifier;
import java.lang.reflect.Type;
import java.lang.reflect.TypeVariable;
import java.util.Arrays;
import java.util.ArrayList;
import java.util.List;
import static java.lang.System.out;

public class ClassDeclarationSpy {
    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    out.format("Class:%n  %s%n%n", c.getCanonicalName());
	    out.format("Modifiers:%n  %s%n%n",
		       Modifier.toString(c.getModifiers()));

	    out.format("Type Parameters:%n");
	    TypeVariable[] tv = c.getTypeParameters();
	    if (tv.length != 0) {
		out.format("  ");
		for (TypeVariable t : tv)
		    out.format("%s ", t.getName());
		out.format("%n%n");
	    } else {
		out.format("  -- No Type Parameters --%n%n");
	    }

	    out.format("Implemented Interfaces:%n");
	    Type[] intfs = c.getGenericInterfaces();
	    if (intfs.length != 0) {
		for (Type intf : intfs)
		    out.format("  %s%n", intf.toString());
		out.format("%n");
	    } else {
		out.format("  -- No Implemented Interfaces --%n%n");
	    }

	    out.format("Inheritance Path:%n");
	    List<Class> l = new ArrayList<Class>();
	    printAncestor(c, l);
	    if (l.size() != 0) {
		for (Class<?> cl : l)
		    out.format("  %s%n", cl.getCanonicalName());
		out.format("%n");
	    } else {
		out.format("  -- No Super Classes --%n%n");
	    }

	    out.format("Annotations:%n");
	    Annotation[] ann = c.getAnnotations();
	    if (ann.length != 0) {
		for (Annotation a : ann)
		    out.format("  %s%n", a.toString());
		out.format("%n");
	    } else {
		out.format("  -- No Annotations --%n%n");
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }

    private static void printAncestor(Class<?> c, List<Class> l) {
	Class<?> ancestor = c.getSuperclass();
 	if (ancestor != null) {
	    l.add(ancestor);
	    printAncestor(ancestor, l);
 	}
    }
}
~~~
下面是输出的一些样本. 
输入: 
~~~
$ java ClassDeclarationSpy java.util.concurrent.ConcurrentNavigableMap
~~~
输出: 
~~~
Class:
  java.util.concurrent.ConcurrentNavigableMap

Modifiers:
  public abstract interface

Type Parameters:
  K V

Implemented Interfaces:
  java.util.concurrent.ConcurrentMap<K, V>
  java.util.NavigableMap<K, V>

Inheritance Path:
  -- No Super Classes --

Annotations:
  -- No Annotations --
~~~
这也确实是java.util.concurrent.ConcurrentNavigableMap在源代码中的实际声明: 
~~~
public interface ConcurrentNavigableMap<K,V>
    extends ConcurrentMap<K,V>, NavigableMap<K,V>
~~~
注意它是个接口, 所以修饰符里含有隐式的abstract. 编译器给所有接口都添加这个修饰符. 同样这个声明包含了两个泛型参数K和V. 例子中只是简单的输出了参数的名字, 我们还可以使用java.lang.reflect.TypeVariable中的一些方法获取更多的信息. 在上面的例子中接口可以实现其他接口. 
输入: 
~~~
$ java ClassDeclarationSpy "[Ljava.lang.String;"
~~~
输出: 
~~~
Class:
  java.lang.String[]

Modifiers:
  public abstract final

Type Parameters:
  -- No Type Parameters --

Implemented Interfaces:
  interface java.lang.Cloneable
  interface java.io.Serializable

Inheritance Path:
  java.lang.Object

Annotations:
  -- No Annotations --
~~~
因为数组是一个运行时对象runtime objects(理解成JVM虚拟机底层的内容就可以啦), 所有类型信息由JVM定义. 特别是, 数组实现了Cloneable和java.io.Serializable接口, 它的直接父类也是Object. 
输入: 
~~~
$ java ClassDeclarationSpy java.io.InterruptedIOException
~~~
输出: 
~~~
Class:
  java.io.InterruptedIOException

Modifiers:
  public

Type Parameters:
  -- No Type Parameters --

Implemented Interfaces:
  -- No Implemented Interfaces --

Inheritance Path:
  java.io.IOException
  java.lang.Exception
  java.lang.Throwable
  java.lang.Object

Annotations:
  -- No Annotations --
~~~
从继承路径可以看到java.io.InterruptedIOException是一个检查异常checked exception因为路径中没有RuntimeException. (除了Error及它子类和RuntimeException及它子类是非检查异常unchecked exception其他的都是检查异常)
输入:
~~~
$ java ClassDeclarationSpy java.security.Identity
~~~
输出:
~~~
Class:
  java.security.Identity

Modifiers:
  public abstract

Type Parameters:
  -- No Type Parameters --

Implemented Interfaces:
  interface java.security.Principal
  interface java.io.Serializable

Inheritance Path:
  java.lang.Object

Annotations:
  @java.lang.Deprecated()
~~~
从输出中的注解我们java.lang.Deprecated可以知道java.security.Identity是一个不推荐再使用的deprecated API. 这可以被反射代码检测出哪些是不推荐再使用的API. 
注: 不是所有的注解都能通过反射获取, 只有RUNTIME(注解记录在类文件并且运行时VM保留)在java.lang.annotation.RetentionPolicy中的是可访问的. 在预定义的三个注解 @Deprecated, @Override, 和 @SuppressWarnings中, 只有@Deprecated在运行时可用. 

## 发现类成员

Class类中提供了两类方法来访问字段, 方法和构造器: 枚举这些成员的方法和搜索特定的成员的方法. 同样有不同的方法来直接访问类的成员与在超类接口和超类中寻找继承成员. 下面的表格总结了所有定位成员的方法和特点. 
定位字段的类方法: 
<table><tr>
<th>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html"><code>Class</code></a> API</th>
<th>List of members?</th>
<th>Inherited members?</th>
<th>Private members? <!-- Field =====================--></th>
</tr>
<tr>
<td>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredField-java.lang.String-"><code>getDeclaredField()</code></a></td>
<td align="center">no</td>
<td align="center">no</td>
<td align="center">yes</td>
</tr>
<tr>
<td>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getField-java.lang.String-"><code>getField()</code></a></td>
<td align="center">no</td>
<td align="center">yes</td>
<td align="center">no</td>
</tr>
<tr>
<td>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredFields--"><code>getDeclaredFields()</code></a></td>
<td align="center">yes</td>
<td align="center">no</td>
<td align="center">yes</td>
</tr>
<tr>
<td>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getFields--"><code>getFields()</code></a></td>
<td align="center">yes</td>
<td align="center">yes</td>
<td align="center">no</td>
</tr>
</table>
定位方法的类方法: 
<table><tr>
<th>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html"><code>Class</code></a> API</th>
<th>List of members?</th>
<th>Inherited members?</th>
<th>Private members?</th>
</tr>
<tr>
<td>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredMethod-java.lang.String-java.lang.Class...-"><code>getDeclaredMethod()</code></a></td>
<td align="center">no</td>
<td align="center">no</td>
<td align="center">yes</td>
</tr>
<tr>
<td>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethod-java.lang.String-java.lang.Class...-"><code>getMethod()</code></a></td>
<td align="center">no</td>
<td align="center">yes</td>
<td align="center">no</td>
</tr>
<tr>
<td>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredMethods--"><code>getDeclaredMethods()</code></a></td>
<td align="center">yes</td>
<td align="center">no</td>
<td align="center">yes</td>
</tr>
<tr>
<td>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethods--"><code>getMethods()</code></a></td>
<td align="center">yes</td>
<td align="center">yes</td>
<td align="center">no</td>
</tr>
</table>
定位构造方法的类方法: 
<table><tr>
<th>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html"><code>Class</code></a> API</th>
<th>List of members?</th>
<th>Inherited members?</th>
<th>Private members?</th>
</tr>
<tr>
<td>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredConstructor-java.lang.Class...-"><code>getDeclaredConstructor()</code></a></td>
<td align="center">no</td>
<td align="center">N/A<sup>1</sup></td>
<td align="center">yes</td>
</tr>
<tr>
<td>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructor-java.lang.Class...-"><code>getConstructor()</code></a></td>
<td align="center">no</td>
<td align="center">N/A<sup>1</sup></td>
<td align="center">no</td>
</tr>
<tr>
<td>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredConstructors--"><code>getDeclaredConstructors()</code></a></td>
<td align="center">yes</td>
<td align="center">N/A<sup>1</sup></td>
<td align="center">yes</td>
</tr>
<tr>
<td>
<a class="APILink" target="_blank" href="https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructors--"><code>getConstructors()</code></a></td>
<td align="center">yes</td>
<td align="center">N/A<sup>1</sup></td>
<td align="center">no</td>
</tr>
</table>
注1: 构造函数不继承. 
给定一个类名和感兴趣的成员, ClassSpy例子会使用get*s()方法决定列出的元素, 包括继承的内容. 
官方给出的例子: 
~~~
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;
import java.lang.reflect.Member;
import static java.lang.System.out;

enum ClassMember { CONSTRUCTOR, FIELD, METHOD, CLASS, ALL }

public class ClassSpy {
    public static void main(String[] args) {
        try {
            args = new String[]{"java.lang.Integer", "ALL"};
            Class<?> c = Class.forName(args[0]);
            out.format("Class:%n  %s%n%n", c.getCanonicalName());

            Package p = c.getPackage();
            out.format("Package:%n  %s%n%n",
                    (p != null ? p.getName() : "-- No Package --"));

            for (int i = 1; i < args.length; i++) {
                switch (ClassMember.valueOf(args[i])) {
                    case CONSTRUCTOR:
                        printMembers(c.getConstructors(), "Constructor");
                        break;
                    case FIELD:
                        printMembers(c.getFields(), "Fields");
                        break;
                    case METHOD:
                        printMembers(c.getMethods(), "Methods");
                        break;
                    case CLASS:
                        printClasses(c);
                        break;
                    case ALL:
                        printMembers(c.getConstructors(), "Constuctors");
                        printMembers(c.getFields(), "Fields");
                        printMembers(c.getMethods(), "Methods");
                        printClasses(c);
                        break;
                    default:
                        assert false;
                }
            }

            // production code should handle these exceptions more gracefully
        } catch (ClassNotFoundException x) {
            x.printStackTrace();
        }
    }

    private static void printMembers(Member[] mbrs, String s) {
        out.format("%s:%n", s);
        for (Member mbr : mbrs) {
            if (mbr instanceof Field)
                out.format("  %s%n", ((Field)mbr).toGenericString());
            else if (mbr instanceof Constructor)
                out.format("  %s%n", ((Constructor)mbr).toGenericString());
            else if (mbr instanceof Method)
                out.format("  %s%n", ((Method)mbr).toGenericString());
        }
        if (mbrs.length == 0)
            out.format("  -- No %s --%n", s);
        out.format("%n");
    }

    private static void printClasses(Class<?> c) {
        out.format("Classes:%n");
        Class<?>[] clss = c.getClasses();
        for (Class<?> cls : clss)
            out.format("  %s%n", cls.getCanonicalName());
        if (clss.length == 0)
            out.format("  -- No member interfaces, classes, or enums --%n");
        out.format("%n");
    }
}
~~~
这个例子代码非常紧凑, 然而printMembers()方法略显尴尬, 因为在这个方法在早期的反射实现中java.lang.reflect.Member接口里已经存在, 而且在泛型被引入时无法修改它来引入更有用的getGenericString()方法. 唯一的选择就是向上面那样测试和转换, 使用printConstructors(), printFields(), 和 printMethods()来替换, 或者使用相对多余的Member.getName(). 
输入输出样例(当然上面代码中的第12行被我修改了, 如果想使用下面的输入输出删掉就好了): 
~~~
$ java ClassSpy java.lang.ClassCastException CONSTRUCTOR
~~~
输出: 
~~~
Class:
  java.lang.ClassCastException

Package:
  java.lang

Constructor:
  public java.lang.ClassCastException()
  public java.lang.ClassCastException(java.lang.String)
~~~
因为构造函数不继承, 所以定义在直接父类RuntimeException的异常链接机制构造函数(那些拥有Throwable参数的)和其他父类都找不到. 
输入: 
~~~
$ java ClassSpy java.nio.channels.ReadableByteChannel METHOD
~~~
输出: 
~~~
Class:
  java.nio.channels.ReadableByteChannel

Package:
  java.nio.channels

Methods:
  public abstract int java.nio.channels.ReadableByteChannel.read
    (java.nio.ByteBuffer) throws java.io.IOException
  public abstract void java.nio.channels.Channel.close() throws
    java.io.IOException
  public abstract boolean java.nio.channels.Channel.isOpen()
~~~
接口java.nio.channels.ReadableByteChannel定义了一个read()方法. 其余方法均继承于父类接口. 这段代码可以轻松的修改成仅列出某些方法, 把get*s()用getDeclared*s()替代就好了. 
输入(这个ClassMember就是目前写的这个程序): 
~~~
$ java ClassSpy ClassMember FIELD METHOD
~~~
输出: 
~~~
Class:
  ClassMember

Package:
  -- No Package --

Fields:
  public static final ClassMember ClassMember.CONSTRUCTOR
  public static final ClassMember ClassMember.FIELD
  public static final ClassMember ClassMember.METHOD
  public static final ClassMember ClassMember.CLASS
  public static final ClassMember ClassMember.ALL

Methods:
  public static ClassMember ClassMember.valueOf(java.lang.String)
  public static ClassMember[] ClassMember.values()
  public final int java.lang.Enum.hashCode()
  public final int java.lang.Enum.compareTo(E)
  public int java.lang.Enum.compareTo(java.lang.Object)
  public final java.lang.String java.lang.Enum.name()
  public final boolean java.lang.Enum.equals(java.lang.Object)
  public java.lang.String java.lang.Enum.toString()
  public static <T> T java.lang.Enum.valueOf
    (java.lang.Class<T>,java.lang.String)
  public final java.lang.Class<E> java.lang.Enum.getDeclaringClass()
  public final int java.lang.Enum.ordinal()
  public final native java.lang.Class<?> java.lang.Object.getClass()
  public final native void java.lang.Object.wait(long) throws
    java.lang.InterruptedException
  public final void java.lang.Object.wait(long,int) throws
    java.lang.InterruptedException
  public final void java.lang.Object.wait() hrows java.lang.InterruptedException
  public final native void java.lang.Object.notify()
  public final native void java.lang.Object.notifyAll()
~~~
在结果的字段部分, 枚举常量列了出来. 而这些是一些技术上的常量, 将它们和其他字段分清是很有用的. 为此可以修改实例使用java.lang.reflect.Field.isEnumConstant()来达到这个目的. 这个例子在后面的教程检查枚举包含一些可能的实现. (当然看我有没有偷懒继续写了)
在输出的方法部分中, 可以观察到方法名包括声明类. 因此toString()方法是被Enum实现的, 而不是继承自Object. 代码可以修改成Field.getDeclaringClass()来使其更明显. 以下片段说明了潜在的解决方法. 
~~~
if (mbr instanceof Field) {
    Field f = (Field)mbr;
    out.format("  %s%n", f.toGenericString());
    out.format("  -- declared in: %s%n", f.getDeclaringClass());
}
~~~

[故障排除Troubleshooting
](https://docs.oracle.com/javase/tutorial/reflect/class/classTrouble.html)

# 2.成员Members

反射定义了一个java.lang.reflect.Member接口, 被java.lang.reflect.Field, java.lang.reflect.Method, 和 java.lang.reflect.Constructor实现. 接下来会介绍这些类. 对于每个成员, 都将会介绍获取修饰符和信息的一些API, 还有对每个成员都独一无二的操作(例如设置字段值或调用一个方法), 和遇到的常见错误. 每一章会演示一些代码和相关的输出, 这些相关的输出来近似预期一些反射的用途. 
注: 根据[Java语言规范, JavaSE7版](https://docs.oracle.com/javase/specs/jls/se7/html/index.html), 一个类的成员是类体继承的组件包括字段, 方法, 嵌套类, 接口和枚举类型. 因为构造函数不继承, 所以它不是成员. 这与java.lang.reflect.Member实现类不同. 

## 字段Fields

字段拥有类型和值. java.lang.reflect.Field类提供了对给定的对象可以访问类型信息和get, set对应值的一些方法. 
- 获得字段类型Obtaining Field Types章节描述了如何得到字段的声明和通用类型. 
- 检索和解析字段修饰符Retrieving and Parsing Field Modifiers展示了如何得到一部分的字段声明, 例如public或者transient. 
- 读和设置字段值Getting and Setting Field Values展示了如何访问字段值. 
- 故障排除Troubleshooting描述了一些常见的使用混乱的代码错误. 

## 方法Methods

方法拥有返回值, 参数还可能抛出异常. java.lang.reflect.Method类提供了一些用于获取参数和返回值类型信息的方法. 它也可以用于调用给定对象的方法. 
- 获取方法类型信息Obtaining Method Type Information展示如何去列举类中声明的方法和获取它的类型信息. 
- 获取方法参数名Obtaining Names of Method Parameters展示了如何检索方法参数或构造器参数的名字和其他信息. 
- 检索和解析方法修饰符Retrieving and Parsing Method Modifiers阐述了如何访问和解析方法的修饰符和其他信息. 
- 调用方法Invoking Methods展示如何执行一个方法并获得返回值. 
- 故障排除Troubleshooting描述了一些常见的错误在找或者调用一个方法时.
 
## 构造器Constructors

反射中关于构造器的API定义在java.lang.reflect.Constructor, 和上面的方法很像, 但有两处不一样: 首先构造器没有返回值; 其次构造器的调用是用给定类创建一个新的实例. 
- 寻找构造器Finding Constructors展示根据特定参数寻找构造器. 
- 检索和解析构造器的修饰符Retrieving and Parsing Constructor Modifiers展示如何获取一个构造器的修饰符和其他信息. 
- 创建新的实例Creating New Class Instances展示了如何通过调用构造器去实例化一个对象. 
- 故障排除Troubleshooting描述了一些常见的错误在找或者调用一个构造函数时.

(偷懒了后面内容和前面差不多的啦. 想看哪个接着看吧) 
[Members](https://docs.oracle.com/javase/tutorial/reflect/member/index.html)