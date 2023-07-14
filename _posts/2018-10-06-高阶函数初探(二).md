---
title: 高阶函数初探(二)
comment: true
copyright: true
date: 2018-10-06 19:24:03
categories: 高阶函数
tags: scala 高阶函数
---

本周由于雅思笔试，口试延误了很长时间，所以实验做的比较晚。这周主要完成了对scala的一个项目项目的详细统计，统计结果包括高阶函数声明数，高阶函数调用数，以及这两者有什么关系；scala文档对高阶函数的定义；统计泛化功能高阶函数；最后自己总结了一点高阶函数的特点。  

<!--more--> 

## 高阶函数声明与调用  

上次实验我主要统计了所有的scala项目的高阶函数声明的数量，并没有对其调用进行统计，而且后来才发现我只统计了那些函数行数大于10行的函数，所以这次实验要重新进行统计。  
这次实验选取的项目为跑`playframework`:   
`playframework`是一个full-stack（全栈的） Web的应用框架，包括一个简单的无状态MVC模型，具有Hibernate的对象持续，一个基于Groovy的模板引擎，以及建立一个现代Web应用所需的所有东西。而且`playframework`是star最多的一个scala项目。   
实验步骤： 

* 提取数据：在提取数据的过程中发现，函数声明可以利用代码快速提取出来，但是函数调用并不能利用之前的工具提取，因为scalameta并不能像java中的JDT那样对整个项目进行分析，只能每次对一个文件进行解析，只是分析函数中的词法和语法，并不能分析出来函数调用这种语义信息，所以我就手动统计了每个函数的调用关系。方法是提取所有的函数，与高阶函数的名称一一对比，再进行原函数一一查看。   
* 统计数据：在提取完函数声明和函数调用数据后，我对这些数据进行了简单的统计：   
  ![图片描述](/images/139.png)   
  ![图片描述](/images/140.png)  
  这里说明一下，在统计所有的高阶函数里面，并不是所有的都能找到调用关系，而且有些函数都是一些重构或者构造函数，在这里我没有统计这些函数的调用。如：    
  ![图片描述](/images/141.png)  
  像`apply,map`这样的函数，在高阶函数里面就有很多，在所有函数里面搜索关键字就有几百条。  
* 分析数据：我们已经知道了函数声明和调用具体的数量了，接下来就是统计二者到底有啥关系了：  
通过统计分析发现大多数高阶函数的调用和声明都在同一个文件中：  
![图片描述](/images/142.png)   
    * 在一个文件中的函数调用：
    ![图片描述](/images/143.png)  
    ![图片描述](/images/144.png)  
    ![图片描述](/images/145.png)  
    * 不在同一个文件  
    ![图片描述](/images/146.png)  
    ![图片描述](/images/147.png)  
    ![图片描述](/images/148.png)  
    ![图片描述](/images/149.png)  


## 高阶函数声明&调用作者分析  

前面我们统计了一个项目中的高阶函数的分布，发现大多数函数声明和调用都发生在同一个文件中，所以我就打算统计一下那些没有在同一个文件中的高阶函数的声明和调用是不是同一个作者。  
所有的作者信息：  
![图片描述](/images/150.png)  
最先创建文件作者信息：   
![图片描述](/images/151.png)   
只有三对的初始化作者是相同的，其余作者都不相同。

## scala 文档中有关高阶函数的解释   

```
Higher order functions take other functions as parameters or return a function as a result. This is possible because functions are first-class values in Scala. The terminology can get a bit confusing at this point, and we use the phrase “higher order function” for both methods and functions that take functions as parameters or that return a function.
```
也就是之前我们说的高阶函数的定义，没有说明为什么要用高阶函数。   
但在另一个scala-exercise网站上举了一个例子：https://www.scala-exercises.org/scala_tutorial/higher_order_functions  

![图片描述](/images/152.png)  
![图片描述](/images/153.png)  
![图片描述](/images/153.png)  

总结：使用高阶函数可以减少代码，尤其是在比较复杂的流程中，这类似于Strategy模式，将核心算法整体替换，整个架构不变，比如一个有不同水平的棋类游戏，高阶函数的让这个实现更简单。  

## scala 高阶函数调用   

调用函数的类型：  
* 函数参数就在高阶函数附近：   
![图片描述](/images/155.png)  
* 函数参数就在函数调用中编写：  
![图片描述](/images/156.png)  
![图片描述](/images/157.png)  
* 函数参数来自上一级函数的参数  
![图片描述](/images/159.png)  
![图片描述](/images/160.png)  
* 函数参数是对象的一个方法  
![图片描述](/images/158.png)  

经过我粗略的统计，发现上面四中使用方法中，第一种使用的最少，使用最多的是第三种和第四种，函数参数就在函数调用中编写的情况处于两者之间，但是比第一种多很多。   

## 高阶函数的泛化  

之前所说的高阶函数的主要目的就是简化代码，然后抽象化。在项目统计高阶函数数据时，我发现很多高阶函数在参数面前都有一个[T],这根我们之前c++中的template<T>的功能相同，提供模板。  
![图片描述](/images/161.png)   

而且在所统计的数据中，模板高阶函数还占有很大的比重，在我提取的166个高阶函数中，有97个加了模板[T]。

