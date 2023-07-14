---
title: 简单代码克隆检测一(AST leaf)
comment: true
copyright: true
date: 2018-06-10 17:56:28
tags: code_detection
categories: Scala
---
### 今天主要将昨天得到的数据，放到之前看到的模型中跑了一下，看了一下效果，简单叙述一下实验。<!--more-->
* 实验思路：
	* 将提取的Scala叶子节点的特征作为文本数据，输入到`AutoenCODE`中 ：AutoenCODE is a Deep Learning infrastructure that allows to encode source code fragments into vector representations, which can be used to learn similarities. https://github.com/micheletufano/AutoenCODE
	基本上所有的代码这个网站已经提供了，所以只要将代码clone到本地，配置一下环境就可以开始我们的实验。具体的AutoenCODE的原理，我会在以后的博客中详细的解释，本篇博客主要讲如何使用这个框架。
* 实验步骤：
	* 按照`AutoenCODE`给的教程，第一步是将我们整理的数据转化成词向量，这里他使用的工具是word2vec，这里注意一下，他的这个word2vec需要build但是windows系统不支持这个build，所以我将转化词向量的这部分的工作转移到了centos服务器上进行，最终得到了测试样本的所有的词向量的数据。
	* 接下来就将这个词向量输入到`Recursive Autoencoder`(论文中提及是一个斯坦福的情感分析器)中去，最终得到五个结果文件(这里提及一点，训练时间实在是太长了，源代码是使用matlab写的。27000条数据整整跑了一个小时，而且CUP满负荷运行，可能是我的电脑配置低，之后需要优化)。分别是：
		* `data.mat` contains the input data including the corpus, vocabulary (a 1-by-|V| cell array), and We (the m-by-|V| word embedding matrix where m is the size of the word vectors). So columns of We correspond to word embeddings.
		* `corpus.dist.matrix.mat` contains the distance matrix saved as matlab file. The values in the distance matrix are doubles that represent the Euclidean distance between two sentences. In particular, the cell (i,j) contains the Euclidean distance between the i-th sentence (i.e., i-th line in corpus.src) and the j-th sentence in the corpus.
		* `corpus.dist.matrix.csv` contains the distance matrix saved as .csv file.
		* `corpus.sentence_codes.mat` contain the embeddings for each sentence in the corpus. The sentence_codes object contains the representations for sentences, and the pairwise Euclidean distance between these representations are used to measure similarity.
		* `detector.mat` contains opttheta (the trained clone detector), hparams, and options.   

    这里对我们最有用的就是那个矩阵，它显示两句话的距离大小，越小越相似。
    ![图片描述](https://i.loli.net/2018/06/10/5b1cfe2b76237.png)
    那么大的矩阵，怎么进行分析！！，只能硬着头皮通过写matlab代码，将矩阵中每行的最小值（非零）提取出来，这样就能得到27000多个最小值，然后再通过这27000个最小值进行筛选，因为本次实验主要看一下效果所以没有注意到那么多的细节，先把最小值求出来先看看。
    然后我将最小值又进行了划分，论文中说他们的想法是如果距离小于`1e-8`就认为他们是克隆的代码，然后我以这个为分界线进行了筛选，发现只有5对符合要求，最终的结果在最终结果那里进行展示。求取最小值代码：  
	![图片描述](https://i.loli.net/2018/06/10/5b1cffc14224f.png)  

* 实验结果：
    * 当判断距离为1e-8时(5对) 
    * 当判断距离为1e-4时(75对)
    * 当判断距离为1e-2时(800多对)
    通过观察主要分为以下几个类型：  
        * 函数重载和相似函数(在同一个文件中)  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf6dcac4d4.png)  
        (5对)  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf8dac24e9.png)  
        (800多对)  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf73260cd5.png)  
        (75对)  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf808e710b.png)  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf821d17bc.png)  
        (75对)  
         D:\Git\spark\core\src\test\scala\org\apache\spark\deploy\master\MasterSuite.scala  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf8420d854.png)  
        D:\Git\spark\core\src\test\scala\org\apache\spark\deploy\master\MasterSuite.scala
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf859f2b3b.png)  
        (75对)  
        * 父子继承关系或者同时继承同一个父类的子类之间(不同文件)   
        D:\Git\spark\core\src\main\scala\org\apache\spark\scheduler\DAGScheduler.scala  (父类) 
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf8c23604c.png)  
        D:\Git\spark\core\src\test\scala\org\apache\spark\scheduler\TaskSetManagerSuite.scala (子类) 
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf8ae30b8c.png)  
        (75对)  
        D:\Git\spark\core\src\test\scala\org\apache\spark\scheduler\SparkListenerWithClusterSuite.scala  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf883e30b0.png)  
        D:\Git\spark\core\src\test\scala\org\apache\spark\deploy\LogUrlsStandaloneSuite.scala  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf8985af35.png)
        (800对)  
        * 相似或者相同的函数(不同文件)  
        D:\Git\spark\core\src\main\scala\org\apache\spark\util\collection\PrimitiveKeyOpenHashMap.scala  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf6f60d131.png)  
        D:\Git\spark\graphx\src\main\scala\org\apache\spark\graphx\util\collection\GraphXPrimitiveKeyOpenHashMap.scala  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf71289bdf.png)  
        (75对)  
        D:\Git\spark\core\src\main\scala\org\apache\spark\deploy\history\HistoryServerArguments.scala  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf74b107b6.png)  
        D:\Git\spark\core\src\main\scala\org\apache\spark\deploy\worker\WorkerArguments.scala  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf760d148d.png)  
        (75对)  
        * 不像是克隆的函数(我的观点)  
        D:\Git\spark\core\src\main\scala\org\apache\spark\status\LiveEntity.scala  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf7772a91a.png)  
        D:\Git\spark\core\src\main\scala\org\apache\spark\status\LiveEntity.scala  
        ![图片描述](https://i.loli.net/2018/06/10/5b1cf7c51aa3d.png)  
        (75对)
* 实验总结：
    由于时间和人手有限，现在只是对这几个结果进行了分析，还有很多对都没有看，之后找时间看看还有没有其他类型，或者老师可以分配几个人帮我看看。
* 附录：  
    在得到结果以后，这是忘了如何去找源文件，这里我在原来的parse的基础上加上了一个统计样本所在的文件的文件，通过`行数`来查找对应的文件，感觉很费时费力。  
    ![图片描述](https://i.loli.net/2018/06/10/5b1cf96c01a60.png)