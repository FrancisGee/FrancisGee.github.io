---
layout: post
title:  双系统下Linux上github解决方案
date: 2016-08-20
tags: linux
---

目录

* TOC 
{:toc}

问题由来：笔者有些项目要上传到Linux系统，而之前已经给Win8.1配好了Github上的SSH Keys,而由于系统是双系统，所以github上配好Linux的SSH Keys后，使用git clone命令发现上传项目失败，**publicity denied**的Error.


> 在Github上可以以repo为最小单位设置SSH Keys

## 解决方案(采用Hostname方法)：

现在假设我们在Github已经有两个repo:-`git@github.com:username/foo.git`和`git@github.com:username/bar.git`,这两个repo我们都希望能够在双系统上的Linux进行*git clone*,*git push* ,*git pull*等一系列操作

### 创建多组SSH Keys

首先，我们为我们的每个repo创建各自的SSH Keys
```shell
$ ssh-keygen -f ~/.ssh/id_rsa.github.username.foo
$ ssh-keygen -f ~/.ssh/id_rsa.github.username.bar
```

### 在Github上配置SSH Keys

在Github上的对应repo上配置各自的SSH Keys, 可以copy/paste对应的**public keys**.（例如可以在`~/.ssh/id_rsa.github.username.foo.pub`中找到）

现在，我们就已经把每个Github上的repo和本地绑定了

### 在SSH Config上配置Hosts

创建`~/.ssh/config`,如果你已经有此文件，在文件中配置如下

```shell
//account for the foo repo`

Host github.com-foo

    HostName github.com

    User username

    IdentitiesOnly yes

    IdentityFile ~/.ssh/id_rsa.github.username.foo
	
//account for the bar repo`

Host github.com-bar

    HostName github.com

    User username

    IdentitiesOnly yes

    IdentityFile ~/.ssh/id_rsa.github.username.bar
```
	
**注意**:你的Git URL从`git@github.com:username/foo`变为`git@github.com-foo:username/foo.git`,明白这点，大功告成

### 更改远程库

如果之前的本地库已联系到远程库(特别是文件直接拷贝过来容易出现)，这时如果需要更改远程库，则有以下操作

```shell
//view existing remote

get remote -v

//change remote

git remote set-url origin git@github.com-foo:username/foo.git`

//verify the changed remote

git remote -v
```

## 参考

* [Using Multiple SSH Keys with Github](http://www.freshblurbs.com/blog/2013/06/22/github-multiple-ssh-keys.html)
* [Frequently Operation in Git and Github](http://www.waynechu.cn/git%20&%20github/2016/07/26/Linux%E4%B8%8BGit%E5%92%8CGitHub%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.html)

--------------------------------------------------------------------
*Copyright 2016 , FrancisGeek*

*Last updated:  Sun  Aug 21  EDT 2016*

