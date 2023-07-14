---
title: 简单代码克隆检测(经验性分析初版)
comment: true
copyright: true
date: 2018-09-17 14:38:24
categories: Scala
tags: clone_detection
---

这周主要按照上次开会的内容作了两个实验，第一个是统计20个github上star最多的scala项目中高阶函数的使用情况，第二个是借助github api 查看高阶函数所在文件的创建和修改的人时候固定。   

<!--more-->

## 提取高阶函数   

这一步就是在之前构造AST的代码上做进一步进行修改。之前博客中提到高阶函数的定义：   
满足以下条件之一：   
* 接受一个或多个函数作为输入  
* 输出一个函数    

就是高阶函数：   

但是由于scala 函数默认最后一句话为返回值，且利用工具scala-mate查看不出最后一句话的类型，所以我只能从传入的是个函数这个判断条件来进行提取数据。  
高阶函数示例：     
```
def sum(f:Int=>Int):(Int,Int)=>Int={
       def sumF(a:Int,b:Int):Int=
         if(a>b) 0
         else f(a)+sumF(a+1,b)
       sumF
   }
```
构造的AST树：   
![图片描述](/images/133.png)  

所以只要判断函数的参数里面有函数的类型即`Type.Function`即可，所以修改后的代码如下：  
![图片描述](/images/134.png)  
20个项目选取的网址：  
https://github.com/search?l=Scala&o=desc&p=1&q=scala&s=stars&type=Repositories  
接下来就是git clone 20个项目，跑实验，统计每个项目中高阶函数的个数，统计结果如下：  



![图片描述](/images/135.png)   

通过观察实验结果可以发现，高阶函数占比并不是很高，除了`fpinscala`,`scalaz`,`sbt` 三个项目占比较高外，其他项目都低于10%。说明高阶函数的普及型并不是很高。   

## 获取commit信息  

通过github api 获取文件的commit信息，很简单，就直接写了一个python 函数，统计了每个项目每个高阶函数所在文件的commit的历史作者和最早提交的作者。   

提取函数的代码链接：https://gitee.com/iss2015302580249/scalaCommitInfo  

在提取commit的信息的时候遇到了一个问题，就是提取账户名的时候，会有不存在的现象，也就是  `login`字段为空，我感觉应该是账户已经注销了，所以我提取的是commit里面的`author`字段的信息，也就是用户姓名的信息。   

![图片描述](/images/136.png)   

最终的统计结果：   
![图片描述](/images/137.png)   

通过实验结果我们可以看出，高阶函数所在文件创始者总数占比也很低，但是我们不能确定，文件的创始者就是高阶函数的创始者，这一点有待讨论和解决：    

`实验最终数据链接`：https://gitee.com/iss2015302580249/scala_data 

![图片描述](/images/138.png) 
