---
layout: post
title: Subsets and Permutation
category: 刷题
comments: true
---



NineChapter lesson1



一、模板总结：




1.几乎适用于所有搜索问题
2.考虑什么时候输出
3.考虑什么情况跳过





二、Lintcode例题



1.subset        求所给集合的子集

链接：http://www.lintcode.com/zh-cn/problem/subsets/



![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson1/SubSet.png)



题目分析：


(1)画出递归树，以{1,2}为例：



![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson1/DiGuiShu.png)


(2)每个中间时刻list，都是一个答案，所以每次生成新的list就输出



(3)题目要求非降序排列，所以，先把输入排好序，之后每次只遍历当前节点之后的节点就可以了。

此外，需要注意的是:

    for (int i = pos; i < num.length; i++) {
        ......
    }


控制的每一个步骤的选择情况，此处用pos控制是因为要保证数组元素的升序排列。






程序：



    class Solution {
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
        public ArrayList<ArrayList<Integer>> subsets(ArrayList<Integer> S) {

            ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        
            ArrayList<Integer> list = new ArrayList<Integer>();

            Collections.sort(S);

            backTrack(result,list,S,0);
        
            return result;
      
        }

        private void backTrack(ArrayList<ArrayList<Integer>> result, ArrayList<Integer> list, ArrayList<Integer> num, int pos){
    
            result.add(new ArrayList<Integer>(list));       //每一个中间list都是一个答案

            for(int i = pos; i < num.size(); i++){          //因为num中的元素都是已经排好序的，所以每次都从当前节点pos往后开始
            
                list.add(num.get(i));

                backTrack(result,list,num,i+1);

                list.remove(list.size()-1);                 //消除已经遍历完的节点，返回上一节点
            
            }


        }
        
    
    }





2.Unique subsets        求带重复元素集合的子集


链接：http://www.lintcode.com/zh-cn/problem/subsets-ii/



题目分析：

(1)



分析可知，与上一题目十分相似，不同的只是数组中有重复元素，所以要想办法如何去除


(2)解决重复——对重复元素编号，每次按顺序依次选择重复元素


如{1, 2(1), 2(2), 2(3)}， 规定{1, 2(1)}和{1, 2(2)}重复， {1, 2(1), 2(2)}和{1, 2(2), 2(3)}重复。

规定必须从第一个2开始连续取（作为重复集合中的代表），如必须是{1, 2(1)}不能是{1, 2{2})。




程序：


    class Solution {
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
        public ArrayList<ArrayList<Integer>> subsetsWithDup(ArrayList<Integer> S) {
        // write your code here
        
            ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();

            ArrayList<Integer> list = new ArrayList<Integer>();

            Collections.sort(S);

            backTrack(result,list,S,0);

            return result;
        
        }
    
    
    
        private void backTrack(ArrayList<ArrayList<Integer>> result,
            ArrayList<Integer> list, ArrayList<Integer> num, int pos) {
        
        result.add(new ArrayList<Integer>(list));
        
            for(int i = pos; i < num.size(); i++){
            
                if(pos != i && num.get(i) == num.get(i-1)){             //确定应该跳过的情况
                    continue;
                }
            
                list.add(num.get(i));

                backTrack(result,list,num,i+1);

                list.remove(list.size()-1);
            
            }
        
        }
       
    }







3.Permutation           求所给集合的全排列


链接：http://www.lintcode.com/zh-cn/problem/permutations/


![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson1/Permutation.png)





题目分析：


考虑套用求子集题中的模板，确定需要修改的地方：

(1)输出条件：

求subset时，每个中间结果list都是要输出的答案

求permutation时，中间结果list满时才输出


(2)遍历方法：



第一种就是用for循环，如果当前的元素已经在集合中存在了就不再添加

第二种是交换法，每个元素都能和当前位置的元素进行交换，然后再进行下一个位置的元素交换

我用的是for循环



(3)顺序：


求全排列时不限定各元素按大小排列其顺序，所以不需要专门设定一个变量来记录位置



程序：


    class Solution {
    
    /**
     * @param nums: A list of integers.
     * @return: A list of permutations.
     */
        public ArrayList<ArrayList<Integer>> permute(ArrayList<Integer> nums) {
        // write your code here
        
            ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();

            ArrayList<Integer> list = new ArrayList<Integer>();
        
            if(nums == null ){

                return result;

            }
        
        
        
        
            perm(result,list,nums);
        
            return result;
       
    
        }
    
        public static void perm(ArrayList<ArrayList<Integer>> result, ArrayList<Integer> list, ArrayList<Integer> num){
        
            if(list.size() == num.size()){         //三个元素时才进行输出

                result.add(new ArrayList<Integer> (list));

            }
        
        
            for(int i = 0; i < num.size(); i++){

                if(list.contains(num.get(i))){     //list中有的元素就不再添加

                    continue;

                }
            
            list.add(num.get(i));

            perm(result,list,num);

            list.remove(list.size()-1);

            }
        
        
        }
    }





4.Unique permutations           求带重复元素集合的全排列


链接：http://www.lintcode.com/zh-cn/problem/subsets/




![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson1/Permutation2.png)




题目分析：


对Permutation的模板进行改写：


同样的，应该对重复元素进行判断，思路和unique subset一样，即对重复元素进行编号，按顺序加入进List中


这题一开始没搞懂的就是“何时跳过的问题”：

由于不是按照大小顺序输出没有上题中的pos变量，所以要创建一个新的数组visited[]来表示这个节点是否加入到集合list中，


    if(pos[i] == 1 || (i != 0 && num.get(i) == num.get(i-1) && pos[i - 1] == 0)){
                    
        continue;

    }


这个判断条件我是这么理解的，只要这个元素在集合中有，就直接跳过。除此之外，在i > 0 时(这个很重要，是防止i-1出现数组越界的情况)：

num.get(i) == num.get(i-1)      当前元素与上个元素相同，因为输入数据是按顺序排列好，相同元素一定相邻。


pos[i - 1] == 0            这句话是表明上一个元素没有被加入到list中，因为重复元素的添加一定是按照顺序进行的，

所以，这说明了当前的元素不应该进行添加。



程序：



    class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of unique permutations.
     */
        public ArrayList<ArrayList<Integer>> permuteUnique(ArrayList<Integer> nums) {
        // write your code here
        
            ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();

            ArrayList<Integer> list = new ArrayList<Integer>();

            int[] visited = new int[nums.size()];

            if(nums == null ){
                
                return result;
            }
        
            Collections.sort(nums);
        
        
            perm(result,list,nums,visited);
        
            return result;
        
        
        }
    
    
        public static void perm(ArrayList<ArrayList<Integer>> result, ArrayList<Integer> list, ArrayList<Integer> num, int[] visited){
        
            if(list.size() == num.size()){

                result.add(new ArrayList<Integer> (list));

            }
        
        
            for(int i = 0; i < num.size(); i++){

                if(visited[i] == 1 || (i != 0 && num.get(i) == num.get(i-1)) && visited[i-1] == 0){
                
                    continue;
            
                }
            
                visited[i] = 1;

                list.add(num.get(i));

                perm(result,list,num,visited);

                visited[i] = 0;

                list.remove(list.size()-1);

            }
        
        }
     
    }


















