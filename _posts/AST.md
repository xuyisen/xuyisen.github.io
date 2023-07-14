---
title: AST
comment: true
copyright: true
date: 2018-06-08 09:42:52
tags: AST
categories: Scala
---
### Scala AST 叶子节点提取
* 背景：
	* 前几天由于考试没有来得及整理基础知识，今天在这里先整理一下这两天做的Scala叶子节点的值提取
    * Scala 是一门多范式（multi-paradigm）的编程语言，设计初衷是要集成面向对象编程和函数式编程的各种特性。Scala 运行在Java虚拟机上，并兼容现有的Java程序。Scala 源代码被编译成Java字节码，所以它可以运行于JVM之上，并可以调用现有的Java类库。(百度百科）
<!--more-->
* 实验步骤：
	* 使用的工具：
		* scala meta : https://scalameta.org/
		* Intellij Idea: https://www.jetbrains.com/idea/
	* 借助的参考资料：
		* scala meta的示例程序
		* 浏览器 构造 scala AST 树：https://astexplorer.net/#/gist/22cf8a3fcb2155c087ae94b4d194c1b6/d10c646ecfae4c69c919408aa3aaefb2deda2df7
	* 实验带代码： 
		查看 scala meta 源程序可以发现 ，该工具里面有一个Tree的类，该类有children 属性和parent属性：![图片描述](https://i.loli.net/2018/06/08/5b1a5c0fda8dd.png)
		所以可以根据这个类来进行遍历得到我们需要的叶子节点的数据，在这里我采用visitor的方式来进行遍历。主要的遍历的对象有以下几个：
		![图片描述](https://i.loli.net/2018/06/08/5b1a5cab0871e.png)
		分别代表Scala中的各个语法，这里在做的时候出现了几个问题。一个是Term.param和Type.param 需要“精准的查找”，不能像其他的Term.Name,Term.Annonate那样，可以通过Term来进行查找：![标题](https://i.loli.net/2018/06/08/5b1a5dc5f0c91.png)
        也就是说其他的Term里面的属性可以通过遍历Term然后再进行查找，但是这个Term.param必须在第一次遍历的时候就指出来，难道Term.param不属于Term?很奇怪。以后再查一查。
    * 代码的逻辑并不难，下面就开始打印叶子节点，通过观察浏览器AST解析器https://astexplorer.net/#/gist/22cf8a3fcb2155c087ae94b4d194c1b6/d10c646ecfae4c69c919408aa3aaefb2deda2df7
    发现：叶子节点主要在以下几个地方打印：
    ![图片描述](https://i.loli.net/2018/06/08/5b1a5f3dcd9de.png)基本数据类型  
    ![图片描述](https://i.loli.net/2018/06/08/5b1a5f81f12ba.png)Term.Name处![图片描述](https://i.loli.net/2018/06/08/5b1a5fb82699e.png)Type.Name 处
    还有一个是Name处
* 实验的最终结果：
	* ![图片描述](https://i.loli.net/2018/06/08/5b1a6012ac69a.png)Scala 源代码
	* ![图片描述](https://i.loli.net/2018/06/08/5b1a604443ddf.png) 提取的叶子节点
	* ![图片描述](https://i.loli.net/2018/06/08/5b1a609ceb152.png) 源代码
	* ![图片描述](https://i.loli.net/2018/06/08/5b1a60d3a65f9.png) 提取的叶子节点
* 总结：实验结果还未仔细观察，具体的细节有待改进，还有就是上次说的将string值改成<String>等基本数据的转化还未加入。


本博客持续更新。

