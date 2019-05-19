
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
CSP的全称是communicating sequential processes（CSP）也叫通信顺序进程模型，最初是一种基于通信的协议进程。

在CSP模型中，先定义一个模型，定义语法为：`//@@Model Name@@ ` 全局常量和全局变量的语法规则为：`#define max 5;` 和`var knight`,常量还支持枚举类型，变量还支持数组类型。CSP模型中有一种比较特殊的概念叫通道（Channels），我感觉就像是缓冲区，用来存储一些信息，多半是数字。我们可以通过以下的语法来定义一个通道`channel c 5;`,代表的意思是定义了一个通道，通道名为c，通道最多可存储的规模是5个。
```
1.c!a.b -> P                          -- channel output

2.c?x.y -> P                         -- channel input

3.c?1 -> P                            -- channel input with expected value

4.c?[x+y+9>10]x.y-> P      -- channel input with guard expression

```
1代表的意思是a,b都是表达式，若通道的缓存区未满，将a,b的表达式的复合值存储在缓存区中以FIFO的规则输出，若通道已满，则等待
2代表的意思是先检索队列顶的元素，若缓存区为非空，将分配给x和y，然后执行p
3代表的意思是判断缓冲区顶的元素是否为1，若为1就执行p，否则等待
4代表的的意思是在x+y+9<10的条件下，检索缓冲区是否已满，未满则将分配给x和y，否则等待

## 二、PRISM随机模型检验工具
下面是随机模型检验工具PRISM的官方网址：[PRISM官网](http://www.prismmodelchecker.org/)
我们主要通过官方的说明文档来学习如何通过这个工具来帮助我们进行随机模型检验的研究，[官方文档](http://www.prismmodelchecker.org/manual/)
