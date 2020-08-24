---
title: 如何快速使用Git
date: 2019-02-18 20:39:13
updated: 2020-04-30
tags:
  - Git
  - Note
urlname: how_to_quickly_use_git
original: false
---
快速上手类内容注意更新日期, 因为技术迭代很快教程可能对不上, 以官网为主.
<!--more-->
# Cues
自报家门, 生成ssh, 关联远程
add, commit, push
# Notes
## 1.初始化配置
安装完Git自报家门:
~~~
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
~~~
win下~目录生成ssh密钥:
~~~
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
~~~
关联远程仓库(git默认远程名字叫origin):
~~~
git remote add origin SSH仓库地址
~~~
## 2.开始使用
初始化Git仓库:
~~~
git init
~~~
## 3.添加与提交
添加到缓存区(Stage):
~~~ 
git add <file>
~~~
撤销(回到最近一次add/commit):
~~~
git checkout <file>
~~~
提交到仓库:
~~~
git commit -m "提交说明"
~~~
暂存区概念图:
![0](/picture/20190218-0.jpg)
		
## 4.推送改动
推送到远程:
~~~
git push origin master
~~~
推送失败(远程分支比本地新),试图合并,解决冲突后再push:
~~~
git pull
~~~
## 5.版本回退
回退上个版本/回退上上个版本/回退上100个版本
~~~
git reset --hard HEAD^ 
git reset --hard HEAD^^ 
git reset --hard HEAD~100 
~~~
通过commit id回退
~~~
git reset --hard commit_id
~~~
## 6.查看版本log
参数为简化信息,commit id由SHA1计算出来
~~~
git log --pretty=oneline
~~~
后悔药,记录了你每一次git操作
~~~
git reflog
~~~
## 7.分支操作
查看当前分支
~~~
git branch
~~~
创建dev分支
~~~
git branch dev
~~~
切换到dev分支:
~~~
git checkout dev
~~~
创建并切换分支:
~~~
git checkout -b dev
~~~
删除本地dev分支:
~~~
git branch -d dev
~~~
删除远程dev分支(分之前冒号代表删除):
~~~
 git push origin :dev 
~~~
合并分支(当前为master,合并dev分支到master)
~~~
git merge dev
~~~
禁用fast forward的合并:
~~~
git merge --no-ff -m "merge with no-ff" dev
~~~
分支合并情况:
~~~
git log --graph --pretty=oneline --abbrev-commit
~~~
推送分支:
~~~
git push origin <branch>
~~~
## 8.标签操作
查看所有标签:
~~~
git tag
~~~
查看标签信息:
~~~
git show <tag name>
~~~
打标签
~~~
git tag <name>
~~~
指定commit id打标签:
~~~
git tag <tag name> <commit id>
~~~
删除标签
~~~
git tag -d <name>
~~~
推送标签到远程:
~~~
git push origin <tag name>
~~~
删除一个远程标签:
~~~
git push origin :refs/tags/<tag name>
~~~
## 9.其他操作
关联多个远程:
关联github远程库:
~~~
git remote add github git@github.com:michaelliao/learngit.git
~~~
关联码云远程库:
~~~
git remote add gitee git@gitee.com:liaoxuefeng/learngit.git
~~~
移除关联: 
~~~
git remote remove {REMOVE_NAME}
~~~

## Summary
git上手难度还是比较高的, 只有理解和多练习了, 才能熟练掌握git.

参考资料:
[Git教程-廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
[git使用简易指南](http://www.bootcss.com/p/git-guide/)
