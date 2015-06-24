---
layout: post
title: CS61B Homework1
category: 公开课
comments: true
---

CS61B Homework1




作业链接：

http://www.cs.berkeley.edu/~jrs/61b/hw/hw1/


一、题目要求：



1。补充完整OpenCommercial.java 

要求： 实现从键盘输入一个公司的名字，然后输出www.公司名.com的网页中的前五行。注意这五行还要倒序输出。

举例：

输入： 1point3acres

输出：<meta name="viewport" content="width=device-width; initial-scale=1.0; maximum-scale=1.0; user-scalable=0;">                            
<meta charset="UTF-8" />
<head>
<html lang="en-US">
<!DOCTYPE html>
   
2写Nuke2.java

要求：从键盘输入一个字符串，然后删掉字符串的第二个字符，再重新输出该字符串。

举例：

输入：skin

输出：sin   



二、解题思路：



1.抓取网页


	public class TestWeb {

 		String urlString;

 		String[] Message = new String[5];

 		public static void main(String[] args) throws Exception {

	 		BufferedReader keyboard;

	 		String inputLine;

	 		keyboard = new BufferedReader(new InputStreamReader(System.in));

	 		System.out.print("Please enter the name of a company (without spaces): ");

	 		System.out.flush();        /* Make sure the line is printed immediately. */

	 		inputLine = keyboard.readLine();

			String urlString2 = "http://www." + inputLine +".com";
	
			TestWeb client = new TestWeb(urlString2);

	 		client.run();

 		}
 
 		public void run() throws Exception {

  		//生成一个URL对象 

	 	URL url = new URL(urlString);

  		//打开URL 

	 	HttpURLConnection urlConnection = (HttpURLConnection) url.openConnection();

  		//得到输入流，即获得了网页的内容 

	 	BufferedReader reader = new BufferedReader(new InputStreamReader(urlConnection
    .getInputStream()));

  		String line;

  		// 读取输入流的数据，并显示

  		for(int i = 0; i < 5; i++){

	  		Message[i] = reader.readLine();

	  
  		}

  		System.out.println(Message[4]);

  		System.out.println(Message[3]);

 		System.out.println(Message[2]);

  		System.out.println(Message[1]);

  		System.out.println(Message[0]);
  
  		}
	}




2.字符串截取




	public class TestString {

		public static void main(String[] args) throws Exception {

			BufferedReader keyboard;

			String inputLine;

			String outputLine;

			keyboard = new BufferedReader(new InputStreamReader(System.in));

			System.out.print("Please enter the name of a String:");

			inputLine = keyboard.readLine();

			System.out.println(inputLine);

			char[] content = inputLine.toCharArray();
		
			content[1] = content[0];

			for(int i = 1; i < content.length; i++){

				System.out.print(content[i]);

			}

		}

	}



三、程序结果：



![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework1/Homework1-1.png)


![_config.yml]({{ site.baseurl }}/images/DataStructure/Homework1/Homework1-2.png)