---
layout: post
title: 初探红黑树
date: 2016-10-16
tag: algorithms
---

目录
* TOC 
{:toc}

>First, we learn concepts faster by studying examples. As kids, we didn’t learn languages by understanding the grammar first. We learned by copying others around us; we learned by studying examples.
>Second, everybody starts at zero, but if we learn something new every day it is possible to acquire working knowledge of almost anything in a very short period of time. Knowledge builds on itself; it is governed by the rule of compounding. The key is to learn something new every single day.
>Third, passion is a powerful force. It is hugely rewarding to follow your passion and get better at things you are interested in.

**红黑树**是将**2-3树**的平衡性优点与**二叉查找树**的代码进行复用，从而实现对于最坏情况下的插入和查找操作均能达到对数级别，虽然**红黑树**好用，但是理解起来可不是那么容易。

## 红黑树的定义

![红黑树基本结构.png](/images/posts/RBT/RedBlackTree.png)

注意：因为每个结点都会有一条指向自己的链接（从它的父结点指向它），我们将链接的颜色保存在表示结点的Node数据类型的变量color中  ，如果指向它的链接是红色的，那么该变量为true，黑色则为false，约定空链接为黑色。当提到一个**结点的颜色**时，指的是**指向该结点链接的颜色**。

红黑二叉查找树的基本思想是用**标准的二叉查找树**（完全由2-结点构成）和一些额外的信息(替换3-结点)来表示2-3树。

树中的链接分为两种类型：**红链接**将两个2-结点连接起来构成一个3-结点（3-结点表示为一条左斜的红色链接相连的两个2-结点），**黑链接**则是2-3树中的普通链接

对于任意的2-3树都可以派生出对应的红黑二叉树

红黑树满足如下条件：

* 红链接均为左链接
* 没有任何一个结点同时和两条红链接相连
* 该树是*完美黑色平衡*的，即任意空链接到根结点的路径上的黑链接数量相同



## 红黑树的基础操作

![红黑树基本操作.png](/images/posts/RBT/BasicOperation.png)

红黑树虽然性能优良，但是其结构容易遭到破坏，所以红黑树有三种基本操作来保证红黑树的结构（有序性）以及完美平衡性，这三种操作分别是*左旋*操作，*右旋*操作，*颜色转换*操作。

### 左旋操作

在实现某些操作的过程中可能会出现红色的右链接或者两条连续的红链接，这个时候就要用到旋转操作了。

假设现在有一条红色的右链接需要被转换为左链接--左旋操作，左旋操作对应的方法接受一条指向红黑树的某个结点的链接作为参数，被指向结点的右链接是红色的,这个方法接受一条指向红黑树的某个结点的链接作为参数，该方法对树进行必要的调整，并且返回一个指向同一组键的子树，且左链接为红色的根结点的链接。


```java

private Node rotateLeft(Node h)
       {
		assert(h!=null)&&isRed(h.right);
		Node x=h.right;
		h.right=x.left;
		x.left=h;
		x.color=h.color;
		h.color=RED;
		return x;
       }

```
这个操作可以理解为：将用两个键中的较小者作为根结点变为将较大者作为根结点

### 右旋操作

右旋操作与左旋类似，只需要将left和right互换即可

```java

private Node rotateRight(Node h)
       {
		assert(h!=null)&&isRed(h.left);
		Node x=h.left;
		h.left=x.right;
		x.right=h;
		x.color=h.color;
		h.color=RED;
		return  x;
	}

```

### 颜色转换操作

颜色转换操作用来处理**一个结点的两个红色子结点的颜色**，除了将子结点的颜色由红变黑外，还要同时将父结点由黑变红，和旋转操作一样是局部变换，不会影响整棵树的**黑色平衡性**。

```java

private void flipColors(Node h)
       {
		assert !isRed(h)&&isRed(h.left)&&isRed(h.right);
		h.color=RED;
		h.left.color=BLACK;
		h.right.color=BLACK;
	}

```

## 红黑树的插入(put)和查找(get)操作

由于红黑树是基于二叉查找树的，所以二叉查找树的get()方法可以直接拿来用，但是插入操作后会破坏红黑树的结构，所以要用红黑树的基本操作来进行处理来恢复红黑树的结构。

![红黑树结构调整.png](/images/posts/RBT/TransferRedLink.png)

### 插入操作

*所有情况*：

* 向单个2-结点中插入新键
* 向树底部的2-结点插入新键
* 向一颗双键树（即一个3-结点）中插入新键
* 向树底部的3-结点插入新键

*归纳总结*：

* 如果右子结点是红色的而左子结点是黑色的，进行左旋转
* 如果左子结点是红色的并且它的左子结点也是红色的，进行右旋转
* 如果左右子结点均为红色，进行颜色转换


```java

public void put(Key key,Value val){
		root=put(root,key,val);
		root.color=BLACK;
		
	}
	private Node put(Node h,Key key,Value val){
		if(h==null){
			n++;
			return new Node(key,val,RED);
		}
		
		int cmp=key.compareTo(h.key);
		if    (cmp<0) h.left=put(h.left,key,val);
		else if  (cmp>0) h.right=put(h.right,key,val);
		else             h.val=val;
		
		//fix-up any right-leaning links
		if(isRed(h.right)&&!isRed(h.left))   h=rotateLeft(h);
		if(isRed(h.left)&&isRed(h.left.left)) h=rotateRight(h);
		if(isRed(h.left)&&isRed(h.right))    flipColors(h);
		
		return h;
	}

```
第一条if语句会将任意含有红色右链接的3-结点（或临时的4-结点）向左旋转

第二条if语句会将临时的4-结点中两条连续的红链接中的上层链接向右旋转

第三条if语句会进行颜色转换并将红链接在树中向上传递

### 查找操作

查找操作和二叉查找树类似，可以直接拿来用

```java

public Value get(Key key)
        {
		return get(root,key);
	}
	public Value get(Node x,Key key)
        {
		while(x!=null)
                {
			int cmp=key.compareTo(x.key);
			if      (cmp<0) x=x.left;
			else if (cmp>0) x=x.right;
			else return x.val;
		}
		return null;
	}

```

## 参考

* [2-3 tree](https://en.wikipedia.org/wiki/2%E2%80%933_tree)
* [平衡树](http://algs4.cs.princeton.edu/33balanced/)


















