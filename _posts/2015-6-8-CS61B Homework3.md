---
layout: post
title: CS61B Homework3
category: 公开课
comments: true
---

CS61B Homework3




作业链接：
http://www.cs.berkeley.edu/~jrs/61b/hw/hw3/





一、题目要求：

Your task is to write two methods for removing successive duplicate items from
lists, and one method for adding them.  The smoosh() method operates on lists
represented as arrays, and the squish() method and twin() method operate on
singly-linked lists.


1. smoosh() takes an array of ints.  On completion the array contains

   *  the same numbers, but wherever the array had two or more consecutive

   *  duplicate numbers, they are replaced by one copy of the number.  Hence,

   *  after smoosh() is done, no two consecutive numbers in the array are the

   *  same.


   *  For example, if the input array is [ 0 0 0 0 1 1 0 0 0 3 3 3 1 1 0 ],
   *  it reads [ 0 1 0 3 1 0 -1 -1 -1 -1 -1 -1 -1 -1 -1 ] after smoosh()
   *  completes.



2.  squish() takes this list and, wherever two or more consecutive items are

   *  equals(), it removes duplicate nodes so that only one consecutive copy

   *  remains.  Hence, no two consecutive items in this list are equals() upon

   *  completion of the procedure.
   *
   *  After squish() executes, the list may well be shorter than when squish()

   *  began.  No extra items are added to make up for those removed.
   *
   *  For example, if the input list is [ 0 0 0 0 1 1 0 0 0 3 3 3 1 1 0 ], the

   *  output list is [ 0 1 0 3 1 0 ].
   *
   *  IMPORTANT:  Be sure you use the equals() method, and not the "=="

   *  operator, to compare items.
   **/




3.   twin() takes this list and doubles its length by replacing each node

   *  with two consecutive nodes referencing the same item.
   *
   *  For example, if the input list is [ 3 7 4 2 2 ], the

   *  output list is [ 3 3 7 7 4 4 2 2 2 2 ].
   *
   *  IMPORTANT:  Do not try to make new copies of the items themselves.

   *  Make new SListNodes, but just copy the references to the items.
   **/





二、解题思路：




1.smoosh():


    public static void smoosh(int[] ints) {
    
        String str = "";

        String mystr = "";

        for (int i = 0; i < ints.length; i++) {       //先将整数数组转化为字符串

            str = str + Integer.toString(ints[i]) + "";

        }


        char last = str.charAt(0);          //取出头字符

        int count = 0;                      //初始时没有重复的值

        for(int i = 1; i < str.length(); i++){

            if(str.charAt(i) == last){      //如果重复就进行计数累加

                count++;

            }

            else{                       //不重复的话就输出之前的字符串

                mystr = mystr  + last + "  " ;

                last = str.charAt(i);       //指向当前的字符

                
            }

        }

        mystr = mystr  + last + "  " ;         //输出最后一个字符
      
        for(int i =1; i <= count; i++){

            mystr = mystr + -1 + "  ";

        }

        //System.out.println(mystr);


        String[] temp = mystr.split("  ");

        for(int i=0;i<temp.length;i++)      //再将字符串转化整数数组

        {

            ints[i] = Integer.parseInt(temp[i]);

        }

     }




2.
     public void squish() {


        SListNode current = head;

        SListNode after = head;

        if(head != null){

            while(after.next != null){

                while(after.item.equals(current.item) && after.next != null){

                    after = after.next;
                }

                if(current.item.equals(after.item)){

                  current.next = null;

                }

                else{

                current.next = after;

                current = current.next;

              }
              
            }
        }


    }




3.
    public void twin() {
   
        SListNode current = head;

        while(current != null){

            SListNode after = new SListNode(null);

            after.item = current.item; 

            after.next = current.next;

            current.next = after;

            current = after.next;

        }

  }







三、运行结果：



![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework3/Homework3.png)


