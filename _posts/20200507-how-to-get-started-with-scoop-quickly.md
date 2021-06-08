---
title: 如何快速使用Scoop
original: false
date: 2020-05-07
updated: 2021-04-28
tags: 
  - Win10
urlname: how-to-get-started-with-scoop-quickly
---
快速上手类内容注意更新日期, 因为技术迭代很快教程可能对不上, 以官网为主. 
<!--more-->
Scoop的Win下的包管理器, 这样一些软件安装可能就不需要UAC了. 当然你也可以使用微软官方的WinGet.
首先Powershell这里要修改成允许远程脚本执行: 
~~~
set-executionpolicy remotesigned -scope currentuser
~~~
然后执行远程的脚本进行安装: 
~~~
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
~~~
安装完测试一次下: 
~~~
scoop help
~~~
添加一个软件比较多的bucket: 
~~~
scoop bucket add extras
~~~
查看所有bucket: 
~~~
scoop bucket known
~~~
添加一个大佬维护的bucket: 
~~~
scoop bucket add dorado https://github.com/h404bi/dorado
~~~
搜索微信: 
~~~
scoop search wechat
~~~
安装: 
~~~
scoop install dorado/wechat
~~~

参考: 
[少数派: 「一行代码」搞定软件安装卸载，用 Scoop 管理你的 Windows 软件(https://sspai.com/post/52710](https://sspai.com/post/52496)
[少数派: 给 Scoop 加上这些软件仓库，让它变成强大的 Windows 软件管理器](https://sspai.com/post/52710)
[Scoop官方rep](https://github.com/lukesampson/scoop)
[dorado](https://github.com/h404bi/dorado)
