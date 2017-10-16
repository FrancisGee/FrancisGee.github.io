---

layout: post
title: Caffe模型
date: 2017-10-16
tag: [tools]

---

目录

* TOC 
{:toc}


## 引言

本博客是帮助读者将caffe模型转化成Tensorflow模型，主要的需求为**目前只有预训练好的Caffe模型，但是熟悉Tensorflow环境。**

但是不能保证转化过程一定成功，这里选取的主要工具为**caffe-tensorflow**这个开源工具。

## 依赖安装

由于这个环境默认是用Py2.X版本，所以用Py3.X版本错误很多。不建议折腾，这里**采用conda的env管理。**

```shell

conda create -n caffe-tf python=2 tensorflow caffe
source activate caffe-tf

```

这样就创建好了开源工具所需的环境，并且成功进入环境.

## 转化操作

开源项目**caffe-tensorflow**[地址](https://github.com/ethereon/caffe-tensorflow)

安装这个[示例](https://github.com/ethereon/caffe-tensorflow/tree/master/examples/mnist)操作，基本上就可以了。

## 文件说明


### 输入

xx.prototxt为caffe的网络定义文件，xx.caffemodel为caffe训练好模型的权值

### 输出

xx.py为tf的计算图，xx.npy为tf的模型二进制文件


## 参考

+ [官方测试例子](https://github.com/ethereon/caffe-tensorflow/tree/master/examples/mnist)
+ [Conda环境管理参考](http://blog.csdn.net/u010376591/article/details/66975632)
+ [中文参考](http://ddrpa.github.io/2017/tf-use-caffe-model.html)



