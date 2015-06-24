---
layout: post
title: CS61B Homework7
category: 公开课
comments: true
---

CS61B Homework7




作业链接：
http://www.cs.berkeley.edu/~jrs/61b/hw/hw7/





一、题目要求：



实现2-3-4树的插入算法，使用讲义中提过的top-down插入算法。



二、基本概念：


多路查找树：
每一个节点的孩子数可以多于两个，且每一个节点还可以存储多个元素。


2-3-4树：

1.节点最大为4节点，包含4个孩子或没有孩子，且节点具有三个元素。


2.是平衡树，即每一节的左右子树高度最多差1。


3.top-down：插入和删除操作从root节点开始查找，在叶子节点完成。


4.B树：一种平衡的多路查找树，2-3和2-3-4树都是B树的特例。




三、解题思路：



1.Tree234Node的数据结构（部分由本作业工程中提供）：



	class Tree234Node {

		int keys;	//该节点的元素总数

  		int key1;

  		int key2;

  		int key3;

  		Tree234Node parent;

  		Tree234Node child1;

  		Tree234Node child2;

  		Tree234Node child3;

  		Tree234Node child4;

  	}





2.Tree234的数据结构：


	public class Tree234 {

		Tree234Node root;

		protected int size;

		public boolean find(int key) {      //return true if "key" is in the tree; false otherwise.

			Tree234Node node = root;

    		while (node != null) {

      			if (key < node.key1) {

       				node = node.child1;

      			} 

      			else if (key == node.key1) {

        			return true;

      			} 

      
     			else if ((node.keys == 1) || (key < node.key2)) {

        			node = node.child2;

      			} 

      			else if (key == node.key2) {

        			return true;

      			} 

      			else if ((node.keys == 2) || (key < node.key3)) {

        			node = node.child3;

      			} 

     		 	else if (key == node.key3) {

        			return true;

      			} 

     			else {

        			node = node.child4;

      			}
    		}
    		return false;
  		}


  		public void insert(int key) {		//insert() inserts the key "key" into this 2-3-4 tree



  		}



  	}



3.实现insert()插入方法：


(1).从root节点开始遍历


(2).只要遇到有三个key的节点时，就将此节点中间的key弹出至其parent节点，剩余的左右节点则分别成为parent节点的左右孩子。



(3).注意节点的引用是双向的。记得修改。








4.这道题的代码复用用的太多了，暂时就不贴程序了。