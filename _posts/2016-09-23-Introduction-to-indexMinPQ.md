---
layout: post
title: 索引优先队列介绍
date: 2016-09-23
tags: algorithms
---

目录

* TOC 
{:toc}


>本文建立在已经熟悉优先队列的基础上

## 为什么要用索引优先队列

优先队列应用的场景有**任务调度**问题（模拟系统），实现**Dijkstra算法**，实现**堆排序等**。。但有时候需要允许用例引用*已经进入队列中的元素*，为了做到这一点，需要给每个元素一个索引，所以解决**多向归并问题**的索引优先队列应运而生。

## 实现及用例

**多向归并问题**将多个有序的输入流归并成一个有序的输出流，常用场景为输入可能来自多种*科学仪器的输出*（按时间排序），或是来自*多个音乐或者电影网站的信息列表*（按名称或艺术家名字排序）等等

### 实现

```java
package priority_queuse;

import java.util.Iterator;
import java.util.NoSuchElementException;

import edu.princeton.cs.algs4.StdOut;

public class IndexMinPQ<Key extends Comparable<Key>> implements Iterable<Integer> {
	private int maxN;                                                                                               //maximum number of elements on PQ
	private int n;												 	//number of elements on PQ
	private int[] pq;											        //binary heap using 1-based indexing
	private int[] qp;											        //inverse of pq---qp[pq[i]]=pq[qp[i]]=i
    private Key[] keys;										                        //keys[i]=piority of i
    
     public IndexMinPQ(int maxN){
    	 if (maxN < 0) throw new IllegalArgumentException();
         this.maxN = maxN;
         n=0;
    	 keys=(Key[]) new Comparable[maxN+1];
    	 pq=new int[maxN+1];
    	 qp=new int[maxN+1];
    	 for(int i=0;i<=maxN;i++)
    		 qp[i]=-1;
     }
     
     public boolean isEmpty(){
    	 return n==0;
     }

     public boolean contains(int k){
    	 if (k < 0 || k >= maxN) throw new IndexOutOfBoundsException();
    	 return qp[k]!=-1; 	 
     }
     
     public int size(){
    	 return n;
     }
     
     public void insert(int i,Key key){
    	 if (i < 0 || i >= maxN) throw new IndexOutOfBoundsException();
    	 if(contains(i)) throw new IllegalArgumentException("index is already in the priority queue");
    	 n++;
    	 qp[i]=n;
    	 pq[n]=i;
    	 keys[i]=key;
    	 swim(n);
     }
     
     public int minIndex(){
    	 if(n==0) throw new NoSuchElementException("Piority queue underflow");
    	 return pq[1];
     }
     
     public Key minKey(){
    	 if(n==0) throw new NoSuchElementException("Piority queue underflow");
    	 return keys[pq[1]];
     }
     
     public int delMin(){
    	 if(n==0) throw new NoSuchElementException("Piority queue underflow");
    	 int min=pq[1];
    	 exch(1,n--);
    	 sink(1);
    	 assert min == pq[n+1];
    	 qp[min]=-1;;                                     //delete
    	 keys[min]=null;                                  //to help with garbage collection
    	 pq[n+1]=-1;;
    	 return min;
    	 
     }
    	public void changeKey(int i,Key key){
    		if (i < 0 || i >= maxN) throw new IndexOutOfBoundsException();
    		if(!contains(i)) throw new NoSuchElementException("index is not in the piority queue");
    		keys[i]=key;
    		swim(qp[i]);
    		sink(qp[i]);
    	}
     
    	public void delete(int i){
    		if (i < 0 || i >= maxN) throw new IndexOutOfBoundsException();
    		if(!contains(i)) throw new NoSuchElementException("index is not in the priority queue");
    		int index=qp[i];
    		exch(index,n--);
    		swim(index);
    		sink(index);
    		keys[i]=null;
    		qp[i]=-1; 		
    	}
    	
    	/******************************************************8
    	 * General helper functions
    	 * @param args
    	 */
    	private boolean greater(int i,int j){
    		return keys[pq[i]].compareTo(keys[pq[j]])>0;
    	}
    	
    	private void exch(int i,int j){
    		int swap=pq[i];
    		pq[i]=pq[j];
    		pq[j]=swap;
    		qp[pq[i]]=i;
    		qp[pq[j]]=j;
    	}
    	
    	/********************************
    	 * Heap helper functions
    	 * 
    	 * @param args
    	 */
    	private void swim(int k){
    		while(k>1&&greater(k/2,k)){
    			exch(k/2,k);
    			k=k/2;
    		} 		
    	}
    	private void sink(int k){
    		while(2*k<n){
    			int j=2*k;
    			if(j<n&&greater(j,j+1)) j++;
    			if(!greater(k,j)) break;
    			exch(k,j);
    			k=j;
    		}
    	}
    	
    	@Override
		public Iterator<Integer> iterator() {
			// TODO Auto-generated method stub
			return new HeapIterator();
		}
    	private class HeapIterator implements Iterator<Integer>{

    		//create a new pq
    		private IndexMinPQ<Key> copy;
    		
    		//add all elements to copy of heap
    		//takes linear time since already in heap order so no keys move
    		public HeapIterator() {
				copy=new IndexMinPQ<Key>(pq.length-1);
				for(int i=1;i<=n;i++)
					copy.insert(pq[i], keys[pq[i]]);
			}
			@Override
			public boolean hasNext() {
				// TODO Auto-generated method stub
				return !copy.isEmpty();
			}

			@Override
			public Integer next() {
				// TODO Auto-generated method stub
				if(!hasNext()) {throw new NoSuchElementException();}
				return copy.delMin();
			}

			@Override
			public void remove() {
				// TODO Auto-generated method stub
				throw new UnsupportedOperationException();
			}
    		
    	}
    	
    	//Unit Test
    	
	public static void main(String[] args) {
		//insert a bunch of strings
		String[] strings = { "it", "was", "the", "best", "of", "times", "it", "was", "the", "worst" };
		
		IndexMinPQ<String> pq=new IndexMinPQ<String>(strings.length);
		for(int i=0;i<strings.length;i++){
			pq.insert(i, strings[i]);
             }
		
		//print each key using iterator
		for(int i:pq){
			StdOut.println(i+" "+strings[i]);
		}
		
		StdOut.println();

	}
 }
```


 **分析**：使用pq[]保存索引，keys[]保存元素，qp[]数组保存pq[]的逆序（类似反函数）。上沉和下浮操作同优先队列一样，用来维持堆有序。并且为索引优先队列实现了泛型和迭代接口，用来支持任意数据类型和使用Java的foreach语句通过迭代遍历并处理集合中的每个元素。

 
### 用例
 
```java
package testcase;

import edu.princeton.cs.algs4.In;

import edu.princeton.cs.algs4.StdOut;
import priority_queuse.IndexMinPQ;

public class Multiway {
	//this class should not be instantiated
	private Multiway() {}
	
 //merge together the sorted input streams and write the sorted results to standard output
	private static void merge(In[] streams){
		int n=streams.length;
		IndexMinPQ<String> pq=new IndexMinPQ<String>(n);
		for(int i=0;i<n;i++)
			if(!streams[i].isEmpty())
				pq.insert(i, streams[i].readString());
		
		//Extract and print min  and read next from its stream
		while(!pq.isEmpty()){
			StdOut.println(pq.minKey()+" ");
			int i=pq.delMin();
			if(!streams[i].isEmpty())
				pq.insert(i, streams[i].readString());
		}
		StdOut.println();
	}
	

    /**
     *  Reads sorted text files specified as command-line arguments;
     *  merges them together into a sorted output; and writes
     *  the results to standard output.
     *  Note: this client does not check that the input files are sorted.
     *
     * @param args the command-line arguments
     */
	

	public static void main(String[] args) {
		int n=args.length;
		In[] streams=new In[n];
	
		for(int i=0;i<n;i++)
			streams[i]=new In(args[i]);
		merge(streams);
	}

}
```

*Input*:

`m1.txt:A B C F G I I Z` 

 `m2.txt:B D H P Q Q`  

`m3.txt:A B E F J N`

*Output*: `A A B B B C D E F F G H I I J N P Q Q Z`(此处为列输出，节约版面横向写出)


**分析**:一共输入3个输入流，创建了长度为3的索引优先队列，每个输入流的索引(i取值总为0,1,2)都关联着一个元素（输入中的下个字符串）。初始化之后，代码进入一个循环，删除并打印出队列中最小的字符串，然后将该输入的下一个字符串添加一个元素，直到队列为空，多向归并完成。


## 参考文献以及代码

* [When should I use priority queue](http://stackoverflow.com/questions/18049847/when-would-i-use-a-priority-queue)
* [Priority Queue Wiki](https://en.wikipedia.org/wiki/Priority_queue)
* [Source Code](https://github.com/FrancisGee/coding_marathon/tree/master/reference_to_2016.09.23_Blog)


 


