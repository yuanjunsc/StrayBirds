---
layout: post
title: Dynamic Programming I
category: 刷题
comments: true
---



NineChapter lesson5




一、Triangle的例子：




![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson5/Triangle.png)



1.DFS:

(1)Traverse：O(2^n)

(2)Divide & Conquer: O(2^n)


时间复杂度相同是因为要对三角形中每一个依次进行遍历，而且点又有左右两个节点，即每次都有两种路径


2.Divide & Conquer + Memorization：


![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson5/memory.png)


(1)设定一个hash二维数组，初始化全部元素为-1

(2)储存此次的路径和

(3)返回路径和

(4)下次先进行查表工作，如果hash中对应位置已经有值，直接返回

时间复杂度为 O(n^2)， 因为这样保证了每个点只访问一次，所以时间复杂度等价于点的个数



二、动态规划


本质：动态规划就是 *解决了重复计算* 的搜索




1.动态规划的实现方式：

(1) 记忆化搜索：耗费空间较大，思维难度小

(2) 循环：更加正规



2.编程方式：

(1)自底向上---底层先有值，
(2)自顶向下(建议使用此种方法)---顶层先有值，f[i][j]表示从(0,0)走到(i,j)的最短距离




3.什么情况可能考到动态规划？

(1)Maximum/Minimum

(2)Yes/No

(3)Count(*)




4.什么情况下不是动态规划？

(1)如果题目需要求出所有“具体”的方案而非方案“个数”


(2)输入数据是一个“集合”而不是“序列”



5.动态规划的四点要素

(1)状态：

f[i][j]/f[i] 是什么东西？


(2)方程：

由前边的小状态如何得到大状态？


(3)初始化


(4)答案




6.动态规划最常见的四种考题类型：

(1) Matrix DP (15%)

(2) Sequence (40%)

(3) Two Sequences DP (40%)

(4) Others (5%)







二、Lintcode例题(简单易懂题只写四要素)



1.minPathSum        最小路径和


给定一个只含非负整数的m*n网格，找到一条从左上角到右下角的可以使数字和最小的路径。






题目分析：


(1)处理二维数组的问题最好先初始化第一行和第一列，这样可以保证点存在

(2)四要素：

state:  f[x][y] 表示从起点走到(x,y)的最小路径和

function:   (x-1,y)和(x,y-1)可以走到这点，所以 f[i][j] = Math.min(f[i-1][j],f[i][j-1]) + grid[i][j]

intialize:  第一行和第一列

answer:     f[m-1][n-1]





2.uniquePaths---不同的路径


有一个机器人的位于一个M×N个网格左上角（下图中标记为'Start'）。

机器人每一时刻只能向下或者向右移动一步。机器人试图达到网格的右下角（下图中标记为'Finish'）。
问有多少条不同的路径？



题目分析：

(1)同样先进行初始化，第一行和第一列的点都只有一条路能到达，所以全部初始化为1

(2)四要素

state：  f[x][y]，从起点走到(x,y)的路径数

function:   f[i][j] = f[i-1][j] + f[i][j-1];

intialize:  第一行和第一列

answer:     f[m-1][n-1]




3.uniquePaths2---不同的路径2

不同点，原地图中位置为1表示障碍，0为通路，在初始化和修改状态时判断即可






4.Climbing Stairs
    

假设你正在爬楼梯，需要n步你才能到达顶部。但每次你只能爬一步或者两步，你能有多少种不同的方法爬到楼顶部？


题目分析：

(1)求出走到第n层有几种方式

(2)四要素

state:  f[i],走到第i层有这几种方式

function:   i层，可以有i-1层和i-2层走到，所以 f[i] = f[i-2] + f[i-1]

intialize:  f[0] = 1, f[1] = 2

answer: f[n-1]




5.canJump---跳跃游戏


给出一个非负整数数组，你最初定位在数组的第一个位置。　　　

数组中的每个元素代表你在那个位置可以跳跃的最大长度。　　　　

判断你是否能到达数组的最后一个位置。



题目分析：

state:  f[i],   能否跳到第i个位置 

function:   f[j] == true && j + A[j] >= i，即j位置可以跳到，并且从j位置可以跳到i位置

intialize:  f[0] == true

answer:     f[n-1]





6.canJump2---跳跃游戏2

    
条件和上题相同，但是求跳到终点的最少步数.


四要素：

state:  f[i]，跳到第i个位置需要的步数

function:   f[i] = Math.min(f[i], f[j] + 1);   j<i && A[j]+j>=i,在所有满足于这个条件的点中求出最小值

intialize:  因为求最大值，所以每次比较时，先将f[i]设为Integer.Max

answer: f[n-1]





7.maxSubArray

给定一个整数数组，找到一个具有最大和的子数组，返回其最大和。


题目分析：

一开始被这道题卡了很久，因为状态一直找不对，后来得到大神的指点才发现是如此的简单啊！



f ( i ） 定义为 "以 i 结尾的子数组可能得到的最大和"。这样，状态方程就是

f ( i ) = max ( f (i - 1 ) + A( i ) , A ( i ))


state:  f[i], 以 i 结尾的子数组可能得到的最大和

function:   f[i] = Math.max(f[i-1] + A[i],A[i])

intialize:  f[0] = A[0]

answer: max





8.word break---单词切分


给出一个字符串s和一个词典，判断字符串s是否可以被空格切分成一个或多个出现在字典中的单词。


题目分析：

(1)这道题一开始我对题目的理解有些错误，应该是s切分后，其两部分必须都是由dict中单词构成

(2)处理这种单sequence问题时，可以设成f[s.length+1]

state:  f[i]前i个字符能被切分

function：  f[i] = OR(f[j] == true && j-i是dict中的单词)

intialize:  f[0] = true

answer:    f[s.length]




9.minCut---分割回文串


给定一个字符串s，将s分割成一些子串，使每个子串都是回文。

返回s符合要求的的最少分割次数。



题目分析：

(1)同样是单sequence问题，所以设f[s.length+1]

(2)四要素


state： f[i]前i个字符组成的回文串，分割次数最少

function： f[i] = Math.min(f[i], f[j] + 1)   
j < i && j+1 - i是回文串

intialize:  求最小值，所以f[i]都设为max

answer: f[s.length]






10.Longest Increasing Subsequence---最长上升子序列




给定一个整数序列，找到最长上升子序列（LIS），返回LIS的长度。


挑战：要求时间复杂度为O(n^2) 或者O(nlogn)

给出[5,4,1,2,3]，这个LIS是[1,2,3]，返回 3

给出[4,2,4,5,3,7]，这个LIS是[4,4,5,7]，返回 4





题目分析：

(1)首先看到挑战要求复杂度为O(n^2)能想到DP，因为使用DP可以把DFS的时间复杂度O(2^n)降为n^2

(2)接下来是本题最重要的state设置问题，一开始也是没有想好，看了讲义，

这道题也是应该将f[i]设置为以i结尾所构成的最长上升子序列，和最大、小序列和相同


(3)四要素：

state: f[i]表示前i个数字中以第i个结尾的LIS的长度

function:   f[i] = MAX{f[j]+1}, j < i && a[j] <= a[i])

j要满足的条件是，j在i之前，并且a[j] <= a[i],这样才能把i添加入LIS中

然后在当前f[i]满足的所有f[j]中，选择最大的f[j]，并将长度加1

最后，在所有的f[i]中选择最大的值返回即为所求

intialize:  因为求每次的最大值，所以一开始可以将f[i]中都初始化为1

answer: max(f[0..n-1])





三、总结：

1.先列出四要素

2.注意处理单sequence问题时，将状态数组设为f[len+1]，方便处理：

即单词长度枚举，切分位置枚举

3.记住最长上升子序列、最小数组和，最大数组和function的写法：

f[i]为以i即为以i结尾的什么什么