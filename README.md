
# 深入了解PAT与PRISM
**_笔者最近几天一直在学习两款有关模型检验的软件的说明书，下面就把这两款软件的功能及其用法介绍一下_**

## 一、PAT工具

PAT是一款有新加坡国立大学研发的模型检验工具，其推出的主要特点就是模型语言的可拓展性，官方的下载网址[PAT官方下载](https://pat.comp.nus.edu.sg/?page_id=2587)

### PAT User Manual（PAT说明文档）
接下来我们来看一下有关PAT模型检验工具的相关说明文档。[User Manual](https://pat.comp.nus.edu.sg/wp-source/resources/OnlineHelp/htm/index.htm)版本是基于PAT3.5版本的。

**我们来看一下introduction**

![image](https://github.com/Gaoshiguo/Model-Checker-Introduction-of-PAT-PRISM-/blob/master/IMAGE/1.png)

这个的大概意思讲的就是PAT是一个独立的框架，用于支持并发，实时系统和其他可能域的模拟和推理。 它具有用户友好的界面，特征鲜明的模型编辑器和动画模拟器。 最重要的是，PAT实现了各种模型检查的技术，以满足不同的属性，例如死锁，散度，可达性，具有公平性假设的LTL属性，细化检查和概率模型检查。 为了获得良好的性能，在PAT中实现了高级优化技术，例如， 部分降阶，对称性约简，过程计数器抽象，并行模型检验。

**总而言之就是这个软件呢比较灵活，可以支持语言框架的拓展，跟PRISM和SPIN不太一样，具体怎么来拓展，我也刚开始看是学，还没整明白**

### 1.1 PAT工具的说明文档
首先是安装，安装就不做太多说明了，前面给的有网址，大家去加载一路next就ok，需要说明的是，Mac和Unix系统得下载一个MONO的工具包，就不多说了。下载完了之后，打开是这样的

![image](https://github.com/Gaoshiguo/Model-Checker-Introduction-of-PAT-PRISM-/blob/master/IMAGE/2.png)

初始是英文版的，在view栏的language选项里可以调成中文版的

然后接下来是介绍其几个模块的，*编辑器、模拟器以及验证*，这些可以看看。我们主要关注一些语法及模型的搭建

### 1.2 CSP模型
