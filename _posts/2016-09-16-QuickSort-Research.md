---
layout: post
title: 快排学习研究
date: 2016-09-16
tags: algorithms
---

目录

* TOC 
{:toc}

众所周知，快速排序是一类非常经典的算法，作为计算机科学学习者，我们必须将其熟练掌握。

## 基本实现

```java
public class Quick
{
 public static void sort(Comparable[] a)
 {
    StdRandom.shuffle(a);   //random the input
    sort(a,0,a.length-1);
 }

 private static void sort(Comparable[] a,int lo,int hi)
 {
    if(hi<=lo) return;
    int j=parttion(a,lo,hi);
    sort(a,lo,j-1);
    sort(a,j+1,hi);
 }

 private static int parttion(Comparable[] a,int lo,int hi)
 {
   //parttion the array into a[lo..i-1],a[i],a[i+1...hi]
   int i=lo,j=hi+1;
   Comparable v=a[lo];
   while(true)
    {
       while(less(a[++i],v))   if(i==hi) break;
       while(less(v,a[--j]))   if(j==lo) break;
       if(i>=j)                          break;
       exch(a,i,j);
    }
    exch(a,lo,j);
 }

}
/*************************************
**********Helpful functions***********
**************************************/
private static boolean less(Comparable v,Comparable w)
{ return v.compareTo(w)<0;}

private static void exch(Comparable[] a,int i,int j)
{
  Comparable t=a[i];
   a[i]=a[j];
   a[j]=t;
}
```

### 优化方案

*优化基本思路1*：对于小数组，快速排序比插入排序慢。。

将`if(hi<=lo) return`替换为`if(hi<=lo+M) {Insertion.sort(a,lo,hi); return;}`

**M**值根据个人系统做出调整，取值在5-15之间

*优化基本思路2*：在切分元素上下手，采用子数组的一小部分元素的中位数来切分数组，最常用的切分策略为3取样切分

```java
//return the index of the median element amomg a[i],a[j] and a[k]
private static int median3(Comparable[] a,int i,int j ,int k)
{
        return (less(a[i], a[j]) ?
               (less(a[j], a[k]) ? j : less(a[i], a[k]) ? k : i) :
               (less(a[k], a[j]) ? j : less(a[k], a[i]) ? k : i));
}

//add some statements to private sort 
private static void sort(Comparable[] a ,int lo ,int hi)
 {

  int n=hi-lo+1;
  if(n<CUTOFF)
  { insertionSort(a,lo,hi); return;}
  int m=median3(a,lo,lo+n/2,hi)
  exch(a,m,lo);
  int j=parttion(a,lo,hi);

 }
```


### 特殊问题

`荷兰国旗`（只有R,W,B三种样值）问题，具有大量重复元素的数组，在现实生活中非常常见，对此采用三向切分，无需对重复元素排序。

```java
public class Quick3way
{
 private static void sort(Comparable[] a,int lo ,int hi)
{
  if(hi<=lo) return;
  int lt=lo,i=lo+1,gt=hi;
  Comparable v=a[lo];
  while(i<=gt)
  {
    int cmp=a[i].compareTo(v);
    if             (cmp<0) exch(a,lt++,i++);
    else if        (cmp>0) exch(a,i,gt--);
    else                   i++;
   }
   //now a[lo..lt-1]<v=a[lt..gt]<a[gt+1..hi]
   sort(a,lo,gt-1);
   sort(a,gt+1,hi);
}


}
```

## Q&&A
* 如果代码有问题或者需要帮助，请联系我的邮箱
* 以上代码均为Java编写，其他语言也可以根据思路实现
* 任何算法总有改进的余地，请不要浅尝辄止，欢迎相互交流



