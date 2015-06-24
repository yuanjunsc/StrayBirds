---
layout: post
title: CS61B Homework5
category: 公开课
comments: true
---

CS61B Homework5




作业链接：
http://www.cs.berkeley.edu/~jrs/61b/hw/hw5/





一、题目要求：


1.完成DList和DListNode classes：

在DList.java，实现 insertFront(), insertBack(), 和 DList() constructor。


在DListNode.java，实现insertAfter(), insertBefore() 和 remove()。



2.实现一个Set数据结构，使用一个List来存储元素。且其按照从小到大的顺序存储非重复元素（Comparable elements）。并实现下列方法：

	public Set()                          // Constructs an empty Set.
  	public int cardinality()              // Number of elements in this Set.
  	public void insert(Comparable c)      // Insert c into this Set.
  	public void union(Set s)              // Assign this = (this union s).
  	public void intersect(Set s)          // Assign this = (this intersect s).
  	public String toString()              // Express this Set as a String.



二、基本概念：DList数据结构：

	public class DList extends List {


		}

（1）如果 o1.compareTo(o2) == 0，那么就认为o1和o2是重复的。

（2）union()和intersect()的运行时间必须与this.cardinality() + s.cardinality()成正比，也就是说不能对s中的每一个元素都遍历一遍this中的元素这样的话运行时间就是 this.cardinality() * s.cardinality()。




三、解题思路：

1.DList数据结构：

	public class DList extends List {

		protected DListNode head;	//头结点

	

		protected DListNode newNode(Object item, DList list, DListNode prev, DListNode next) {

    		return new DListNode(item, list, prev, next);	//生成新节点
 
 		}


 		public DList() {
	  
	  		head = this.newNode(null, null,null, null);		//空双向链表的头节点作为哨兵节点

	  		head.prev = head; 	//head.prev指向末尾元素

	  		head.next = head;	//head.next指向第一个元素

  		}


  		public void insertFront(Object item) {
    
	 		head.next = this.newNode(item,this,head, head.next);

	  		head.next.next.prev = head.next;

	  		this.size++;   

	  	}



	  	public void insertBack(Object item) {
    
	  		head.prev = this.newNode(item, this,head.prev, head);

	  		head.prev.prev.next = head.prev;

	  		this.size++; 
	  
		}



 	}




2.DListNode的数据结构：

	public class DListNode extends ListNode {

		protected DListNode prev;

  		protected DListNode next;

  		DListNode(Object i, DList l, DListNode p, DListNode n) {

    		item = i;

    		myList = l;

    		prev = p;

    		next = n;

  		}


  		public void insertAfter(Object item) throws InvalidNodeException {

  			if (!isValidNode()) {

     	    	throw new InvalidNodeException("insertAfter() called on invalid node");

    		}
    
      		DList str = (DList) this.myList;

      		this.next = str.newNode(item,str,this,this.next);

      		this.next.next.prev=this.next;

  		}


  		public void insertBefore(Object item) throws InvalidNodeException {

    		if (!isValidNode()) {

      			throw new InvalidNodeException("insertBefore() called on invalid node");

    		}
    
    		DList str = (DList) this.myList;

    		this.prev = str.newNode(item, str,this.prev, this);

			this.prev.prev.next = this.prev;
    
  		}

  		public void remove() throws InvalidNodeException {

    		if (!isValidNode()) {

      			throw new InvalidNodeException("remove() called on invalid node");

    		}

    		this.prev.next = this.next;

			this.next.prev = this.prev;

    		myList = null;

    		next = null;

    		prev = null;
  		}



	}



3.Set数据结构实现


	public class Set  {

		List l;			//这样以后DList和SListNode都可以使用

		ListNode head;

		public Set() { 
   
			l= new DList();  	//父类引用指向子类对象

	 		head = l.front();

  		}


  		public int cardinality() throws InvalidNodeException{

	  		int count = 0;
    
	  		ListNode node = l.front();

	  		while(node.isValidNode()) {

		  		node = node.next();

		  		count++;

	  		}

	  		return count;

  		}


  		public void insert(Comparable c) {

  			if(l.isEmpty()){		//如果为空

		  		l.insertFront(c);

		  		return;

	  		}



	  		else{

		  		ListNode curr = l.front();

		  		while(curr.isValidNode()){

			  		try{
				  		if(c.compareTo(l.back().item()) > 0){

					 	l.back().insertAfter(c);

					  	return;

				  		}

				  		if(c.compareTo(curr.item()) == 0) return;

				  		if(c.compareTo(curr.item()) < 0){

					  		((DListNode) curr).insertBefore(c);

					  		return;

				  		}

				  		if(c.compareTo( curr.item()) > 0){

					  		curr= curr.next();
					 
				  		}
				  
			  		}

			  		catch(InvalidNodeException e){

						System.out.println(e);

			  		}
			  
		  		}
	  

  			}

  		}


  		public void union(Set s) {

  			ListNode node1 = this.l.front();		//两个Set的第一个元素

	  		ListNode node2 = s.l.front();

	  		try{


		  		Comparable c1 = (Comparable)(node1.item());		//这句话是重点，将Object强制转换为Comparable：

		  		能进行强制转换的原因是，传入 new Integre(x) 作为 node.item，而Integer对象实现了Comparable接口并继承了Object类。

		  		所以，Integer可以作为item传入ListNode,此时item就具有了使用compareTo的特性，所以可以强制转换成Comparable，然后使用compareTo。



	  		}

	  		catch(InvalidNodeException e){

		  		System.out.println(e);
	  		}
	  
	  
	  		while(node2.isValidNode()){

		 		try{ 

			 		Comparable c1 = (Comparable)(node1.item());

		 	 		while (c1.compareTo(node2.item()) < 0){
			  
			  			try{
			  		
			  				if(node1.next().isValidNode() ){

			  					node1 = node1.next();

			  					c1 = (Comparable)(node1.item());

			  				}

			  				else{
			  			
			  					while(node2.isValidNode()){

			  						node1.insertAfter(node2.item());

			  						node1 = node1.next();

			  						node2 = node2.next();

			  					}

			  				break;

			  			
			  				}
			  		
			  			}

			    		catch(InvalidNodeException e){

			    			System.out.println(e);

			    			break;
			    		}

		  			}

		  			if(!node2.isValidNode())

			  		break;

		  			try{
		  				
		  				if(c1.compareTo(node2.item()) > 0) {
		  			
		  				((DListNode) node1).insertBefore(node2.item());

		  				node2 = node2.next();

		  				}

		  				else{
		  			
		  				node2 = node2.next();

		  				}

		  			}

		  			catch(InvalidNodeException e){

		  				System.out.println(e);

		  			}

				}

		 		catch(InvalidNodeException e){

			 		System.out.println(e);

		 		}

	  		}

  		}


	}



union之所以能实现 this.cardinality() + s.cardinality() 的运行时，就是因为两个Set中的元素都是已经排好序的，所以我们只需依次进行即可，不需要每次都去遍历集合。




四、运行结果：




[_config.yml]({{ site.baseurl }}/images/DataStructure/Homework5/Homework5-1.png)



![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework5/Homework5-2.png)




![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework5/Homework5-3.png)





