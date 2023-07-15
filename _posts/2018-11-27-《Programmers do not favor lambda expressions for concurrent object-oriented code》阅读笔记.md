---
title: 《Programmers do not favor lambda expressions for concurrent object-oriented code》阅读笔记
comment: true
copyright: true
date: 2018-11-27 19:58:51
categories: 论文阅读笔记
tags: Lambda expression  Concurrency
---

原文链接： https://link.springer.com/content/pdf/10.1007%2Fs10664-018-9622-9.pdf  
作者：MSebastian Nielebock，Robert Heumüller，Frank Ortmeier
文章来源： Empir Software Eng 2018
简介： 本文针对`lambda expression`，提出了`concurrent applications`与`lambda expression`可能存在一定联系，并通过一系列实验和实验数据的分析验证了自己的观点。  

<!--more-->  

## Motivation and Research Questions

`lambda expressions`有很优越性，它允许声明匿名函数，能够更容易将行为转化为程序组件。在一些纯(purity)表达式中，能够保证代码没有副作用等等，这些好处推动了程序员更加广泛的使用`lambda expressions`。

### 前人主要研究的问题

`Järvi and Freeman 2010; Gyori et al. 2013` 等人在自己的论文中声明了`lambda expressions`的几个优点或者影响即：  

* 减少了代码数量
* 促进了并行应用的发展
* 使组合软件功能更加容易
* 对软件的性能上没有非常严重的不良影响

本文主要针对第二点做了更深入的研究。

### 本文主要的研究问题 

* 并发上下文中lambda表达式的突出程度如何，分析的语言在这方面有何不同？ (三种语言c#,c++,java，3000个项目)  
* 在并发上下文中，开发人员是更喜欢非捕捉lambda(non-capturing)还是捕捉lambda(capturing)   
non-capturing:即在并发上下文中lambda表达式没有引入外部变量，更安全。  
![图片描述](/images/191.png)  
capturing:即在并发上下文中lambda表达式引入了外部变量，不安全。  
![图片描述](/images/190.png)  
* 除了并发编程之外，还有哪些其他用例是lambda表达式的典型用例？ 


##  Experimental Analysis of the Research Questions  

### RQ1   

针对第一个RQ,本文主要做了以下实验：  

* 统计了3000个c#（1000）,java（1000）,c++（1000）开源项目(github star ranking)  
* 统计了所有项目中包含lamda 表达式的项目的数量  
  ![图片描述](/images/192.png)  
* 统计了所有项目中包含并行上下文项目的数量  
  ![图片描述](/images/193.png)  
* 统计了所有项目中既包含lambda表达式又包含并行上下文项目的数量  
  ![图片描述](/images/194.png)    
* 定义了两个度即强度(Intensity),采用度(Utilization)  
    * intensity of lambda expression usage in concurrent contexts:  
    ![图片描述](/images/195.png)  
    p:为一个项目  
    numberLambdaInConcContext(p) : 为求出一个项目中在并行上下文中有lambda表达式这种情况的数量的函数
    numberLambda(p)：为求出一个项目中所有lambda表达式的数量的函数 
    * utilization of lambda expressions  
    ![图片描述](/images/196.png)  
    p:为一个项目  
    numberFilesWithLambdaInConcContext(p)：为求出一个项目在并行上下文中有lambda表达式所在文件数量的函数  
    numberFilesWithConcContext(p):为求出一个项目中存在并行上下文所有文件数量的函数  
* 针对两个`度`,利用`Kruskal-Wallis test`和`Dunn’s test`来探索三种语言项目的相关性。 
  ![图片描述](/images/197.png)  
  ![图片描述](/images/198.png)  
  ![图片描述](/images/199.png)  

结论： 
* C＃项目比C ++和Java项目在并发上下文中具有明显更高的lambda表达式强度。 
* 在所有三种语言中，并发上下文不是lambda表达式的主要用例(应用场景)  
* 与其他两种语言的程序员相比，C＃程序员在实现并发上下文时更频繁地应用lambda表达式。   
* 除了这些显着差异之外，结果表明大量项目可以在并发上下文中增加lambda表达式的应用。  

### RQ2  

针对第二个RQ,本文主要做了以下实验：  

* 还是之前的3000个项目  
* 统计了所有项目中 `capturing lambda` 和`non-capturing lambda`的数量  
 ![图片描述](/images/200.png)  
* 求出采用度(Utilization) ,利用`Kruskal-Wallis test`和`Dunn’s test`来探索三种语言项目的相关性  
 ![图片描述](/images/201.png)  

结论：  
* 尽管捕获lambda表达式可能会引入额外的副作用，但所有被调查的编程语言的开发人员在并发中比在一般应用程序中更多地应用lambda表达式，与预期相反。  

### RQ3  

针对第三个RQ,本文主要做了以下实验：  

* 还是之前的3000个项目  
* 根据源代码文件的名字（定性分析，没给具体的方法，自己看的？），将该文件的应用场景分为以下几类：  
![图片描述](/images/202.png)  
* 统计了每种语言应用场景的分布情况(文件数量)  
 ![图片描述](/images/203.png) 
 ![图片描述](/images/204.png) 
 ![图片描述](/images/205.png)  

结论：  

* 开发人员倾向于在自动测试中，在算法和用户界面的实现中使用lambda表达式  
* 三种语言的应用场景也不尽相同：  
![图片描述](/images/206.png)  



## Related Work  

###  Analyzes on the Adoption of Lambda Expressions  

相关工作里面对我们有用的只有这个感觉，对是否采用lambda表达式进行分析   

* `Mazinanian et al. (2017)`等最近对Java中的lambda表达式进行了一项研究。特别是，他们分析了来自GitHub的241个开源项目的历史数据，以找到开发人员最近添加了lambda表达式的项目。然后他们采访了有意义的开发人员，以发现他们插入具体lambda表达式的原因。他们的主要见解是lambda表达式的采用正在增加，并且大多数lambda都是手动插入的。此外，分析表明，由于几个原因，例如缺少异常处理，开发人员不使用标准库提供的许​​多功能接口。此外，在他们的定性研究中确定了采用lambda表达式的几个关键原因。这些包括减少代码大小，避免类实现，包装行为，迁移到Java 8或强制使用lambda表达式的库。除了这些抽象的原因，他们还确定了lambda表达式的一些特定用例，例如，实现了监听器和回调，简化了测试，以及多线程和异常处理。与我们的研究相反，他们只调查了一种编程语言中的lambda表达式的使用，即Java。此外，我们还进行了跨语言分析，可以更深入地了解编程语言之间的差异。这可以帮助语言设计者通过学习其他语言的实现来改进他们当前的lambda表达式设计  

* `Uesbeck et al. (2016)`等对58名不同经验水平的参与者进行了用户实验。在本实验中，他们被要求通过使用迭代器接口或lambda表达式来解决C++中的编程任务。他们的研究结果表明，有经验的用户更有可能正确应用lambda表达式。 这表明我们必须仔细选择我们分析中使用的项目，因为大量新手开发人员正在处理的项目可能具有更高的错误使用的lambda表达式。  

* `Tsantalis et al. (2017)`等开发了一个Java重构工具，旨在通过插入lambda表达式来减少代码克隆。 他们表明，他们的工具可以在开源项目中正确地重构57-60％的类型-2（除了标识符名称语法相同）和类型-3（添加或删除语句）克隆。 特别是，他们指出，与生产代码中的克隆相比，驻留在测试代码中的克隆比例更高，可以成功重构（71％对51％）。这些发现揭示了lambda表达式的潜在用例，即克隆代码减少和自动测试。



