---
layout: post
title:  修改Hosts使用Google
date: 2016-11-05
tags:  [tools]

---

目录

* TOC 
{:toc}


>科学上网，从上Google做起

## Linux环境下

版本**Ubuntu**(14.04),Hosts文件在*/etc/hosts*,改为最新的[*hosts*](https://github.com/racaljk/hosts).
注意：你需要切换为**root权限**，在**vi**中操作。

**提示**：vi在命令行模式下输入 *:1,$d*即可删除所有文件，在将新hosts文件Ctrl+A,Ctrl+C,右键粘贴，稍等片刻，就可以看到更改成功,

删除文件前先**备份**操作 **cp /etc/hosts /etc/hosts.backup**

注意：由于Linux无DNS缓存，需要安装**nscd**使其有DNS缓存，如果没有装过，请执行 *sudo apt-get install nscd* ，安装nscd(Name Service Cache Daemon)

改好后启动nscd, *sudo /etc/init.d/nscd restart*,之后HOSTS生效.


###Trouble Shooting

Ubuntu**无法解析主机**, 这个时候在/etc/hosts中添加 127.0.1.0 yourterminalname就解决了。

## Windows环境下

如果按照[传送门](https://github.com/racaljk/hosts)操作后，还是无法访问。在访问链接上的http上加**https**，等到访问成功后，赶紧收藏这个网页，以后就可以方便访问。 

另外推荐好用的[ss](http://my.yizhihongxing.com/aff.php?aff=3488) 


## 彩蛋

有的时候发现修改hosts也不能科学上网了，这个时候就需要**ss+国外服务器**。
其中ss+国外服务器可以自己搭，也可以购买整套服务。
如果需要自己搭的话，服务器推荐[Digital Ocean](https://m.do.co/c/05177e9703e3).

我这里有[推荐码](https://m.do.co/c/05177e9703e3)，可以领取10刀的优惠。

win下有客户端一键安装, ubuntu下配合**sslocal**和**Proxy SwitchSharp**(chrome插件)一起使用。
技术咨询可以给博主发邮件。

## 参考
+ [各种系统下Shadowsocks客户端的安装与配置](http://www.jeyzhang.com/how-to-install-and-setup-shadowsocks-client-in-different-os.html)
+ [服务器端搭建ss](https://kobefanzzf.github.io/)
+ [shadowsocks](https://shadowsocks.org/en/index.html)

