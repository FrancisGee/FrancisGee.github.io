---

layout: post
title:  Ensemble_Stacking技术
date: 2017-11-30
tags: [Algorithms]


---

目录

* TOC 
{:toc}


## Motivation

做过机器学习的朋友都知道，Ensemble技术非常重要。Ensemble技术是个什么意思呢？我们知道对于一个分类问题，有很多分类器(比如KNN, LR, Xgboost)都可以进行分类，但是每个机器学习的算法都有其优缺点，
可以理解为一份数学卷子，有人擅长做里面的立体几何，有人擅长做里面的解析几何，有人擅长做数列题。而Ensemble技术就是将不同分类器的预测能力来一个综合，最后得到的分类器比单个分类器要好。

那么Ensemble的种类其实是非常多的，本博客仅仅用Stacking举例，因为理论上Stacking是可以学习出各种函数(Voting, Bagging等Ensemble技术),并且Stacking在比赛中效果最好，所以在本博客给读者
将Stacking原理。

## Stacking Generalization

目前的Stacking技术用的最多但是**Stacking Generalization**, 当然还有最新的Stacking技术，比如StackNet.
为了便于描述算法原理，这里只用一层Stack.

Stacking技术中有些术语先做说明，base model(sub-model)为基本分类器，如前所述(KNN, LR, Xgboost).meta-model(aggregate model)为元模型，也就是在基本分类器训练得到的基础上进行
训练的模型，为了方便，这里选用逻辑斯特回归。

具体怎么操作呢？

为了模型的泛化能力，这里还要引入K-fold验证方式。首先把训练集(train-set)分成K份，这里假设有２个分类器(base model)，用其中的K-1个作为训练集, 对最后一个训练集，用基本分类器进行分类，得到
训练结果M1, M2.最后得到所有训练集的结果M1, M2.然后用M1, M2, Target训练meta-model训练得到meta-model模型。
对测试集采用同样的方式，用基本分类器拿到M1, M2.然后用M1, M2给meta-model进行预测得到目标值。

## Tips

Stacking技术厉害之处在于综合各种模型的优点，没有模型是perfect的。
但是这些模型之间最好相似度尽可能低，比如多种树模型的Stacking就不一定比树模型，线性模型, NN模型的效果要好。

## Conclusion

本来这篇博客配图应该方便理解，但是由于时间关系不方便这样做。所以在参考页面给了读者资源，便于自己查阅。我这篇博客的主要目的是把Stacking技术的精华告诉读者。

## References

+ [How to Implement Stacked Generalization From Scratch With Python](https://machinelearningmastery.com/implementing-stacking-scratch-python/)
+ [Ensemble learning-Wiki](https://en.wikipedia.org/wiki/Ensemble_learning#Stacking)
+ [KAGGLE ENSEMBLING GUIDE](https://mlwave.com/kaggle-ensembling-guide/)
+ [Stacking (stacked generalization)-Github上某个开源方案](https://github.com/ikki407/stacking)
+ [StackNet](https://github.com/kaz-Anova/StackNet)


