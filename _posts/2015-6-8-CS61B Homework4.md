---
layout: post
title: CS61B Homework4
category: 公开课
comments: true
---

CS61B Homework4




作业链接：
http://www.cs.berkeley.edu/~jrs/61b/hw/hw4/





一、题目要求：



1.实现一个双向链表，头作为哨兵节点（没有item，空的双向链表头节点指向自己），注意要插入一个新的节点时调用newNode()方法。




2.实现一个 "lockable" 的双向链表，每一个节点都能够进行上锁。

（1）定义一个 LockDListNode 类继承自DListNode，并且包含有是否上锁的信息。（注意调用DListNode的构造器）

（2）定义一个继承自DList的LockDList类，并且包含有一个新的方法：

	 public void lockNode(DListNode node) { ... }


通过它来实现对各节点的上锁操作。





二、基本概念：




1.DListNode数据结构：


	public class DListNode {

		public Object item;

 		protected DListNode prev;

  		protected DListNode next;


  		//DListNode() constructor


  		DListNode(Object i, DListNode p, DListNode n) {

    		item = i;

    		prev = p;

    		next = n;

  		}

	}



2.DList数据结构：


	public class DList {

		protected DListNode head;

  		protected int size;


  		//使用这个方法来创建新的DListNode，这样的话如果DList类的子类想要创建新类型的节点，只要覆盖这个方法就可以了。


  		protected DListNode newNode(Object item, DListNode prev, DListNode next) {

    		return new DListNode(item, prev, next);

  		}

  		public DList() {

	  		head = newNode(null,null,null);		//哨兵节点

	 		head.next = head.prev = head;

	  		size = 0;


	  		//简单列举几个method:


	  		public void insertFront(Object item) {
    
	  			DListNode current = newNode(item,head,head.next);

	  			current.next.prev = current;

	  			current.prev.next = current;

	  			size++;

  			}

  			public void insertBack(Object item) {


  			}


  			public void insertBefore(Object item, DListNode node) {
    
	  			if(node != null){

		  			DListNode current = newNode(item,node.prev,node);

		  			node.prev = current;

		  			current.prev.next = current;
		  
	 			}

  			}

  			public void insertAfter(Object item, DListNode node) {


  			}

 	 	}

  	}




三、解题思路：


1.实现DLisNode数据结构：


	class LockDListNode extends DListNode{

    	boolean lock;

    	LockDListNode(Object i, DListNode p, DListNode n){

            super(i, p, n);				//调用父类的构造函数，参数类型必须相同，父类的构造器不能被继承，但必须被子类构造器调用！

            lock = false;				//默认不上锁

    	}
	}


2.实现LockDList数据结构：

	class LockDList extends DList{

   		protected DListNode newNode(Object item, DListNode prev, DListNode next) {
    		
    		//实现了方法覆盖，将LockDListNode转换为DListNode返回
            
        	return new LockDListNode(item,prev,next);


    	}


    	public void lockNode(DListNode node){

            LockDListNode tempNode = (LockDListNode)node;

            tempNode.lock = true;
    	
    	
    	}



    	public void remove(DListNode node) {

            if (((LockDListNode)node).lock!=true) {

                if (node!=null) {
                          node.prev.next = node.next;
                          node.next.prev = node.prev;
                          node.prev = null;
                          node.next = null;
                          size--;      
                }
            }        
   		}
	}



这部分是本体中的重点和难点，涉及到了继承和多态，再来看下这个方法：

	protected DListNode newNode(Object item, DListNode prev, DListNode next) {
    		
    		//实现了方法覆盖，将LockDListNode转换为DListNode返回
            
        	return new LockDListNode(item,prev,next);


    	}


因为 LockDList extends DList，所以此处实现了对于newNode方法的重写，

在返回时LockDLitNode完成向上转型变为DListNode：

所以，在LockDlist生成的节点虽然都被转换成DListNode，但本质仍然是LockDLitNode，

其他DList的方法可以完全继承而不用重写，而lockNode() 和 remove() 这两个方法在其中都会使用

LockDLitNode的lock属性，所以在方法内部使用时将其强制转换成LockDListNode即可。


java的多态复习：

多态的定义（选择多种运行状态）：

引用变量所指的具体类型，和通过该引用变量发出的方法调用在编程时不确定，必须在运行时确定。


发生条件：

继承，向上转型，重写！

发生向上转型时，只能使用父类的方法和属性。只有在子类重写相关方法后才可以调用。





四、程序结果：


![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework4/Homework4-1.png)



![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework4/Homework4-2.png)
