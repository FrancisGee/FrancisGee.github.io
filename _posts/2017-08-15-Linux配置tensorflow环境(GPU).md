---

layout: post
title: Linux配置tensorflow环境(GPU版)
date: 2017-08-15
tag: [configuration]

---

目录

* TOC 
{:toc}

>有一个月没有更新博客，主要是因为休假了一段时间，又没有学啥东西，所以没什么好东西可以分享给读者，这里首先向读者表示道歉．

这次分享的主要内容为在Ubuntu环境下配置深度学习的环境(GPU版本)，由于项目的需要这个环境，所以入了一台新电脑配置环境．配置环境的过程中踩了很多坑，相信配过环境的读者应该都有体会．

网上相关的配置过程也比较多，我主要分享我配置环境中的主要步骤和关键以及对出现问题的简要分析．这篇博客有几个目的: 一来是给读者配深度学习环境的参考，二来是以后出了问题可以再次装好环境，三来是随着知识储备的增加，可以更好的分析以前的问题．

## 硬件环境和软件包

**硬件环境**: *NVIDIA 1050 ti*(因为穷), *CPU i7*, *8G内存*

**软件包**: *Ubuntu 16.04 LTS*, *NVIDIA驱动程序*，*CUDA 8.0*, *cuDNN 5.1*, *Anaconda 4.2.0(Python 3.5)*

一定要注意版本，一定要注意版本，一定要注意版本．

这几环环环相扣，后面还将说明重要性．

## 安装Ubuntu 16.04 LTS系统
由于Tensorflow官网支持的Ubuntu系统只有**16.04**，所以要下载这个系统的镜像，另外注意千万不能下载Ubuntu Kylin 16.04，否则在装显卡驱动的时候会出现问题，注意一定要用原生的Ubuntu系统．

由于现在的电脑都已经用**UEFI启动模式**了，而不是老电脑的Legacy模式，所以这里用UEFI模式安装比较简单　(Ubuntu支持UEFI启动)．其中怎么制作启动盘和怎么划分空闲空间比较简单，这里就不阐述了，有兴趣可以看我提供的参考链接．

这里有几个需要注意的地方，在进行Ｕ盘安装时，要先**关闭WINDOWS的快速启动(Fast Boot)和Security Boot**,这两项都可以在BIOS中设置(我的电脑是按F2)，然后选择从U盘启动电脑(我的电脑是按F12)。

接下来就是安装Ubuntu和分区操作，这里需要注意的是**新建efi系统分区(没有原先的/boot)**，**安装引导启动器的设备选择efi系统分区所在的磁盘**。

最后成功安装系统后，拔掉Ｕ盘重启，默认是会进入Ubuntu系统的．如果要进入Windows系统，则需要开机时按F12选择Windows Boot Manager.

## 安装NVIDIA显卡驱动
安装显卡驱动上面的坑是最多的，我卡在上面花的时间最多．安装CUDA工具包的时候是包含驱动的，但是最好将**驱动分开装**．

安装NVIDIA显卡驱动前要先**关闭Security Boot**(据说不关闭Security Boot，Ubuntu 16.04无法使用第三方驱动)以及禁用nouveau驱动(开源的第三方驱动)．

安装驱动的流程为：进入命令行终端(文字模式:Ctrl + ALt + F1)--->禁用lightdm(sudo service lightdm stop)---->安装驱动---->启动lightdm(sudo service lightdm start)--->重启

### 棘手问题:安装好驱动后循环登录

解决的方案是在安装驱动(注意参数)时，执行 sudo ./NVIDIA-Linux-x86_64-375.20.run -no-x-check -no-nouveau-check -no-opengl-files 

-no-x-check 安装驱动时关闭Ｘ服务
-no-nouveau-check　安装驱动时禁用nouveau
-no-opengl-files　只安装驱动文件，不安装OpenGL文件

猜想循环登录的原因：我们安装的ubuntu显示器并没有接到nvidia的显卡上，而是使用了intel的集显．我们安装的驱动其实是想将我们运算用的显卡驱动更新，结果都给搞了．也有可能是opengl产生的冲突．

### 验证是否安装成功

输入命令　nvidia-smi，有输出信息则成功．


## 安装CUDA

这里安装CUDA的版本为**8.0**，里面包含了我们要的CUDA 8.0 Toolkit和CUDA 8.0 Samples，其他低版本的CUDA就不要碰了．．．

这里装CUDA也有一些注意的点，下载CUDA文件格式为**runfile**的，不要用deb包，否则不可控，因为我们并不需要再装驱动．安装CUDA还是在命令行终端(文字模式:Ctrl + ALt + F1)下安装，按照提示一步一步来，不过在选择是否装驱动的时候一定选择no.直到CUDA安装完成．

我安装CUDA是一次通过的，不过显示在CUDA 8.0 Samples里缺一些lib,比如libGLU等．

### 出现问题:nvcc --version无效

输入nvcc --version显示没安装CUDA，但是编译CUDA 8.0 Samples中的程序，没有任何Error．并且运行./device*程序显示信息则安装成功．
然后配置环境变量，这步非常重要，不配环境变量系统不知道CUDA是否安装．

问题分析是PATH路径写入不对，官网的教程是写入/etc/profile，但是Ubuntu 16.04要写入~/bashrc..写入~/bashrc后，输入命令　nvcc --version 显示CUDA信息．


### 验证是否安装成功

编译CUDA 8.0 Samples中的程序，没有任何Error．并且运行./device*程序显示信息则安装成功．

输入命令　nvcc --version 显示CUDA信息．

## 安装cudnn

安装cudnn比较简单，其实就是把cudnn的头文件和库加到cuda中来．不过还是有注意的细节，由于tensorflow的官网是支持**cudnn 5.1版本**，所以只能下载**cudnn 5.1 for linux(amd64架构)**.

### 出现问题:cudnn无法调用
import tensorflow as tf显示import error啥的，最后执行命令　sudo ldconfig /usr/local/cuda/lib64

问题分析：在装cuda的时候已经执行过sudo ldconfig，但是最开始下错cudnn版本，所以换新版本的cudnn要重新执行命令．


## 安装Anaconda

上面的一大通文字都是为配置GPU环境做准备，现在将要接触tensorflow了．

Anaconda的好处在前面的博客中已经提到过了，不过这里还是有细节需要注意．由于(tensorflow目前只支持Python 3.5和Python 2.7)(截至到2017.08.15)，所以我这里安装的是**Anaconda 4.2.0**(附带Python 3.5)..

### 出现的问题：无法执行 xx.sh

解决方案: bash xx.sh

## 安装tensorflow

安装tensorflow有四种方案可以选择，这里选用的是Anaconda安装方案．

安装方法这里可以完全照着官网来，不过选Python Package选Python 3.5版本支持gpu的tensorflow包，安装GPU版本的tensorflow**不需要先安装CPU版本的**，如果安装了CPU版本的，请先卸载CPU版本的tensorflow..

### 出现的问题：cudnn无法调用

上文已解决

## 使用Tensorflow

照着官网的教程在jupyter notebook上实现了MINST手写字符识别任务，识别准确率从单层的softmax regression的92%提高到三层cnn的99.2的准确率，确实深度学习在cv这块做的非常出色．

另外官网上说的，训练上述网络需要大约30min的时间，我用**装好的gpu的tensorflow运行只用了10min**，确实算力有很大提升．

但是这个并没有达到手写数字识别(MNIST)的目前最高准确率，[目前最高的准确率为99.79%](https://rodrigob.github.io/are_we_there_yet/build/classification_datasets_results.html#4d4e495354).

## 总结

配环境肯定是以**官网**为主，但是每个人的电脑以及系统千差万别，所以要查阅资料，解决问题．

另外要在庞杂的英文信息中找到有用的信息对我而言也是挑战，可以参考SO上的方法，**点赞数最多**，加强英语的学习．

配置环境中出现的问题是令人沮丧的，解决问题是很愉快的．．但是要分析问题的原因，这样才能提升，并且解决问题．．

当然有些分析不一定正确，随着知识增加会越来越接近真相．．另外有所错误和纰漏，希望读者指正．．


## 附录

**所用到的文件全名**：

NVIDIA驱动：NVIDIA-Linux-x86_64-375.66.run

CUDA ToolKit: cuda_8.0.61_375.26_linux.run

cudnn: cudnn-8.0-linux-x64-v5.1.tgz

Anaconda: Anaconda3-4.2.0-Linux-x86_64.sh

Tensorflow: https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.2.1-cp35-cp35m-linux_x86_64.whl




**下载传送门**：本周结束之前附上，欢迎点击．

## 参考

+ [UEFI模式安裝Ubuntu 16.04參考](http://blog.csdn.net/ysy950803/article/details/52643737)
+ [配置深度学习环境主要参考](http://www.jianshu.com/p/5b708817f5d8?from=groupmessage)
+ [禁用nouveau驱动参考](http://blog.csdn.net/10km/article/details/61191230) 
+ [解决循环登录问题](http://blog.csdn.net/u010159842/article/details/54344683)
+ [安装cuda和cudnn环境参考](http://blog.csdn.net/xuezhisdc/article/details/48651003)
+ [cudnn官网](https://developer.nvidia.com/cudnn)
+ [安装Anaconda问题解决](https://askubuntu.com/questions/843253/running-simple-bash-script-fails-with-syntax-error-word-unexpected-expecting)
+ [tensorflow官网指导安装Ubuntu环境](https://www.tensorflow.org/install/install_linux)
+ [cudnn无法调用问题解决](https://github.com/tensorflow/tensorflow/issues/7522)
+ [MNIST数据集排行榜](https://rodrigob.github.io/are_we_there_yet/build/classification_datasets_results.html#4d4e495354)

















