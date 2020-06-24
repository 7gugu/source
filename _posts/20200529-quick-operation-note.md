---
title: 快速操作笔记
original: false
date: 2020-05-29
updated: 2020-05-29
tags: 
- 笔记
urlname: quick-operation-note
---
那些搜索过两次以上的解决办法. 
<!--more-->

# Linux

## CentOS7 ifconfig command not found

~~~
// 安装net-tools
sudo yum install net-tools
~~~

## CentOS7修改软件源: 
~~~
// 下面的操作结果还有点问题, 懒得搞了
cd /etc/yum.repos.d/
// 安装weget
yum install -y wget
// 下载CentOS 7的repo文件
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repop://mirrors.aliyun.com/repo/Centos-7.repo
// 或者
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
// 清除缓存
yum clean all
// 生成缓存
yum makecache
~~~

## CentOS7安装Docker

~~~
// 切换root用户
// 照着官网教程跑, 修改为阿里云的镜像
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
// 启动并加入开机启动
systemctl start docker
systemctl enable docker
~~~

## CentOS7防火墙

~~~
// 开启防火墙
systemctl start firewalld
// 关闭防火墙
systemctl stop firewalld.service
// 查看已开放的端口(默认不开放任何端口)
firewall-cmd --list-ports
// 开启80端口
// --zone(作用域), --add-port(端口和访问类型) , --permanent(永久生效) 记得重启
firewall-cmd --zone=public --add-port=80/tcp --permanent
// 重启防火墙
firewall-cmd --reload
// 禁止防火墙开机启动
systemctl disable firewalld.service
// 删除
firewall-cmd --zone= public --remove-port=80/tcp --permanent
~~~

## CentOS7使用Docker

~~~
// 列出所有镜像
docker images
// 删除镜像(需要先停止容器)
docker rmi image_id
// 运行 --name(容器名) -i(交互模式) -t(tty打开终端) -d(后台运行)(可以合成-itd) -p 容器外:容器内(端口映射) image-name:tag(版本) 
docker run --name container-name -d image-name:tag
如:docker run --name myredis –d redis
// 查看运行中容器命令 -a(全部)
docker ps
// 停止容器
docker stop container_id
// 启动容器
docker start container_id
// 删除容器
docker rm container_id
// 进入容器
docker exec -it container_id /bin/bash
// 使用当前目录的 Dockerfile 创建镜像，镜像名字:版本号
docker build -t 名字:版本号 .
示例: docker build -t runoob/ubuntu:v1 .
// 启动时配置文件挂载 
示例: docker run -p 82:80 --name nginx1 -v /src/nginx/nginx.conf:/etc/nginx/nginx.conf -d nginx
// 拷贝文件进容器(覆盖)
示例: docker cp nginx.conf nginx1:/etc/nginx/nginx.conf

~~~
错误: 
WARNING: IPv4 forwarding is disabled. Networking will not work.
~~~
// 有些容器好像默认是ipv6, 需要指定ipv4, 或者修改系统配置:
 /usr/lib/sysctl.d/00-system.conf
// 中添加
net.ipv4.ip_forward=1
// 再重启
 systemctl restart network
~~~

# Java

~~~
// 服务器运行jar定义输出
nohup java -jar getCimiss-surf.jar >consoleMsg.log 2>&1 &

tail -fn 50 nohup.out
~~~
