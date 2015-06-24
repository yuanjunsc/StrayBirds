---
layout: post
title: CS61B Homework9
category: 公开课
comments: true
---




CS61B Homework9



作业链接：
http://www.cs.berkeley.edu/~jrs/61b/hw/hw9/





一、题目要求：
使用并查集生成迷宫，使得迷宫中的任意两点间都有且只有一条通路。


6 * 3 的迷宫示意图如下：


![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework9/Maze.png)




墙分为竖墙和横墙两种，竖墙隔开左右两个点，横墙隔离上下两个点。



二、解题思路：


迷宫中的任意两点间都有且只有一条通路 == 整个迷宫是连通的，即迷宫中的所有点都在并查集的一个集合中。






1.初始化迷宫，将所有墙都设置为存在。创建一个并查集（disjoint sets data structure), 将迷宫中的每个点都存入其中：





并查集数据结构的实现：


	public class DisjointSets {
		private int[] array;	//记录各元素的上级是什么，若为负数说明此节点为root，且负数大小为节点数。

		public DisjointSets(int numElements) {    
		               //墙全存在
    		array = new int [numElements];

    		for (int i = 0; i < array.length; i++) {

      			array[i] = -1;

    		}
  		}


  		public void union(int root1, int root2) {   //参数为两个set的root,将小的子树插入到大的子树中

  			 if (array[root2] < array[root1]) {                 // root2 has larger tree

      			array[root2] += array[root1];        // update # of items in root2's tree

      			array[root1] = root2;                              // make root2 new root

    		} else {                                  // root1 has equal or larger tree

      			array[root1] += array[root2];        // update # of items in root1's tree

     			array[root2] = root1;                              // make root1 new root

    		}

  		}


  		public int find(int x) {              //找到x所在set的root

    		if (array[x] < 0) {

      		return x;                         // x is the root of the tree; return it

    	} else {
      							// Find out who the root is; compress path by making the root x's parent.
      		array[x] = find(array[x]);

      		return array[x];                                       // Return the root

    		}
  		}



2.随机选在一个墙，检查它相邻的两个节点是否连通:


(1)如果连通:随机访问下一面墙。



(2)不连通：union这两个节点，再随机访问下一面墙。






3.Maze的算法：



这个体的思路比较简单，依次遍历所有墙，并检查墙两旁的节点是否在同一个集合中即可。




（1）如何随机遍历所有墙（以本题中 6 * 3的迷宫为例）：


horizontal墙数量为12，vertical墙数量为15，总共有27面墙,18个节点。


我的办法是创建一个int[] array数组，将所有墙都存入，并且我对所有的墙进行了一个默认的排序：


horizontal墙的序号为0 - 11，从左到右依次递增。vertical墙的序号为12 - 26，同样从左到右递增。


	int[] array = new int[27];

    for(int v = 0; v < 27; v++){

    	array[v] = v;        

    	System.out.print(array[v] + " ");
    	
    }



数组下标v（0 - 26）表示我们遍历墙时的顺序，而此时array[v] = v 表示我们依次对所有墙进行遍历。所以如果想要随机遍历的话，我们只需要修改array[v]的数字即可，算法如下：



	while(w != 1){
    	random = randInt(w);	//每次得到一个0到w-1的随机数

    	temp = array[w-1];

    	array[w-1] = array[random];	//将此随机数与末尾元素交换

    	array[random] = temp;

    	w--;	当前位置已经交换，进行提前
   	
    }


最后我们就得到了一个随机的array数组，虽然仍是按照0 - 26对数组进行访问，但我们只需按照array[v]的大小来判断为哪种墙即可。


（2）墙算法（省略）：


	for(int f = 0; f < 27; f++){	//对所有墙进行遍历

		if(array[f] <= 11){                         //是横墙壁

			//确定墙的具体坐标；

			//找到墙相邻的两个节点；


			if(set.find(num1) != set.find(num2)){	//判断两个节点是否在一个并查集的集合中

    			set.union(set.find(num1),set.find(num2));	//不在的话就连通这两个集合

    			hWalls[x][y] = false;					//将此墙壁取消
    		}

    		else{

    			break;      //如果已经连通，则直接终止此次的遍历操作即可

    			}


		}


		else{					//是竖墙壁

			//同上

		}


程序结果：


![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework9/FineMaze.png)