---
layout: post
title:  安装Ubuntu与Windows双系统
date: 2016-10-30
tags: [tools]
---


目录

* TOC 
{:toc}


>谨以此文献给单硬盘，双硬盘，想装双系统的孩子。
**分割线**之后为在双硬盘条件下装双系统的经历。

## 预备知识
在安装Linux之前，一定要弄清楚**磁盘分区**,**挂载**，**目录树**等概念,具体可以参阅《鸟哥的Linux私房菜--基础篇》中的说明。需要准备的软件工具是*分区助手*(用于将磁盘分区)和*UltraISO*（用于刻录镜像文件到U盘）

## 分区规划
个人分区规划，仅供参考：用*分区助手*(或者**DiskGenius**)分出100G空闲空间用于装Linux，一般20G就够了，不过我是要用Linux的，考虑到还会有软件安装，所以大点也无妨，反正有空间。/根目录挂载了15G，/usr这个目录与装到系统上的软件有关，分了10G，/var这个目录与邮件等文档有关，分了10G，/home是自己的主文件夹，多多益善。Swap交换空间（推荐1~2G，>=512MB）我分了2G，由于我电脑内存4G，所以分了实际内存一半空间。（/boot(推荐128MB)这个我没有分，听说可以不分，反正网上讲的比较含糊，还不是太懂）
---------------------------------我是分割线--------------------------------------------
这里总共装了两次，一次不分/boot，结果报错**无法将grub安装到/dev/sda(SSD)**，分出/boot就成功了。这里分出的/boot在SSD，并且为**主分区**。其余分区在HDD，分出了100G给/根目录 主分区，8G给swap交换空间 逻辑分区,50G给/usr(这个分区分不分都行)逻辑分区。这里我认为分区的*先后次序*和装系统没有影响，不过一块磁盘上只能有4块主分区，1块逻辑分区(Linux系统怎么可以有多个逻辑分区？)，多个扩展分区。


## 启动引导
采用**U盘安装**的方式和以**Ubuntu引导Windows的系统**引导方式：用UltraISO刻录好镜像文件,将U盘插上电脑（这里有时候会接触不良，所以要插紧点），设置BIOS从U盘启动，然后进行分区，这里要注意**分区的先后次序**（否则可能空闲区域不可用，无法按照预期的分区安装），先分/home等逻辑分区，再分swap,最后分/根目录等主分区（挂载点设为\），就能成功安装。

-------------------------------我是分割线--------------------------------------------
这里要说明的是/boot分区有Linux的boot loader信息。所以与引导系统有关，不过我在EasyBCD软件创建用/boot所在磁盘的引导加载却并没成功。反而是在开机时点击ESC退出，选择SSD那个磁盘启动，反而能够进去Ubuntu。虽然SSD中有Ubuntu的/boot信息，但是也有Win10的MBR(主引导记录)，怎么就能识别要开机Ubuntu呢？


## 安装后的后续工作

之前为了加速系统安装，很多时候都跳过了，所以要sudo apt-get **update**。然后配置好谷歌和vim。然后根据自己的需求安装软件。

## 参考
+ [Linux中各个目录的意义和内容](http://cn.linux.vbird.org/linux_basic/0210filepermission_3.php)
+ [分区助手](http://www.disktool.cn/)
+ [UltraISO](https://www.ezbsystems.com/ultraiso/)
+ [安装win8.1和Ubuntu14.04双系统](http://blog.csdn.net/u012280578/article/details/50017153)
+ [SSD迁移攻略](http://blog.csdn.net/u012280578/article/details/57475582)

