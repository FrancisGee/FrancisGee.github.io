---

layout: post
title: Linux配置caffe环境(GPU版)
date: 2017-10-12
tag: [configuration]

---

目录

* TOC 
{:toc}


>**如果你想折腾一个人，那么就让他(她)装cafee吧.**

## 引言

Caffe是一个非常难安装的框架，看它的issue就知道了。这篇博客主要是给某个环境安装Caffe来一个**记忆化缓存**,加快重新安装的速度。

当然，如果已经有对应的云服务器提供深度学习服务，不建议安装，毕竟把**时间花在更有意义的事情上比较好**。

既然我已经安装好了Caffe环境，本着学习研究的态度，分析下原因和原理对于读者也是极好的。


## 硬件环境和软件包

+ [参考tensorflow的GPU安装环境](http://www.yikanggao.com/blog/2017/08/Linux%E9%85%8D%E7%BD%AEtensorflow%E7%8E%AF%E5%A2%83(GPU).html)

**新加软件包**: *Caffe*, *OpenCV 3.1*

**Tips** : 这里要特殊说明下显卡驱动，N卡的显卡驱动在Ubuntu16.04 LTS系统上用得太酸爽....经常搞的电脑无法进入桌面模式....造成恐慌....只有无脑卸载驱动...
           
           这里要说的是**可以在CUDA和cudnn安装好的基础上再安装驱动**，先后关系无所谓。但不要用系统---软件和更新中的驱动自动安装，否则会有麻烦。
           
           自己在tty1模式下手动安装，安装好了之后，用N卡(独显)驱动主要做并行计算，用集显来做显示功能，免得占用计算内存。
           
           
## 安装OpenCV 3.1

在Caffe的安装说明中是可以不用安装OpenCV的，我也试过不安装OpenCV跑通程序，但是再次安装出现问题，跑不通。

所以为了避免出现问题，索性一次安装到位，安装**OpenCV 3.1**...

之前[博客](http://www.yikanggao.com/blog/2017/03/%E5%AE%89%E8%A3%85OpenCV.html)中写过在Linux下安装OpenCV 2.X, 所以我把安装OpenCV 3.1的过程加到那篇博客中.

## 安装Caffe

默认你已经完成了上面的步骤，下面详细讲解安装Caffe的过程。

安装的流程以[官网的安装说明](http://caffe.berkeleyvision.org/installation.html)为主，这里主要是分析原理和安装细节。


### 安装依赖库

首先说下Caffe**特有的依赖库**...

+ Boost >= 1.55 
+ BLAS via ATLAS, MKL, or OpenBLAS.
+ protobuf, glog, gflags, hdf5

由于Caffe是用**C++**写的，所以需要C++的**线性代数库**BLAS和Boost(调用Python的C++).

*Boost其实是C++的STL的补充库*

```shell

sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libopenblas-dev liblapack-dev libatlas-base-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
sudo apt-get install git cmake build-essential

```
一定要确保以上依赖包都已经安装成功，验证方法是重新运行安装命令..


### 配置Makefile.config文件

安装Caffe的精华就在这里，我大概读了3-4遍这个文件。

我主要说明其中关于**Python配置**特别重要的改动, 其它的参考我给出的链接。

其实主要保证一个原则: 让编译器找到python头文件，不管是anaconda的python还是原生python, 不管是py2.x还是py3.x, 但是一定保证**选择不冲突**。

**比如我这里选择的是Anaconda3的py3.X**


```shell

     ANACONDA_HOME := $(HOME)/anaconda3
     PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
		    $(ANACONDA_HOME)/include/python3.5m \
		    $(ANACONDA_HOME)/lib/python3.5/site-packages/numpy/core/include
		
		
		

    PYTHON_LIBRARIES := boost_python-py35 python3.5m

    PYTHON_LIB := $(ANACONDA_HOME)/lib
    
```

其实就是配好**PYTHON_INCLUDE**，**PYTHON_LIBRARIES**，**PYTHON_LIB**这几个变量的正确路径。

**Ubuntu 16.04的细节**:

*Whatever else you find you need goes here.*


```shell

INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial

```

这是由于Ubuntu16.04系统文件目录的改变hdf5的文件位置


### 修改Makefile文件

改　LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_hl hdf５
为 LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_serial_hl hdf5_serial opencv_imgcodecs opencv_core opencv_highgui opencv_imgproc opencv_imgcodecs

这是因为由于需要opencv，所以**加了编译所需opencv的库**


### 测试安装

运行:
　
```shell
　
     make all -j8
     make test -j8
     make runtest -j8
     
```    

如果没有问题，则说明安装成功。     
     
     
## 配置Caffe的python接口

执行命令


```shell

make pycaffe -j8

```

运行通过后，写入 **~/.bashrc文件(类似win安装软件的修改PATH)**

在~/.bashrc中写入

```shell

export PYTHONPATH=/home/gaoyikang/caffe/python:$PYTHONPATH

```



## 疑难杂症
1. Caffe编译出错, **pyconfig.h: No such file or directory**
   解决方案: 安装python-dev等依赖, **sudo apt-get install python-dev libxml2-dev libxslt-dev**


## 总结     

还是按照官网安装，之所以困难还是有些东西没搞清楚，不可以盲目参考别人的教程，要结合教程分析自己的问题..




## 参考


+ [Caffe安装官方教程](http://caffe.berkeleyvision.org/install_apt.html)
+ [Caffe依赖库安装](http://blog.csdn.net/yhaolpz/article/details/71375762)
+ [修改Makefile文件参考](http://blog.csdn.net/yhaolpz/article/details/71375762)
+ [pyconfig.h问题解决参考](https://github.com/okfn/piati/issues/65)













