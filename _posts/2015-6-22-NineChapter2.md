---
layout: post
title: Binary Search
category: 刷题
comments: true
---



NineChapter lesson2



Classical Binary Search:


Find the any/first/last position of target in nums, return -1 if target doesn’t exist.


![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson2/DoSearch.png)



P.S.   算法面 试中如果需要 优化O(n)的 时间复 杂度 那么只能是O(logn)的二分法.





一、模板总结(四要素)：


1. start + 1 < end : 

两指针不相邻时进行循环


2. mid = start + (end - start) / 2 ： 

mid 取值的设定，考虑溢出情况


3. A[mid] ==, <, >

判断范围

    if(A[mid] == target){
        end = mid;
    }
    else if(A[mid] > target){
        start = mid;
    }
    else{
        end = mid;
    }


4. A[start] A[end] ? target

先判断start还是end---决定是寻找first position 还是 last position:

    if(A[start] == target){
        return start;
    }

    if(A[end] == target){
        return end;
    }


如果是求any position的话，当A[mid] == target时，直接return






二、Lintcode例题



1.SearchRange        搜索区间

链接：http://www.lintcode.com/zh-cn/problem/search-for-a-range/



![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson2/SearchRange.png)





题目分析：


(1)数组为排序数组，元素可以重复：

(2)寻找target的range范围即找到target在数组中的 first position & last position.

(3)first position 和 last position 程序大体相同，只不过在 A[mid] == target时：

first:  end = mid

last:   start = mid
    


算法思路：

先用二分法找到target的 first position 作为 bound[0]，没有直接返回[-1,-1]
再用二分法找到target的 last position 作为 bound[1], 返回bound.


    


2.Search Insert Position        


链接：http://www.lintcode.com/zh-cn/problem/search-insert-position/


![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson2/Search Insert Position .png)




题目分析：

(1)数组为排序数组，无重复元素

(2)如果在数组中直接找到target，则直接return A[mid]

(3)否则就证明数组中不存在target，但此时的start和end已经是最逼近target的值了：

判断start和end哪个更接近target，因为数组是递增顺序：

因此先判断start：

    if (A[start] >= target) {
            return start;
        } 
    else if (A[end] >= target) {
            return end;
        } 
    else {
            return end + 1;
        }




算法思路：

用二分法查找target，因为无重复元素，所以找到后直接return即可（相当于找any position)，
没找到时，用start和end逼近，看哪个更接近。











3.Search a 2D Matrix           搜索二维矩阵


链接：http://www.lintcode.com/zh-cn/problem/search-a-2d-matrix/


![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson2/SearchMatrix.png)





题目分析：




(1)每一行元素都是递增顺序，且下一行元素都大于上一行

(2)O(log(n) + log(m)) 时间复杂度：

一定是使用了两次二分法来实现，先二分找出所在行，再在该行中用二分找出所在列





4.Search a 2D Matrix II          搜索二维矩阵


链接：http://www.lintcode.com/zh-cn/problem/search-a-2d-matrix-ii/




![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson2/SearchMatrix2.png)




题目分析：

(1)各行的元素都为递增顺序，各列的元素也都是递增顺序（无重复元素）

(2)选取矩阵的左下角为起点进行搜索

(3)算法步骤：

在每一行中都进行二分法查找：

如果该行中有target,则计数加一，跳到target上一行中进行搜索

如果该行没有target，则在start和end中选择接近target的数，并跳到相应的上一行进行搜索



如下图所示：


![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson2/Search.png)







程序：

    public static int searchMatrix(int[][] matrix, int target) {

        if(matrix.length == 0){
            return 0;
        }

        int num = 0;
        int x,y;
        int x1 = matrix.length-1;
        int y1 = 0;
        
        int start = y1;
        int end = matrix[x1].length-1;
        int mid;
        int flag = 0;
        
        while(x1 >=0 && y1 <= matrix[x1].length-1){
            end = matrix[x1].length-1;
            
            while(start + 1 < end && x1>=0){

                mid = start + (end - start) / 2;
                
                if(matrix[x1][mid] == target){
                    num++;
                    x1--;
                    y1 = mid;
                    flag = 1;
                    continue;
                }
                
                else if(matrix[x1][mid] < target){
                    start = mid;
                }
                else{
                    end = mid;
                }
                
            }
            
            
            if(flag == 1){
                flag = 0;
                continue;
            }
            
            
            if(matrix[x1][start] == target){
                num++;
                x1--;
                y1 = start;
                
            }
            
            else if(matrix[x1][start] > target){
                x1--;
                y1 = start;
            }
            
            else if(matrix[x1][end] == target){
                num++;
                x1--;
                y1 = end;
                
            }
            else{
                x1--;
                y1 = end;
            }

        }
        
        return num;
    }





5.Find Peak Element     找出一组数据元素中的任意一个峰值点


链接：http://www.lintcode.com/zh-cn/problem/find-peak-element/


![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson2/SearchPeak.png)




题目分析：


(1)峰值的特点就是该点的值大于前一个点和后一个点

(2)三个点间的曲线有四种情况：递增，递减，波峰，波谷

(3)用start和end区间去逼近峰值点，最后返回这两点中较大的那个



程序：

    public int findPeak(int[] A) {
     
        int start = 1, end = A.length-2; // 1.答案在之间，2.不会出界 

        while(start + 1 <  end) {
            int mid = (start + end) / 2;
            if(A[mid] < A[mid - 1]) {
                end = mid;
            } else if(A[mid] < A[mid + 1]) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if(A[start] < A[end]) {
            return end;
        } else { 
            return start;
        }
        
        
    }




6.Search in Rotated Sorted Array     在旋转数组中进行搜索


链接：http://www.lintcode.com/zh-cn/problem/search-in-rotated-sorted-array/


![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson2/xuanzhuan.png)




题目分析：

(1)数组中无重复元素，且最初为排序数组

(2)我是把旋转后的数组大致看为前后断开分别递增的两部分，所以和普通二分法查找不同的是：

在确定区间时，不能直接拿A[i]和target进行比较，需要拿区间端点进行辅助比较，

确定不同的单调区间：




程序：


    public int search(int[] A, int target) {
        int start = 0;
        int end = A.length - 1;
        int mid;
        
        while (start + 1 < end) {
            mid = start + (end - start) / 2;
            if (A[mid] == target) {         //无重复元素，所以直接找到any position返回即可
                return mid;
            }
            if (A[start] < A[mid]) {        //确定单调区间
                // situation 1, red line
                if (A[start] <= target && target <= A[mid]) {
                    end = mid;
                } else {
                    start = mid;
                }
            } else {
                // situation 2, green line
                if (A[mid] <= target && target <= A[end]) {
                    start = mid;
                } else {
                    end = mid;
                }
            }
        } // while
        
        if (A[start] == target) {
            return start;
        }
        if (A[end] == target) {
            return end;
        }
        return -1;
    }



三、总结


1.Binary Search Template---4 key points

见上文


2.Rotated Sorted Array---三类问题

(1)Find Minimum---只考虑A[mid]大小和区间端点大小关系即可，两种情况

(2)Find Target---需要考虑A[mid]

(3)why o(n) with duplicates ---有疑问！！！


3.Find Median in Two Sorted Array find kth --- 太难了，还不会！！！




