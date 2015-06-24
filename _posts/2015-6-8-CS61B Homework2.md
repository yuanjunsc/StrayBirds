---
layout: post
title: CS61B Homework2
category: 公开课
comments: true
---

CS61B Homework2




作业链接：
http://www.cs.berkeley.edu/~jrs/61b/hw/hw2/




一、题目要求：


将date.java补充完整，使输出正确。（并自己补充isLeapYear的测试数据）

程序简单直接写了。。。



二、解题思路：

判断是否闰年

	
    public static boolean isLeapYear(int year) {
    	
    	if(year%100 == 0){

    		if(year%400 == 0){

        		return true;

      		}

      		else

        		return false;

    	}

    	else{

    		if(year%4 == 0){

        		return true;

      		}

      		else

        		return false;

    	}

  	}



返回某年某月的天数
 

  public static int daysInMonth(int month, int year) {
    
    	if(isLeapYear(year)){

      		if(month == 2){

        		return 29;

     		}

      		else if (month==1 || month==3 || month==5 || month==7 || month==8 || month==10 || month==12) {

        		return 31;

      		}

      		else{

        		return 30;

      		}

    	}

    	else{

     		if(month == 2){

        		return 28;

      		}

      		else if (month==1 || month==3 || month==5 || month==7 || month==8 || month==10 || month==12) {

        		return 31;

      		}

      		else{

        		return 30;

      		}

    	}

  	}


  	public boolean isBefore(Date d) {
   
    	if(this.year>d.year){

      		return false;

    	}

    	else if(this.year==d.year){

      		if(this.month < d.month){

        		return true;

      		}

      		else if (this.month == d.month){

        		if(this.day < d.day){

          			return true;

        		}

        	else

          		return false;

      		}

      		else{

        		return false;

      		}
   		}

    	else{

     		return true;

    	}
  	}


计算今天是此年的第几天


  	public int dayInYear() {
   
    	int sum=0;

    	for(int i = 1; i <= this.month-1; i++){

      		sum = sum + daysInMonth(i,this.year);

    	}

    	return sum+this.day;
   
  	}



  	public int difference(Date d) {
    
    	int sum=0;

    	if(this.year == d.year){

    		int sum1=0;

       		int sum2=0;
        
    		sum1=this.dayInYear();

    		sum2=d.dayInYear();

    		sum = sum1-sum2;
    	}

    	else if(this.year < d.year){
    	
    		for(int i =this.year+1; i<=d.year-1;i++){

    			if(isLeapYear(i)){

    				sum=sum+366;
    			}

    			else{

    			sum=sum+365;

    			}

    		}

    		if(isLeapYear(this.year)){

    			sum = -(366-this.dayInYear() + sum + d.dayInYear());

    		}

    		else{

    		sum = -(365-this.dayInYear() + sum + d.dayInYear());

    		}
    	
    	}

   		else if(this.year > d.year){
    	
    		for(int i =d.year+1; i<=this.year-1;i++){

    			if(isLeapYear(i)){

    				sum=sum+366;

    			}

    			else{

    				sum=sum+365;

    			}
    		}

    		if(isLeapYear(d.year)){

    			sum = 366-d.dayInYear() + sum + this.dayInYear();

    		}

    		else{

    			sum = 365-d.dayInYear() + sum + this.dayInYear();
    		}
    	
    	}

    	return sum;

  	}




三、程序结果：



![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework2/Homework2.png)