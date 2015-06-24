---
layout: post
title: CS61B Homework6
category: 公开课
comments: true
---

CS61B Homework6




作业链接：
http://www.cs.berkeley.edu/~jrs/61b/hw/hw6/




一、题目要求：



实现一个hashtable,且以8*8的SimpleBoard矩阵（矩阵每个节点有0,1,2这三种取值可能）作为其key值。







二、基本概念：

1.load factor = 存入数据的数量 / 哈希表容量

2.collision : key1 != key2，但是 f(key1) = f(key2)

3.除留余数法（本题所用）：f(key) = key / N (N为hashtable的大小)

4.链地址法（本题所用）：hashtable的每个节点都被设置为一个单链表。






三、解题思路：


1.题目分析：


hashtable中要存放8 * 8组成的SimpleBoard类：

	public class SimpleBoard { 

		private final static int DIMENSION = 8;

  		private int[][] grid;

  	}


hashtable中的每个数据类型设为Entry:
	public class Entry {

  		protected Object key;		//SimpleBoard

  		protected Object value;

  	}


因为我们要使用的是链地址法，对于key值相同的entry，都应该存入一个单链表中、所以，我们可以直接将hashtable中的每个存储单元都设为一个单链表。



从而得到hashtable的结构类型如下：


	public class HashTableChained implements Dictionary {
	
		protected double load_factor;	//	n / N

  		public int N;   // length of the table

  		protected int n;    // number of entries in the dictionary

  		protected int collision;

  		DList[] bucket = new DList[167];	//hashtable的存储空间

  	}


所以，接下来的问题就是：

根据每次SimpleBoard类的grid[][]数组作为key值，生成不同的hashcode值，从而存入到bucket[]链表中。




2.生成hashcode:


这部分的难点是如何将8 * 8的矩阵转换为整数值（java的int类型为32位，即-2147483648 到 2147483647）。而每一位有三种可能，所以一共可能会生成3的64次方种int值，我们在此使用强制转换，所以当超出范围时就会发生collision。

	public int hashcode(){

	 	double val = 0;

	  	for (int x = 0; x < 8; x++) {

	      	for (int y = 0; y < 8; y++) {

	    	  val = val + this.grid[x][y] * Math.pow(3, 8*x+y);

	      	}

	    }

	  	return (int) val;

  	}


3.compFunction：将hashcode值（-2147483648 到 2147483647）转换为hashcode空间的存储范围（0 到 N - 1）：

	int compFunction(int code) {

    	// Replace the following line with your solution.
	 
    	return code % this.N;

  	}


4.插入算法：


	public Entry insert(Object key, Object value) {

    	// Replace the following line with your solution.

    	//return null;

		Entry entry = new Entry();

		entry.key = key;

		entry.value = value;

		int code = key.hashCode();

		int i = compFunction(code);

		bucket[i].insertBack(entry);


		//bucket[compFunction(key.hashCode())].insertBack(entry);

		return entry;
	
  }



5.统计collision的数量：


对于任意一个bucket[i]，只要其长度大于1，就说明存在相同的hashcode值：

所以我们对bucket[i]顺序遍历：


	for(int i = 0; i < N; i++){

		if(bucket[i].length() > 1){

					sum++;

		}

	}

	System.out.println("Actual number of collisions: " + sum);






四、程序结果：


![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework6/Homework6-1.png)



![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework6/Homework6-2.png)