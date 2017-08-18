---

layout: post
title: Windows配置OpenCV3.X+Python3.x环境
date: 2017-05-17
tag: [tool]

---

目录

* TOC 
{:toc}

>有一个月没有更新博客了，因为实在不知道有什么有用的好东西可以分享给大家，所以很是惭愧。由于最近在研究计算机视觉项目和机器学习方面的项目，实在避免不了要用到Python。而此前都是用C++写的计算机视觉算法，但是多会一门语言多一条路，不是坏事。所以仔细研究了OpenCV与Python相关的文档，由于我自己先前装的是Anaconda(为方便使用numpy, scipy, pandas, jupyter等库)，所以默认的Python解释器为Python3.6，之前用的OpenCV的版本是3.1，当然3.2也有下。我查阅了官方的Python和OpenCV的教程，知道OpenCV3.x才支持Python。而官方教程说的是OpenCV3.x配合Python2.x才能使用。这就很尴尬了，我想要最新的Pythonv版本(Python3.6)调用最新的OpenCV版本(OpenCV3.2)，官方没有解决方案。那怎么办，既然选择了最新，就免不了折腾一番，只能混迹于Google和StackOverflow，所以才有了这篇博客，给遇到同样问题，同样想法的读者参考。

## 依赖项安装
首先综述一下，我们的安装环境以及将要安装的库包，做好心理准备。以我的安装过程为例，**win10 64bit**环境，已经安装好的IDE为**Pycharm**，已经解压好的**OpenCV3.2.0**..
需要安装的工具为**Anaconda3 4.3.1(64bit)** ，某个神秘的**轮子**。

### 安装Anaconda
安装**Anaconda**的原因是我想用**jupyter notebook**来学习别人的程序，顺便也算是用用新工具尝尝新。**jupyter notebook**查了下资料，不得了啊，说是做**Data Science**必须要会的，而且好多开源项目用**jupyter notebook**展示，所以这个工具必须学。这种风靡程度让我想到了**Github Pages**，底层是**Jekyll**，我的博客也是基于这个构建起来的，感谢**Jekyll**,让我有了写作平台，给读者分享知识。
官方推荐安装**jupyter notebook**的推荐安装方式是安装**Anaconda**，**Anaconda**是集成了**numpy**,**Scipy**,**Matplotlib**,**Pandas**等一些科学计算工具的集成包，并且内置了**jupyter notebook**，**spider**,**IPython**等好用的工具和对应的**Python解释器**。
其中**Anaconda3**代表是**Python3的解释器**,**Anaconda2**代表的是**Python2的解释器**，由于未来肯定是属于Python3的，所以我选用了**Anaconda3**,对应的解释器为**Python3.6**。
把**Anaconda3**安装好后，**OpenCV3**需要的**Python**和**numpy**环境就安装好了。

### 深入看下Anaconda
**Anaconda**确实是个好东西，如果机器性能足够，可以在此基础上安装Tensorflow和Caffe跑深度学习。下面看下安装好的OpenCV3+Python配置的在Anaconda下的情况。
**C:\ProgramData\Anaconda3\Lib\site-packages**下有**Anaconda**附带的一些好用的包，比如pandas,numpy等。
![dic.png](http://FrancisGee.github.io/assets/images/dic.png)

## 安装神秘的轮子
这是最关键的一步，因为[官方教程](http://docs.opencv.org/trunk/d5/de5/tutorial_py_setup_in_windows.html)只提供了**Python2.7**的支持，所以这里要借助于**神秘的轮子**,在此之前我仔细调研了下**OpenCV3.2**的发布时间为2016年12月23日，**OpenCV3.1**的发布时间为2015年12月18日，最新的**OpenCV2.4.13**的发布时间为2016年12月16日，**说明OpenCV2.x还在不断更新**。
**Python3.6.1**的发布时间为2017年3月21日,**Python2.7.13**的发布时间为2016年12月17日，由此可见我们的轮子要找到对付Python3.x + OpenCV3.x组合的配置。
说了这么多，现在介绍轮子，先给出[轮子的地址](http://www.lfd.uci.edu/~gohlke/pythonlibs/#opencv)，这个轮子是由**欧文加利福尼亚大学**的**荧光动力学实验室**编写的，感谢轮子的提供者。
下面开始选择合适的轮子,本教程的环境选择的轮子为**opencv_python‑3.2.0‑cp36‑cp36m‑win_amd64.whl**，解读下文件名，**opencv_python‑3.2.0**代表用的OpenCV的版本为OpenCV3.2，**cp36‑cp36m**代表用的Python的版本为Python3.6，**win_amd64**代表64 bit Windows系统。
下载好轮子后,打开**cmd**，运行**pip install opencv_python‑3.2.0‑cp36‑cp36m‑win_amd64.whl**命令，就安装好了**Python3.x + OpenCV3.x**组合环境。
## 验证是否成功安装
在Python命令行或者Pycharm中执行以下代码，显示3.2.0则说明安装成功。


```python

	import cv2

	print(cv2.__version__)

```

## 参考

+ [Install OpenCV 3 with Python 3 on Windows](https://www.solarianprogrammer.com/2016/09/17/install-opencv-3-with-python-3-on-windows/)
+ [神秘的轮子](http://www.lfd.uci.edu/~gohlke/pythonlibs/#opencv)
+ [OpenCV Release](http://opencv.org/releases.html)










