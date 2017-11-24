---

layout: post
title:  Python和Numpy精华版
date: 2017-11-24
tags: [Language]


---

目录

* TOC 
{:toc}


## 引言

最近花了一些时间重温了Python和Numpy的知识，为什么要重温呢?没有需求就没有动力，因为最近尝试用不同的语言写算法, 并且很多库(Scipy, TF, Pytorch, Pandas, scikit-learn)都是用Numpy写的，
在编写程序以及看别人代码的过程中，发现自己掌握的不够好，所以重新温习了Python和Numpy.

##　Python篇

### 观其大略

一直有人说need-driven的方式学习是最好的，在不断践行的过程中，确实能够体会这一点。比如用Python用来刷算法题。

因为是从Java这样一类语言转过来的，所以自然有很多东西不会，不会不要紧，还有狗哥可以查。但是一种语言的发明肯定是解决特定环境下的问题效果更好。

要利用Python语言肯定要在和其他语言对比的基础上来利用其特有的性质。

Python这门语言我个人认为比较妖气，不太好掌握。Python共有四种独特的built-in数据类型, list(用来取代数组), dict(用来取代哈希表), set(用来取代集合), tuple(元组，让代码更concise).

### 记住反常

在理解了上述基本概念之后，应该要学会Python独有的特点，这样让自己更好利用Python写出好的程序，并且更容易读懂别人的程序。

+ Slicing(切片):

但凡听过Python的人应该也听过切片，切片是一种针对list的技术，**通过切片很容易拿到sublist(子数组)**.

EX:

a[start:end] # items start through end-1
a[start:]    # items start through the rest of the array
a[:end]      # items from the beginning through end-1
a[:]         # a copy of the whole array

a[start:end:step] # start through not past end, by step
默认step为1, 所以没有写出。

Tips:
最容易让人感到困惑的就是**切片中的负数**。

a[-1]    # last item in the array
a[-2:]   # last two items in the array
a[:-2]   # everything except the last two item

负数可以理解为从list的末端开始考虑，但是注意a[:-2]显然是一种正难则反的思想


+ enumerate函数在list的Loop中的使用，内置的enumerate函数可以拿到list的索引, 访问元组。

```python

    animals = ['cat', 'dog', 'monkey']
    for idx, animal in enumerate(animals):
        print('#%d: %s' % (idx + 1, animal))
    # Prints "#1: cat", "#2: dog", "#3: monkey", each on its own line

```

+ list comprehension(我没有找到合适的翻译), 通过list comprehension让构造list的操作更加简洁

```python 

    nums = [0, 1, 2, 3, 4]
    squares = []
    for x in nums:
        squares.append(x ** 2)
    print(squares)   # Prints [0, 1, 4, 9, 16]
    
    
    
    nums = [0, 1, 2, 3, 4]
    squares = [x ** 2 for x in nums]
    print(squares)   # Prints [0, 1, 4, 9, 16]
    
    
    nums = [0, 1, 2, 3, 4]
    even_squares = [x ** 2 for x in nums if x % 2 == 0]
    print(even_squares)  # Prints "[0, 4, 16]"

```

+ Dic comprehension　AND Set comprehension


```python

    nums = [0, 1, 2, 3, 4]
    even_num_to_square = {x: x ** 2 for x in nums if x % 2 == 0}
    print(even_num_to_square)  # Prints "{0: 0, 2: 4, 4: 16}"
    
    
    from math import sqrt
    nums = {int(sqrt(x)) for x in range(30)}
    print(nums)  # Prints "{0, 1, 2, 3, 4, 5}"


```

+ Tuple(元组)的特殊性

元组的很多行为和list一样，但有一点很特殊，元组可以作为字典中的键，或者作为集合中的元素，但是list不可以。

```python

    d = {(x, x + 1): x for x in range(10)}  # Create a dictionary with tuple keys
    t = (5, 6)        # Create a tuple
    print(type(t))    # Prints "<class 'tuple'>"
    print(d[t])       # Prints "5"
    print(d[(1, 2)])  # Prints "1"

```


## Numpy篇

在理解了Python的特性之后，再来研究Numpy.

### 观其大略

Numpy的发明主要是应对科学计算，说的大白话一点就是处理矩阵, 张量等数据结构。所以其基本数据类型为ndarrays.

对于Numpy其实把其当做开源的Matlab用就好。

### 记住反常

+ Numpy的索引机制

Numpy中共有三种索引机制: filed access, basic slicing, advanced slicing

前面三种比较好理解，这里我主要讲Advanced slicing, 因为这个最不好理解。

Advanced slicing分为两类: Integer array indexing 和　Boolean array 

**Integer array indexing**:

还是举个例子来说明


```python

    import numpy as np

    a = np.array([[1,2], [3, 4], [5, 6]])

    # An example of integer array indexing.
    # The returned array will have shape (3,) and
    print(a[[0, 1, 2], [0, 1, 0]])  # Prints "[1 4 5]"

    # The above example of integer array indexing is equivalent to this:
    print(np.array([a[0, 0], a[1, 1], a[2, 0]]))  # Prints "[1 4 5]"

    # When using integer array indexing, you can reuse the same
    # element from the source array:
    print(a[[0, 0], [1, 1]])  # Prints "[2 2]"

    # Equivalent to the previous integer array indexing example
    print(np.array([a[0, 1], a[0, 1]]))  # Prints "[2 2]"



```

上述索引中的[0, 1, 2], [0, 1, 0]]要拆开来看，分为[0, 0], [1, 1], [2, 0]


**Boolean array indexing**

这个比较好理解，可以理解为为矩阵的每个slot填充bool值, 还是举个例子来说明

```python

    import numpy as np

    a = np.array([[1,2], [3, 4], [5, 6]])

    bool_idx = (a > 2)   # Find the elements of a that are bigger than 2;
                         # this returns a numpy array of Booleans of the same
                         # shape as a, where each slot of bool_idx tells
                         # whether that element of a is > 2.

    print(bool_idx)      # Prints "[[False False]
                         #          [ True  True]
                         #          [ True  True]]"

    # We use boolean array indexing to construct a rank 1 array
    # consisting of the elements of a corresponding to the True values
    # of bool_idx
    print(a[bool_idx])  # Prints "[3 4 5 6]"

    # We can do all of the above in a single concise statement:
    print(a[a > 2])     # Prints "[3 4 5 6]"

```



+ axis(轴)的说明

axis = 0表示垂直向下操作，　axis = 1表示水平方向操作

+ Broardcasting(广播)

广播是一个非常重要的性质，前面的博客谈到过，这里就不展开了。



## 总结

这篇博客只是对Python和Numpy基础的一些痛点进行了介绍，更多的详细的内容还是要借助文档和狗哥。

学一种东西，最好的方式就是去用。所以我希望读者能够用Python去撸算法，用numpy去生撸机器学习算法。



## 参考


+ [cs231n对Python和Numpy的介绍](http://cs231n.github.io/python-numpy-tutorial/#python-lists)
+ [关于切片的最好解释-SO](https://stackoverflow.com/questions/509211/understanding-pythons-slice-notation)
+ [Numpy轴(axis)的说明](https://docs.scipy.org/doc/numpy-1.13.0/glossary.html)
+ [Numpy中的索引(indexing)操作文档](https://docs.scipy.org/doc/numpy-1.13.0/reference/arrays.indexing.html)
+ [Numpy中的广播(broadcasting)文档](https://docs.scipy.org/doc/numpy/user/basics.broadcasting.html)



