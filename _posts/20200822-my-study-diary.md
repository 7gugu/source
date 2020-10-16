---
title: 学习日志
original: true
date: 2020-08-22
updated: 
tags: 
- Note
urlname: my-study-diary
---
记录一下每天的疑问, 以及解答, 回顾. 
<!--more-->
# 2020

08/19 Vmware三种网络模式? 用桥接可以使得局域网内可访问但ip网关不固定需看情况调整, host-only可固定网关适合本地一些固定的服务. 
08/21 不错的SpringBoot脚手架? Java读取处理Excel文件? 
08/24 MySQL做left join时, mysql file sort排序效率优化? Java8可以用Optional优雅解决NPE(NullPointerException)问题, 但可读性是个问题所以哪一年闲着没事再看看吧. serialVersionUID问题, 一旦类实现了Serializable接口, 建议必须手动写一个serialVersionUID, 该id可以说是类的版本, 不变的话就向下兼容, 否则会抛出序列化运行时异常. 
08/25 MySQL join和union效率? 
08/26 过滤器Filter详解? 拦截器详解? MyBatis不需要对List, Set做null校验, 但是Map还有其他包装类型需要. [viceAres](https://blog.csdn.net/viceAres/article/details/100549272) Filter和Interceptor区别?
08/31 SLF4J了解? 单元测试中的assert验证? 常见的攻击以及正确的预防CSRF, XSS, 重放等? Mysql Explain的使用? Mysql分页原理? 
09/10 mysql数据库底层是用什么结构, 插入或者删除修改发生了什么? MySQL的BTREE索引和HASH索引? 
09/22 架构学习, 优秀的后端架构应该如何设计? (入参怎么接收, 参数校验, 分页, 数据库时间, 删除, 错误处理, 响应封装, 各层关系与调用, 其他层调用时返回实体还是vo, )