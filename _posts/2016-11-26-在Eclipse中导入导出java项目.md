---
layout: post
title: 在Eclipse中导入导出java项目
date: 2016-11-26
tags: [tools]
---

目录

* TOC 
{:toc}


>在IDE中导入导出项目是很频繁的事情，这里已自己用Eclipse导入导出java项目，以及遇到的问题进行下记录，希望对读者有所帮助

## 在Eclipse中导出项目

选中要导出的项目--File---Export--General--Archive File

![Eclipse导出项目示意图.png](/images/posts/Eclipse/Eclipse中导出项目.png)

点击Next,在Browse中输入要导入的文件加，点击Finish,导出成功，这里默认是xx.zip格式

## 在Eclipse中导入项目

File---Import--General--Existing Projects into Workspace

点击Next后，在Browse中找到导出的（已有的）xx.zip,然后点击Finish,导入完成。

### 解决外部jar包和系统jar包问题

+ 因为导入的是完整项目，所以jar包依赖是之前的jar包路径，所以要重新配置jar包。

首先删除失效jar包，找到有效jar包。

Add External JARS---找到外部jar包装上

Add Library----找到合适的系统jar包

这两步做完后，相当于配置好项目所用的jar包，并且把JRE(java运行环境)配置为JDK1.7,可是这样程序是运行不起来的，因为项目默认的JRE是在JDK1.8下，需要将项目的JRE运行环境调整。

![JRE运行环境错误.png](/images/posts/Eclipse/运行环境错误.png)

+ 另外，之前的项目用的JDK为1.8，但是当前系统只有JDK1.7,这个编译问题也要解决

当然，可以下载并且安装JDK1.8解决，但是JDK1.7对于程序也够用，所以采用如下解决方案。

选中项目右键---Properties---Java Complier

将*Complier compliance level*改为1.7,出来对话框点击yes，就可以成功运行

### 解决输入输出乱码问题

拿到项目，发现输入输出都是乱码，原因是**字符集编码不一致**。

解决方法很多,不要在注释中写中文，不要在输出中写中文，这种方案还是有弊端，在中国的公式免不了要在注释中和输出中写中文。

**另外一种方案是团队成员统一编码字符集，都用utf-8(国际化)，这是通法**

但现在要解决问题，也是有办法的。

+ 解决导入文件中文乱码

右键项目--Properities---Resource

![解决导入文件乱码.png](/images/posts/Eclipse/解决导入文件乱码.png)

在Text file encoding中Other填上gbk,导入文件乱码解决

+ 解决输出乱码

右击源码---Run as ---Run Configuration---Encoding--Other---UTF-8,控制台乱码解决

## 参考

+ [Eclipse中导入中文项目乱码问题](http://bbs.csdn.net/topics/390831766?page=1)
+ [Eclipse中导入导出java项目](http://agile.csc.ncsu.edu/SEMaterials/tutorials/import_export/)
+ [添加外部和系统jar包](http://blog.csdn.net/jmyue/article/details/14120387)
+ [解决变更JRE环境问题](http://stackoverflow.com/questions/11350733/how-do-i-convert-my-eclipse-project-to-an-earlier-java-version)

## 拓展阅读
+ [Eclipse--自动代码规范化](http://blog.csdn.net/jmyue/article/details/11060003)
+ [Eclipse--自动代码规范检查工具--CheckStyle](http://blog.csdn.net/jmyue/article/details/11110857)













