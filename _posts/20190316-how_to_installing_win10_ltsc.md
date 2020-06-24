---
title: 如何安装纯净的win10LTSC
date: 2019-03-16 
updated: 2020-04-30
tags: 
  - Win10
urlname: how_to_installing_win10_ltsc
original: true
---
安装环境:一台上了固态把光驱换成机械的装了win10 1809版的有点问题的华硕笔记本,然后
第一次装LTSC啦.
<!--more-->
当然还是在一些大佬的帮助下才顺利的装好.
# 1.下载win10LTSC镜像文件
首先需要下载LTSC版本的镜像,在这里建议大家去[itellyou.cn](https://msdn.itellyou.cn/)下载,点开网址,在'操作系统'下
就能看到win....LTSC 2019之类的了,选择与自己电脑位数对应的版本,大家可以用迅雷之类
支持ed2k链接的下载器就好,我用的是eMule.下载完最好校验一下文件的SHA1之类的值,当然
你觉得没问题的话可以略过.
# 2.格式化U盘,并将镜像解压到U盘.
下载完后我们还需要一个U盘,然后把U盘格式化,记住U盘文件格式要选FAT32的,不要选NTFS.
最后把下载完的镜像直接解压到U盘,解压完是这个样子:
![U盘里的文件截图](/picture/20190316-0.jpg)
可以看到U盘是FAT32文件格式的.
# 3.插入U盘,进BIOS从U盘启动就可以开始安装了
每台电脑进BIOS的方式都不一样,这里我就不详细写了,网上搜吧.
# 4.格式化分区(可选)
如果有跟我一样遇到格式化完分区之后选择磁盘还是没办法安装的情况,可以按Shift + F10(旧版是Alt + F10)
使用diskpart工具,查看对应的驱动器,比如我的固态硬盘是驱动器0,我想格式化它,然后键入命令
~~~
diskpart
select disk 0
clean
~~~
确定好UEFI+GPT或者BIOS+MBR其中一种主流引导方式,然后使用diskpart工具格式化你的磁盘.
然后刷新,上面的disk 0 是我的固态硬盘,大家可以对着自己需要清的盘符来改,然后就好了.

# 写在最后
到这里我就不接着说了,装就是了,如果出现蓝屏之类的可以考虑再重装.本人也是装机小白,有什么建议的话可以联系我啦.
安装微软商店参考:[LTSC-Add-MicrosoftStore](https://github.com/kkkgo/LTSC-Add-MicrosoftStore)