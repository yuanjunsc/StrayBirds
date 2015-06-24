---
layout: post
title: CS61B Homework8
category: 公开课
comments: true
---

CS61B Homework8




作业链接：
http://www.cs.berkeley.edu/~jrs/61b/hw/hw8/




一、题目要求：



使用LinkedQueue实现归并排序和快速排序（mergesort和quicksort），最后再比较这两种算法的运行时间。





二、解题思路：


1.LinkedQueue的数据结构（部分由本作业工程中提供）：




	public class LinkedQueue implements Queue {

  		private SListNode head;

  		private SListNode tail;

  		private int size;


  		public void enqueue(Object item) {
  		
  			//enqueue() inserts an object at the end of the Queue.

  		}


  		public Object dequeue() throws QueueEmptyException {

  			//dequeue() removes and returns the object at the front of the Queue.

  		}


  		public Object front() throws QueueEmptyException {

  			//front() returns the object at the front of the Queue.

  		}


  		public Object nth(int n) {

  			//nth() returns the nth item in this LinkedQueue.

  		}


  		public void append(LinkedQueue q) {

  			//append() appends the contents of q onto the end of this LinkedQueue.

  		}

  	}



	class SListNode {                //这就是体现LinkedQueue中LinkedQueue的地方

		Object item;

		SListNode next;

	}


  	class Queue 省略。



2.实现mergesort



（1）mergesort介绍（此处为2路归并排序）:

将含有n个元素初始序列看为n个子序列，每个子序列长度为1，然后两两归并，得到[n/2]个长度为2的子序列，然后再两两归并...直到得到一个长度为n的有序序列。



（2）本题解法：


先将待排序序列（LinkedQueue）转换为N个子序列的序列，例如输入[ 3 5 2 ]，输出[ [ 3 ] [ 5 ] [ 2 ] ]：



——比较简单，从头开始遍历输入序列，依次将每个每个元素出队并直接放入到新生成的队列中。



再编写mergeSortedQueues()方法，可以将两个已经排序好的队列合并成一个：



——比较两个队列的头元素，将较小的元素出队并入队至新的队列，直到有一个队列为空时，将剩下队列的元素全部放入新队列的末尾即可。



最后实现mergesort：



将输入序列化为子序列——依次取出队列中前两个元素——将这两个合并为1个序列——再将此队列入队——直到只剩下一个序列时																																																									
	public static void mergeSort(LinkedQueue q) {
    	
	  LinkedQueue queue = makeQueueOfQueues(q);

	  LinkedQueue q1 = new LinkedQueue();
	  
	  while(queue.size() >  1){
		  
		  try{
			  
			  q1 = mergeSortedQueues((LinkedQueue)queue.dequeue(),(LinkedQueue)queue.dequeue());

			  queue.enqueue(q1);
			 
		  }
		  
		  catch(QueueEmptyException e){

      		System.out.println(e);

      	}
		 
	  }

	  q.append(queue);

  	}																						



	
3.实现quicksort



(1)quciksort介绍：

一趟排序将待排序数组分割成独立的两个部分，其中一部分的关键字都比另一部分的关键字小，再对这两部分分别进行排序。




（2）本题解法：


首先编写一个partition()方法，将待排序数组分为三个部分，大于，等于或小于pivot关键字。


再实现quicksort()方法，选取pivot关键字然后不断对small和big部分进行递归调用。




	public static void quickSort(LinkedQueue q) {

	  	  LinkedQueue sSmall = new LinkedQueue();

	  	  LinkedQueue sEquals = new LinkedQueue();

	  	  LinkedQueue sLarge = new LinkedQueue();
	  	  
	  	  partition(q,(Comparable)q.nth(((int) (q.size() * Math.random()))),sSmall,sEquals,sLarge);
		  
		 
	  	  if(sSmall.size() > 1){

	  		quickSort(sSmall);

	  	  }
		  
	  	  if(sLarge.size() > 1){

	  		quickSort(sLarge);

	  	  }
		 
		  
		  q.append(sSmall);

		  q.append(sEquals);

		  q.append(sLarge);
		  
	  
	}



三、程序结果：


![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework8/Homework8.png)