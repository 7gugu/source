---
title: 关于停车场的毕业设计实现过程(未完待续...)
original: true
date: 2020-09-21
updated: 
tags: 
- Note
urlname: implementation-graduation-project-about-parking
---
从0开始做一个毕设吧. 
<!--more-->
# 关于选题

没想到好的点子, 还是不要脸的蹭了老师三年的题目. 

# 确定架构设计

## 基础设施

微服务内容实在是太麻烦了, 一般来说毕设怎么方便怎么来吧, 主要是没这么多时间于是就想着怎么精简(偷懒)了. 
数据库用用过的mysql, 这是跑不掉的, 但是图片却是个问题, 要还是用以前nginx+ftp上传觉得没挑战了, 这里试一下Minio吧(主要是没钱买oss). 

所以vmware虚拟机就跑了mysql 5.7.30和minio. 按照官网直接docker跑: 
~~~
docker run -p 9000:9000 \
  -e "MINIO_ACCESS_KEY=AKIAIOSFODNN7EXAMPLE" \
  -e "MINIO_SECRET_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY" \
  -d \
  minio/minio server /data
~~~
浏览器打开 http://192.168.1.67:9000/ (我host-only虚拟机地址)登录进minio能方便操作了. 然后新建一个bucket, 命名'parking'. 添加Bucket Policy (parking), 为星号且Read Only. 然后随便上传一张图片, 通过标签: 
~~~
<img src="http://192.168.1.67:9000/minio/download/parking/6bhg0qcvi1jy.png?token=">
~~~
就能直接访问了. 真是奇怪的访问方式. 至于为什么要带个token, 可能是我操作方式不对吧, 总之能直接跑了就先这样了. 

## 表结构

要是你自己做, 表结构肯定是根据需求设计滴, 需求定了, 定接口, 想一下大概怎么写, 定表, 开写. 

## 项目后端架构

一个优秀的项目架构, 在自己不懂得怎么搭建时应该是抄过来的. 因为我们是做前后分离, 肯定是搜索引擎搜一波优秀的后端架构, 照着抄就好了. 第一件事创建Maven项目, 然后选个合适的Spring Boot版本继承添加web依赖等等...

