---
title: SpringBoot+Vue踩的坑(Spring Cloud)
original: false
date: 2020-05-28
updated: 2020-05-28
tags: 
  - Spring Boot
  - Vue
  - Spring Cloud
urlname: spring-boot-vue-integration-notes
---
集成Spring Boot+Vue, 列出踩坑过程. 
<!--more-->

首先是跨域的坑, 有时候因为配置了拦截器没有符和拦截器规范所以没数据返回. 

然后是前端POST数据到后端, 是应该传输JSON还是form data的坑. 
每个人的习惯不一样, 我觉得前端除了传文件, 其他都用JSON吧. 

Spring cloud zuul 跨域访问的坑:

网关cors处理没有，服务cors处理没有，挂。

网关cors处理没有，服务cors处理有，挂。

网关cors处理有，服务cors处理有，挂。

网关cors处理有，服务cors处理没有，ok。

Feign的坑: 

Feign无法传递多个参数, 所以对于复杂的逻辑可能需要写VO. 或者使用JackSon提供的ObjectMapper. 

参考: 
[Request Payload 和 Form Data 的区别](https://www.cnblogs.com/hanguidong/p/12091140.html)