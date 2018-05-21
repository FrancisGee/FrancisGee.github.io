
layout: post
title: Ubuntu配置Hadoop(单机版)
date: 2017-10-12
tag: [configuration]

---

目录

* TOC 
{:toc}


## 引言

Hadoop是大数据工程师必须掌握的工具，也是玩分布式必须的用品. Hadoop共有三种模式，分别为单机模式，伪分布式模式, 完全分布式模式.
这篇博客讲述Hadoop的单机模式，Hadoop单机模式是在本地文件上操作的(没有用到HDFS文件系统), 单机模式主要是用来**调试mapreduce程序**


##  硬件环境和软件包

我这个地方是用的虚拟机装的Ubuntu16.04, 有关虚拟机的安装以及**虚拟机和宿主机共享文件**和**从Xshell登录虚拟机**以后有空再说。**所以现在要做的是安装好Ubuntu16.04**

### 软件依赖

+ JDK 1.8 (jdk-8u171-linux-x64.tar.gz)
+ hadoop 2.7.6 (hadoop-2.7.6.tar.gz)

这里我把JDK安装在**/usr/java**目录, 把hadoop安装在**/usr/local**目录, 就是解压上述文件到对应目录(没有就自己新建目录)

### 设置环境变量

+ 设置Java环境变量:

sudo gedit ~/.bashrc, 在~/.bashrc文件的末尾添加:

export JAVA_HOME=/usr/java/jdk1.8.0_171
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH

+ 设置Hadoop环境变量

sudo gedit ~/.bashrc, 在~/.bashrc文件的末尾添加:

#HADOOP VARIABLES START  
export HADOOP_INSTALL=/usr/local/hadoop  
export PATH=$PATH:$HADOOP_INSTALL/bin  
export PATH=$PATH:$HADOOP_INSTALL/sbin  
export HADOOP_MAPRED_HOME=$HADOOP_INSTALL  
export HADOOP_COMMON_HOME=$HADOOP_INSTALL  
export HADOOP_HDFS_HOME=$HADOOP_INSTALL  
export YARN_HOME=$HADOOP_INSTALL  
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_INSTALL/lib/native  
export HADOOP_OPTS="-Djava.library.path=$HADOOP_INSTALL/lib"  
export CLASSPATH=$($HADOOP_INSTALL/bin/hadoop classpath):$CLASSPATH
#HADOOP VARIABLES END  

+ 设置Hadoop的配置文件绑定到JDK

编辑文件 /usr/local/hadoop/etc/hadoop/hadoop-env.sh

在如下地方添加:

**#The java implementation to use.**
**#export JAVA_HOME=${JAVA_HOME}**

export JAVA_HOME=/usr/java/jdk1.8.0_171
export HADOOP=/usr/local/hadoop  
export PATH=$PATH:/usr/local/hadoop/bin  

最后记得 **source ~/.bashrc**, 使得环境变量生效


### 验证Hadoop环境是否成功安装

hadoop version

## 运行MapReduce例子

新建input目录存放文件, hadoop自动创建output目录并且把输出结果写入.
**注意：如果已经有output目录, hadoop会报错。**

hadoop jar xxxx/hadoop-mapreduce-examples-2.7.6.jar wordcount ~/input ~/output 

## 从Java源码到运行mapreduce程序


javac WordCount.java

WordCount.java(最好用hadoop2.7.6的官方的源码，教程上的)

接着把.class文件打包成jar, 这个时候才能在Hadop中运行:

jar -cvf WordCount.jar ./WordCount*.class

hadoop jar WordCount.jar WordCount input output

## 分析并改写MapReduce源码

## 参考

+ [使用命令行编译打包运行自己的MapReduce程序 Hadoop2.6.0](http://www.powerxing.com/hadoop-build-project-by-shell/)

 
 




