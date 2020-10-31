---
title: Spring的学习
original: false
date: 2020-10-29
updated: 
tags: 
  - Spring
  - Java
  - Note
urlname: spring-learning
---
记录Spring的学习笔记
<!--more-->
# 1. Spring整体设计理念和整体架构

# 2. Spring Ioc

# 3. Spring AOP

## 3.1 为什么会有AOP? 

如果你写过基于Spring或者Spring Boot相关的项目, 那你可能会遇到以下的问题. 
比如说: 

- 对于某些方法需要记录日志
- 对某些方法的执行前需要对用户进行权限判定

以上的这些功能, 如果直接写在业务代码中, 那么每个方法都会调用一次这些公用的方法. 

![各method都会调用log()方法](/picture/2020-10-29-11-39-46.png)

很明显这样显示调用的代码难以管理, 一旦我的log方法一变动, 或者说我有些方法不需要调用这个log()了, 删减method中的代码是一件麻烦事. 
这时候就可以考虑使用AOP了, AOP和OOP一样是一种观念, 全称为Aspect-oriented programming, 翻译过来叫面向切面编程. 它可以实现对这些重复代码单独抽取出来进行管理维护, 在需要时统一调用, 以及对于如何灵活使用这些公共代码提供了支持. 为了更好的使用AOP技术, 技术专家们是专门成立了一个AOP联盟来探讨AOP标准化, 因此你可以在AOP联盟文档中找到相关介绍. 

## 3.2 Spring AOP中的一些概念

这里直接拿[Spring文档中对AOP的介绍](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop)来讲一下. 

- Aspect 切面: Aspect是一种模块化的观念, 这种观念将横切经过多个类, 所以翻译过来就是切面. 在企业Java应用中, 事务管理是横切观念很好的例子. 在Spring AOP中, 切面可以通过基于配置文件(配置xml的方式), 或者基于@Aspect注解的类. (这里讲的非常抽象, 把它想成管理切面相关的的配置就好了)

- Join Point 连接点: 在程序执行过程中的一个点, 例如方法的执行或者异常的处理. 在Spring AOP中这个连接点总是代表方法执行. (可以理解成就是类中的方法就是连接点)

- Advice 通知: 特定切面在特定的连接点执行的操作. 通知有不同的类型, 包括"前置通知", "后置通知", "环绕通知"等. (其实就是在什么时候执行, 前置就是在目标方法执行前执行, 后置就是目标方法后执行...) 许多AOP框架, 包括Spring, 将通知建模成为一个拦截器, 并且在连接点周围维护一条由拦截器组成的拦截链. (这就是Spring对Advice的实现方式了. ) 

- Pointcut 切入点: 切入点是表达连接点的谓词. (就是连接点的集合) 通知是和这个切入点表达式相关联的, 它会在任何匹配切入点的连接点上执行. (例如, 在特定方法上执行通知). 这种根据切点表达式匹配连接点的观念是AOP的核心, 而Spring默认使用AspectJ切入点表达式)

- Introduction 引入: Declaring additional methods or fields on behalf of a type. Spring AOP lets you introduce new interfaces (and a corresponding implementation) to any advised object. For example, you could use an introduction to make a bean implement an IsModified interface, to simplify caching. (An introduction is known as an inter-type declaration in the AspectJ community.) (TODO: 我不懂求大佬带带)

- Target Object 目标对象: 一个即将被一个或者多个切面增强的对象. (增强也就是被通知影响) 也成为"通知对象". 由于Spring AOP是根据运行时代理实现的, 所以这个对象总是一个代理对象. 

- AOP proxy AOP代理: 一个为了实现切面契约的AOP框架创建的对象. 在Spring框架中, 它可能是JDK动态代理或者是CGLIB代理. 

- Weaving 编织: 将切面和其他应用程序类型或者对象连接, 来创建一个增强了的对象. 这可能在编译时完成, (例如使用AspectJ编译器) 加载时, 或者运行时. 而Spring AOP和其他纯Java AOP框架一样, 是在运行时完成织入的. 

## 3.3 Java中AOP的实现

### 3.3.1 静态AOP

- 在编译器, 切面直接以字节码形式编译到目标字节码文件中. 

### 3.3.2 动态AOP 

- 在运行时, 目标类加载后, 动态生成代理类, 将切面植入到代理类中. 

#### 3.3.2.1 基于JDK动态代理

基于**Java反射**机制实现, 必须要**实现了接口**的业务类才能用这种办法生成代理对象. 

#### 3.3.2.2 基于CGLIB代理

基于**ASM**机制实现，通过**生成业务类的子类**作为代理类。

### 3.3.3 小结

![](/picture/2020-10-29-16-00-37.png)

不同的AOP实现只是在不同的时刻用不同的工具来实现的, 下面就只讲Spring的AOP. 

## 3.4 Spring AOP的实现

下面就拿Spring 5.2.9的源代码来看咯. 

### 3.4.1 Advice 通知

Advice是AOP联盟定义的一个接口, 可以在org.aopalliance.aop.Advice中找到它. 
而Spring提供了更具体的通知类型, BeforeAdvice, AfterAdvice, ThrowsAdvice等. 

- BeforeAdvice接口: 前置通知的标记接口. 
- AfterAdvice接口: 后置通知的标记接口. 
- ThrowsAdvice接口: 一般抛出异常就由这个接口处理. 

Spring中比BeforeAdvice接口更具体一点的实现是MethodBeforeAdvice接口: 
~~~
public interface MethodBeforeAdvice extends BeforeAdvice {
	void before(Method method, Object[] args, @Nullable Object target) throws Throwable;
}
~~~
它提供了一个before方法, 它会在方法被调用前被调用. 这个通知不能阻止方法调用的过程, 除非它抛出Throwable. 如果方法签名允许, 它抛出的任何异常都会返回给调用方. 否则异常会被包装为运行时异常. 而它的method参数是目标方法的反射对象; args对象数组中包含目标方法的输入参数; target为目标对象. 

下面是AfterAdvice更具体的AfterReturningAdvice接口: 
~~~
public interface AfterReturningAdvice extends AfterAdvice {
	void afterReturning(@Nullable Object returnValue, Method method, Object[] args, @Nullable Object target) throws Throwable;

}
~~~
它提供了afterReturning方法, 顾名思义就是在目标对象方法成功返回值了之后执行. (前提当然目标方法也没抛异常) 它可以获得返回值但是无法改变返回值. 参数returnValue为返回值; 

然后是ThrowsAdvice, 它是继承AfterAdvice: 
~~~
public interface ThrowsAdvice extends AfterAdvice {

}
~~~
但是这个接口并没有规定的方法, 如果需要你需要实现这种形式的方法: 
~~~
void afterThrowing([Method, args, target], ThrowableSubclass);
~~~
其中方括号内的参数是可选的. 如果该异常通知抛出新的异常, 将会覆盖原来的异常. 

### 3.4.2 Pointcut 切点

Spring AOP提供了具体的切点, 比如JdkRegexpMethodPointcut的matches方法是通过JDK来实现正则表达式的匹配的. NameMatchMethodPointcut的matches方法就是使用方法名相同或者方法名相匹配. 

### 3.4.3 Advisor 通知器

通过Advisor可以将Advice和Pointcut结合起来. 

### 后面太难了, 夭折了...

Spring通过jdk和cglib两个精彩的实现了AOP, 虽然我们用起来很简单, 但是Spring实现AOP的过程是非常艰苦的, (是我太菜了看不懂了. ) 

# 4. Spring MVC

## 4.1 网络基础知识

在此之前先学习一下[计算机网络基础知识](/post/basic-computer-network). 






参考: 
- 书籍: Spring技术内幕：深入解析Spring架
- 书籍: 看透Spring_MVC：源代码分析与实践
- [AOP的实现的几种方式](https://blog.csdn.net/csujiangyu/article/details/53455094)
