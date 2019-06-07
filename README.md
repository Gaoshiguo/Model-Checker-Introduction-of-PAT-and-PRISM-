
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

首先是PRISM模型检验工具的下载，[下载](http://www.prismmodelchecker.org/download.php)

下载后的PRISM工具打开后是这个样子的![image](https://github.com/Gaoshiguo/Model-Checker-Introduction-of-PAT-PRISM-/blob/master/IMAGE/3.png)

我们从语法开始详细讲解PRISM的用法，PRISM一共提供四种模型，所有的实验都必须基于这四种模型之上，分别是：DTMC（离散时间马尔可夫链）、CTMC（连续时间马尔可夫链）、MDP（马尔科夫决策过程）、PTA（概率有限机）。

在文件名开头必须声明使用哪种模型，语法如下：`mdp` or `dtmc` or `ctmc` or `pta` 代表你将使用一下几种模型其中的一种来进行实验，接下来开始建立model,每一个model都代表一个process，即每一个model代表一个进程，语法如下：`model modelname endmodel`，每一个model包括两部分的内容，变量variables和命令commands，变量描述系统可能存在的状态，命令描述系统的行为。

当前PRISM仅支持整型和布尔类型的变量，假设某个进程可能有的状态有三种，可以如下定义：`x : [0..2] init 0;`代表该进程的初始状态为0，可能的状态有0/1/2三种。
对于布尔类型的变量，语法如下：`b : bool init false; `代表该进程可能的状态有两种true or false

如果我们忽略赋予变量初始值，那么系统会默认将范围值的最低值设为变量的初始值，布尔类型默认为false。

同样需要提及的是，对于某些无法确定范围的变量，类似于在“近似模型检验”和“最快适应性检验”中，我们可以设置无范围变量，如
```
x : int;
y : int init 3;
```
在我们的第一个例子的代码中：
```
// Example 1
// Two process mutual exclusion

mdp

module M1

    x : [0..2] init 0;

    [] x=0 -> 0.8:(x'=0) + 0.2:(x'=1);
    [] x=1 & y!=2 -> (x'=2);
    [] x=2 -> 0.5:(x'=2) + 0.5:(x'=0);

endmodule

module M2

    y : [0..2] init 0;

    [] y=0 -> 0.8:(y'=0) + 0.2:(y'=1);
    [] y=1 & x!=2 -> (y'=2);
    [] y=2 -> 0.5:(y'=2) + 0.5:(y'=0);

endmodule
```
在model M1中，`[] x=0 -> 0.8:(x'=0) + 0.2:(x'=1);`"[]"代表这一行是命令，代表x有0.8的概率维持初始状态0，也有0.2的概率进入下一个状态1，

第二行命令`[] x=1 & y!=2 -> (x'=2);` 代表只有在x=1即x处于状态1并且y!=2即y不处于状态2时，x才会进入到状态2.

example 2:
```
dtmc
module  die
//local state
s:[0..7] init 0;
//value of die
d:[0..6] init 0;

[] s=0 -> 0.5 : (s'=1) + 0.5 : (s'=2);
[] s=1 -> 0.5 : (s'=3) + 0.5 : (s'=4);
[] s=2 -> 0.5 : (s'=5) + 0.5 : (s'=6);
[] s=3 -> 0.5 : (s'=1) + 0.5 : (s'=7)&(d'=1);
[] s=4 -> 0.5 : (s'=7) & (d'=2) + 0.5 : (s'=7) & (d'=3);
[] s=5 -> 0.5 : (s'=7) & (d'=4) + 0.5 : (s'=7) & (d'=5);
[] s=6 -> 0.5 : (s'=2) + 0.5 : (s'=7) & (d'=6);
[] s=7 -> (s'=7);
endmodule
```
我们在prism上先在module模块上建模，是一个色子模型，用字母s来代表程序进行的步数，用字母d代表色子的值，我们可以一张图来直观的理解色子模型。
![image](https://github.com/Gaoshiguo/Model-Checker-Introduction-of-PAT-and-PRISM-/blob/master/IMAGE/sz.png)

现在我们有了这个色子模型，接下里需要我们做的就是验证，为了验证，首先我们需要写一些性质规约（properties），有关性质规约的写法，参见Principles of Model Checking，由于我们使用的是DTMC模型，因此我们应当先建一个文件夹，文件名后缀为.pctl，比如我们想知道，该模型最终色子的值为5的概率为多少？

我们可以这样来写性质规约：

```
const int x;

P=? [ F s=7 & d=x ]
```
这两句话的含义是：定义一个常量x，p=？意思是：概率为多少？

[F S=7 & d=x] 这句话的含义是：最终程序到达状态7并且色子的值为x

合起来的意思是：最终程序到达状态7并且色子的值为x的概率是多少？

我们在PRISM中看看如何来进行验证？

首先我们需要在properties模块中导入一个pctl文件
![image](https://github.com/Gaoshiguo/Model-Checker-Introduction-of-PAT-and-PRISM-/blob/master/IMAGE/4.png)

![image](https://github.com/Gaoshiguo/Model-Checker-Introduction-of-PAT-and-PRISM-/blob/master/IMAGE/5.png)

然后我们需要输入一个值，这个值就代表着x的值，是我们希望验证色子为该值的概率

![image](https://github.com/Gaoshiguo/Model-Checker-Introduction-of-PAT-and-PRISM-/blob/master/IMAGE/7.png)

我们输入了一个色子不可能出现的值：10

接下来让我们看看概率是多少？

![image](https://github.com/Gaoshiguo/Model-Checker-Introduction-of-PAT-and-PRISM-/blob/master/IMAGE/8.png)

很明显，概率是0

我们又输入了一个正常的值：3

![image](https://github.com/Gaoshiguo/Model-Checker-Introduction-of-PAT-and-PRISM-/blob/master/IMAGE/9.png)

看看验证的结果如何？

![image](https://github.com/Gaoshiguo/Model-Checker-Introduction-of-PAT-and-PRISM-/blob/master/IMAGE/10.png)

*2.1 有关指令的一些解释

指令：commands，在PRISM中，主要是通过指令来描述一些条件的，指令标识符为“[]”

例如
`
：[] x=0 -> 0.8:(x'=0) + 0.2:(x'=1);
`
这段指令所代表的的意思就是：*如果x的值等于0，那么有0.8的概率x的值变成0，也有0.2的概率x的值变成1

再来看一条指令：`[] x=1 & y!=2 -> (x'=2); `,代表的意思是：如果x=1并且有y!=2,那么x的值变为2

`[] x1=0 & x2>0 & x2<10 -> 0.5:(x1'=1)&(x2'=x2+1) + 0.5:(x1'=2)&(x2'=x2-1);`  如果x1=0并且x2的值位于2-10之间，那么有0.5的概率x1变成1，x2=x2+1，有0.5概率x1变成2，x2=x2-1

需要注意的一条是：`[] x1=0 & x2=1 -> (x1'=2)&(x2'=x1) ` 经过这条指令后x2的值应该是0而不是2

*2.2 并行部分(Parallel Composition)

在prism中提供串行和并行多种操作，前提是对应相对应的模型，在模型的每一个状态，都会插入一系列的指令，执行哪条指令依赖于模型的类型，对于MDP模型，选择是随机的非确定性的

假设对于例1，有两个变量x和y，初始状态均为0，那么初始state(0,0),先插入两条指令：

`
[] x=0 -> 0.8:(x'=0) + 0.2:(x'=1);
`
`[] y=0 -> 0.8:(y'=0) + 0.2:(y'=1);
`
那么对于状态state(0,0),他的选择是非确定性的，会出现以下两种概率分布：

`
0.8:(0,0) + 0.2:(1,0) (module M1 moves)
0.8:(0,0) + 0.2:(0,1) (module M2 moves)
`
