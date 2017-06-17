---

layout: post
title: 如何在Windows下配置Java Web开发环境
date: 2016-11-19
tags: [configuration]

---

目录
* TOC 
{:toc}


>开发环境及软件包准备：Windows 7 64bit , Eclipse EE Mars , JDK 1.8 , Tomcat 7.0

## 安装JDK

Java安装路径(C:\Program Files\Java) ,下面有jre(Java运行环境)和jdk（Java开发工具）文件夹，准确来说jdk下包含jre,而jre下的lib文件夹包含JVM内容。

## 配置JDK环境变量

>相当于给你安装的JDK路径取个别名---需要新建三个变量

 + *JAVA_HOME*  ：   C:\Program Files\Java\jdk1.8.0_101 （**JDK路径**）

 + *CLASSPATH*  ： .;%JAVA_HOME%\lib; (注意前面的"."还有前后的";"。这其实是配置系统**PATH找JVM**的路径)

 +  *PATH*       :   %JAVA_HOME%\bin(新增系统PATH找JVM和Tomcat路径) 

![配置环境变量图示1.png](/images/posts/JavaWeb/配置环境变量.png)

![配置环境变量图示2.png](/images/posts/JavaWeb/配置环境变量1.png)

**确认是否配置JDK成功方法：能够在cmd(管理员)窗口正常运行java, javac, java -version等命令**

## Tomcat的安装与配置

Tomcat安装路径（D:\Java Web\apache-tomcat-7.0.70-windows-x64\apache-tomcat-7.0.70），Tomcat官网上下载对应系统版本的压缩包，解压至想要安装的路径。

### 配置Tomcat环境变量

需要新建两个个变量

 + *TOMCAT_HOME* ： 下载的tomcat解压路径（同 CATALINA_HOME ）

 + *CATALINA_HOME*(相当于 TOMCAT_HOME ) :  D:\Java Web\apache-tomcat-7.0.70-windows-x64\apache-tomcat-7.0.70  

修改变量*PATH*： 系统变量中找到Path变量名， 双击或点击编辑， 在末尾添加如下内容  ;%CATALINA_HOME%\bin;%CATALINA_HOME%\lib

**注意各变量值之间的";"**

### 测试是否正常配置和安装Tomcat(启动Tomcat服务器):

在cmd命令窗口下输入startup回车, 在浏览器(火狐或者Chrome)中输入http://127.0.0.1:8080

有**小猫图片**出来则正常启动

![Tomcat安装路径及目录结构.png](/images/posts/JavaWeb/Tomcat安装路径及目录结构.png)

![Tomcat运行成功命令.png](/images/posts/JavaWeb/tomcat运行成功命令行.png)

![Tomcat服务成功网页显示.png](/images/posts/JavaWeb/tomcat服务成功网页显示.png)

## 将Tomcat服务配置到Eclipse

![Eclipse下配置Tomcat服务.png](/images/posts/JavaWeb/Eclipse下配置Tomcat服务.png)

![Eclipse下配置Tomcat服务编辑服务器页面.png](/images/posts/JavaWeb/Eclipse下配置Tomcat服务编辑服务器页面.png)

 *将web工程发布至tomcat----run on server，测试是否成功将Tomcat服务部署到Eclipse(测试一个简单的java web项目)-*

![新建测试用动态java web项目.png](/images/posts/JavaWeb/新建测试用动态java_web项目.png)

![Run On Server配置页面.png](/images/posts/JavaWeb/Run_On_Server配置页面.png)

![Web工程发布到Server.png](/images/posts/JavaWeb/web工程发布到server.png)

在WebContent下新建New.jsp文件，编辑New.jsp文件中的body标签中的内容, 运行服务（run）,看到在 http://127.0.0.1:8080/Test2/New.jsp 有对应显示，则部署成功.

![测试项目目录结构及文件内容.png](/images/posts/JavaWeb/测试项目目录结构及文件内容.png)

![配置动态web说明.png](/images/posts/JavaWeb/配置动态web说明.png)

![部署Tomcat服务至Eclipse成功界面.png](/images/posts/JavaWeb/部署Tomcat服务至Eclipse成功界面.png)

## 问题记录

* 端口占用问题（原因：有一个Tomcat实例已在运行，要在tomcat的bin目录下执行shutdown脚本）

* 多个项目运行tomcat(tomcat一次只能服务于一个项目，先remove不要Tomcat服务的项目)

## 参考

+ [JRE与JDK的区别](http://swiftlet.net/archives/639)

+ [windows 7系统安装与配置Tomcat服务器环境](http://jingyan.baidu.com/article/624e7459a7d6e734e9ba5a70.html)

+ [如何在Eclipse配置Tomcat服务器](http://jingyan.baidu.com/article/3065b3b6efa9d7becff8a4c6.html)

+ [windows 7系统安装与配置Tomcat服务器环境](http://jingyan.baidu.com/article/624e7459a7d6e734e9ba5a70.html)

+ [端口占用冲突解决方案参考](http://stackoverflow.com/questions/5064733/several-ports-8005-8080-8009-required-by-tomcat-server-at-localhost-are-alre)









