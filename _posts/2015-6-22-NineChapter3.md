---
layout: post
title: Binary Tree
category: 刷题
comments: true
---



NineChapter lesson3







一、Binary Tree DFS Traversal



1.遍历（前序、中序、后序）




![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson3/bianli.png)


模板：

    public void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        // do something with root      前序
        traverse(root.left);
        // do something with root      中序
        traverse(root.right);
        // do something with root      后序
    }





2.分治算法


分开计算各种情况，再进行合并


模板：

    public ResultType traversal(TreeNode root) {

        if(root == null){
            //do something and return;
        }

        //Divide
        ResultType left = traversal(root.left);
        ResultType right = traversal(root.right);

        //Conquer
        ResultType result = Merge from left and right.
        return result;
    }




3.Binary Tree BFS Traversal

模板：


    ArrayList result = new ArrayList();

    if(root == null){
        return result;
    }

    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.offer(root);

    while(!queue.isEmpty()){
        ArrayList<Integer> level = new ArrayList<Integer>();  //这个集合用来储存每次出队的值
        int size = queue.size();
        for(int i = 0; i < queue.size; i++){
            TreeNode head = queue.poll();
            level.add(head.val);
            if(head.left != null){
                queue.offer(head.left);
            }
            if(head.right != null){
                queue.offer(head.right);
            }     
        }
        result.add(level);

    }

    return result;









二、Lintcode例题



1.Maximum Depth of Binary Tree       求二叉树的最大高度

链接：http://www.lintcode.com/zh-cn/problem/maximum-depth-of-binary-tree/



![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson3/MaximumDepth.png)




题目分析：


使用分治法---最大高度 = max(左子树最大高度，右子树最大高度) + 1 (root节点)



程序:
    
    public int maxDepth(TreeNode root) {
        // write your code here
        
        if(root == null){
            return 0;
        }
        
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);

        return Math.max(left, right) + 1;
  
    }






2.MinDepth       求二叉树的最小高度

链接：http://www.lintcode.com/zh-cn/problem/minimum-depth-of-binary-tree/



![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson3/MinDepth.png)




题目分析：


(1)同样考虑使用分治法---最小高度 = min(左子树最小高度，右子树最小高度) + 1 (root节点)


(2)与求最大高度不同的是，当节点只有一个儿子时，如果只是单纯的求Min(left,right)，那么一定返回零，

所以，要在返回最大高度的基础上对这种情况进行处理。即只有一个儿子时直接返回，另一个高度加一。



程序：


    public int minDepth(TreeNode root) {
        // write your code here
        if(root == null){
            return 0;
        }
        
        if(root.left == null && root.right == null){     //遇到叶子节点时，直接返回1
            return 1;
        }
            
        int left = minDepth(root.left);     //看成已经求得此节点左右子树的最小高度
        int right = minDepth(root.right);
            
        if(root.left == null){   //如果此节点左子树为空，那么此节点最小高度为右子树最小高度加1
            return right + 1;
        }
        if(root.right == null){  //如果此节点右子树为空，那么此节点最小高度为左子树最小高度加1
            return left + 1;
        }

        return Math.min(right,left) + 1;   //如果此节点左右子树都存在，那么就返回左右子树最小高度加1
        
    }




3.Binary Tree Maximum Path Sum       求二叉树的最大路径和

链接：http://www.lintcode.com/en/problem/binary-tree-maximum-path-sum/



![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson3/MaximumPath.png)



题目分析：



(1)首先同样考虑使用分治法，即最大路径和 = 最大左子树路径和 + 最大右子树路径和 + root.val

(2)考虑其他情况，因为各点的值存在正负，所以综合考虑有如下情况：


最大路径经过左右子树和root

最大路径在左子树

最大路径在右子树。。。。。

(3)即二叉树中有两种路径，一种穿过左右子树和root，一种只在一个子树中

singlePath: 从root往下走到任意点的最大路径，这条路径可以不包含任何点

maxPath: 从树中任意到任意点的最大路径，这条路径至少包含一个点（也就是root）


(4)最重要的一点就是这道题我只是大致弄明白，所以先默写并背诵全文




程序：



    class ResultType{
        int singlePath,maxPath;
        ResultType(int singlePath,maxPath){

        this.singlePath = singlePath;
        this.maxPath = maxPath;

        }
    }


    public ResultType helper(TreeNode root){

        if(root == null){
            return new ResultType(0,Integer.MIN_VALUE);
        }

        ResultType left =  helper(root.left);
        ResultType right = helper(root.right);

        int singlePath = Math.max(left.singlePath,right.singlePath) + root.val;
        singlePath = Math.max(0,singlePath);

        int maxPath = Math.max(left.maxPath,right.maxPath);
        maxPath = Math.max(maxPath,left.singlePath + right.singlePath + root.val);

        return new ResultType(singlePath,maxPath);

    }

    public int maxPathSum(TreeNode root){

        ResultType result = helper(root);

        return result.maxPath;
    }




4.Lowest Common Ancestor---最近公共祖先


链接：http://www.lintcode.com/zh-cn/problem/lowest-common-ancestor/



![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson3/lowestCommonAncestor.png)



5.层次遍历 = BFS



6.二叉树的锯齿形层次遍历



链接：http://www.lintcode.com/zh-cn/problem/lowest-common-ancestor/



![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson3/zigzagLevelOrder.png)





题目分析：


定义两个stack，curr和next：

每次从stack中读出栈顶元素，非空时，按head.---head.right，head.right---head.left顺序放入next

交换curr和next,循环进行。





程序：


    public ArrayList<ArrayList<Integer>> zigzagLevelOrder(TreeNode root) {
        // write your code here
        
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>> ();
        
        
        if(root == null){
            return result;
        }
        
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        
        while(!queue.isEmpty()){
            
            ArrayList list = new ArrayList();
            int size = queue.size();
            
            for(int i = 0; i < size; i++){
                TreeNode head = queue.poll();
                list.add(head.val);
                if(head.left != null && head.right != null){
                    
                    if(k == 0){
                        
                        if(head.right != null){
                            queue.offer(head.right);
                        }
                        if(head.left != null){
                            queue.offer(head.left);
                        }
                        k = 1;
                    }
                    
                    else{
                        
                        if(head.left != null){
                            queue.offer(head.left);
                        }
                        if(head.right != null){
                            queue.offer(head.right);
                        }
                        k = 0;
                        
                    }
                    
                }
                
                else{
                    
                    if(head.left != null){
                        queue.offer(head.left);
                    }
                    if(head.right != null){
                        queue.offer(head.right);
                    }
                }  
            }
            result.add(list);     
        }
        return result;
    }



P.S

我直接用levelorder的程序，然后用一个全局变量控制每次的输出次序，一只报有问题，是这种输入情况：


{1,2,3,4,#,#,5,#,#,6,7,#,#,#,8}



7.Validate Binary Search Tree


如果二叉树是二叉查找树，那么用中序遍历一定会得到一个递增数组，照此判断即可





8.在二叉查找树中插入节点

链接：http://www.lintcode.com/zh-cn/problem/insert-node-in-a-binary-search-tree/


![_config.yml]({{ site.baseurl }}/images/NineChapter/Lesson3/insertNode.png)




题目分析：

二叉查找树的元素有一定排列顺序，所以每次按照元素值最接近的位置，生成新节点





程序：

    if(root == null){
            return node;
        }
        
        TreeNode n = root;
        while(n != null){
            if(node.val > n.val){
                if(n.right == null){
                    n.right = node;
                    break;
                }
                n = n.right;
            }
            else{
                if(n.left == null){
                    n.left = node;
                    break; 
                }
                n = n.left;
            }
        }
        return root;
    }




三、总结


1.Binary Search Template---4 key points

见上文


2.Rotated Sorted Array---三类问题

(1)Find Minimum---只考虑A[mid]大小和区间端点大小关系即可，两种情况

(2)Find Target---需要考虑A[mid]

(3)why o(n) with duplicates ---有疑问！！！


3.Find Median in Two Sorted Array find kth --- 太难了，还不会！！！




