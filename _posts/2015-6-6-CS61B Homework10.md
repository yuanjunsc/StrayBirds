---
layout: post
title: CS61B Homework10
category: 公开课
comments: true
---

CS61B Homework10

作业链接：
http://www.cs.berkeley.edu/~jrs/61b/hw/hw10/




一、题目要求：

对一组八进制数进行排序，使用radix sort和counting sort的算法。

[ 60013879 11111119 2c735010 2c732010 7fffffff 4001387c 10111119 529a7385 1e635010 28905879 11119 0 7c725010 1e630010 111111e5 61feed0c 3bba7387 52953fdb 40013879 ]






二、解题思路：



对整个数组进行radix排序，在每次radix sort中使用counting sort排序。

radix sort的注意事项:

1.从低位到高位排序：

高位比低位的影响力大，所以要留到最后进行排序。


2.稳定排序：

待排序相同元素之间的相对前后关系，在各次排序中不会改变。所以在进行每一次时，前一次的排序结果都能够保留。


3.分离数位：

分离出的数位要能够代表原元素，并且使用分离出的数位再进行counting sort。在本题中对十六进制数排序会比较简单些，因为用位操作比较方便。所有数据类型在机器中都是使用二进制来存储的，所以不用在意数据类型(java的int类型是32位，使用如下语句来进行操作：

x & 15  提取出x的后四位二进制数（编译器会自动转换为整数），接下来使用移位运算符将x进行移位，x >> 4从而提取出之后各位。

couting sort的注意事项：

1.counting sort without item:

例如对这些数排序 0 0 1 3 3 3 5 6 7 7
这种情况就和bucket sort几乎差不多了。使用count数组来记录各元素出现的次数，并在最后依次输出，需要注意的是，count.length 取决于元素的最大取值范围，不要和元素的个数搞混了。


2.counting sort with item:	先列出讲义中给的参考程序

输入x数组如下：


![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework10/countingsort_x.png)


按题意中的理解x[i]为item，由key和value组成：


Begin by counting the keys in x.


 	for (i = 0; i < x.length; i++) {

 		counts[x[i].key]++;

 	}


 我们要做的就是通过对x[i]的key进行排序，从而最后能实现对x[i]的value的排序，而难点在于item。在进行到这一步时，我们有的条件如下：

 原始输入的数组：keys[60013879 11111119 2c735010 2c732010 7fffffff 4001387c......]

 及相对应分离后的元素数组：key[9 9 0 0 f c......]



 (1)hashtable: 最初想到的办法，直接把key[i]作为key,keys[i]作为value,put进hashtable中，但是key[]中是有很多重复元素的，而hashtable中的key值不允许重复。后来又想把keys[i]作为key，key[i]作为value传入，这样就避免了重复的问题，但是必须得引入arraylist才能进行sort，后来懒得弄了，有时间再补充上。



 (2)数组：这个是讲义中的思路，但是我一开始没有搞懂x[i]的key和value这种数据结构是怎么对应上的，后来才发现是自己搞复杂了：


 首先对key[]进行coutingsort:


 	for(int l = 0; l < key.length; l++) {                 //统计出key[]中个元素的数量，待排元素的各位数

		count[key[l]]++;

	}

接下来用scan[]数组，表示小于i的数量（scan[i]）,这样就知道了有多少的元素是小于key[i]的。

	int total = 0;

		int c =0;

		for(int k = 0; k < count.length; k++) {

			c = count[k];

			scan[k] = total;

			total = total + c;

		}



最后进行输出：


	for(int p = 0; p < keys.length; p++) {

		result[scan[key[p]]] = keys[p];

		scan[key[p]]++;

	}




key[p]得到keys[p]所对应元素待排序位数，scan[key[p]]确定这个待排序位数应该输出的位置，也就是最后key[p]要输出的位置。最后scan[key[p]]++，是保证下次的key[p]相同时，插入到下一个位置。

