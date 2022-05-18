

收录于合集

#数据可视化 15 个

#实用技术 19 个

从小编的直观感受来看，Stata这几年过得并不好。

一方面，SPSS的界面越来越友善，操作难度一降再降，让零基础的用户爱不释手，甚至反过来帮助许多人学会了统计。另一方面，R语言的魅力逐渐在不同群体中扩散，强大到逆天的功能（尤其是数据可视化）让越来越多的文青变成了码农。相比之下，Stata似乎总是不那么让人满意，初学者对它望洋兴叹，老司机则对它不屑一顾。幸运的是，Stata有着良好的开放性，大神们源源不断地供应着package，而我们则可以简单粗暴地拿来弥补自己的手残。今天，小编就为大家介绍两个实用而方便的package，学会之后记得分享哦。（本文操作软件为Stata
14.0，示例文件为Stata自带的auto.dta，下文功能均可通过findit-install安装。）

**Outreg2**

 **多个模型排排坐**

曾经的小编还不会做回归，但paper中漂亮的回归表格还是让人心驰神往。特别是那些小星星，多好看啊！

![](/images/640/2.png)

  

后来，小编终于学会了Stata，兴冲冲地按下了reg，却发现：

 **这是什么？？？？**

 **我的星星呢！！！**

 **我的星星呢！！！**

![](/images/640/3.png)

  

**我一定用了假的reg！**

![](/images/640/4.png)

  

后来，小编经过沉思，

自认为找到了真相：

他们的系数和星星是

一个一个贴上去的！！

  

![](/images/640/5.png)

然而，他们只是装了outreg2而已。

有了这个杀器，回归结果就直观多了。举个栗子，我们对auto文件中的若干变量进行了一顿毫无逻辑的回归（仅作参考，请勿模仿），并调用了outreg2命令：

clear

sysuse auto

reg price mpg rep78 headroom trunk weight length turndisplacement gear_ratio
foreign

outreg2 using reg, word

就可以得到如下结果：

![](/images/640/6.png)

可见，用星星表示的显著性、括号内的标准误和表格下的R方都已经乖乖排好了，再也不需要复制粘贴这种naïve的操作了。

当然，单个模型还体现不出outreg2的优势，下面展示并列输出的命令：

clear

sysuse auto

reg price mpg rep78 headroom

est store model1

reg price mpg rep78 headroom trunk weight length

est store model2

reg price mpg rep78 headroom trunk weight length turndisplacement gear_ratio
foreign

est store model3

outreg2 [model1 model2 model3] using regs, word replace

得到如下结果:

![](/images/640/7.png)

可见，上方的回归表格非常完整，不需要修改就可以直接插入自己的paper中。此外，生成的文件名（本例中为“reg”和“regs”）和文件格式（本例中为word）都可以根据需要来变更。

 ****

**Coefplot**

 **回归系数可视化**

当小编终于学会上述的操作时，发现paper里好像又不流行回归表了。大牛们会画出一些有点有线的图。类似下图：

![](/images/640/8.png)

  

这些图是怎么来的？又是什么含义呢？小编同样通过一个栗子来为大家说明。我们再次对auto文件进行毫无逻辑的回归，并调用coefplot命令：

clear

sysuse auto

reg price mpg rep78 headroom trunk weight length turn

coefplot, drop(_cons) xline(0)

结果如下：

![](/images/640/9.png)

  

根据package中的说明信息，我们了解到，上图中蓝色的点为回归系数（未标准化），横线则为回归系数的置信区间。根据回归分析中βj=0的虚无假设，回归系数（含置信区间）在0点右侧表示影响为正，在0点左侧则表示影响为负，如果回归系数的置信区间穿越了0点，那么这个自变量对因变量的影响就不显著。可见，在本例中，大部分变量都不显著。

与outreg2类似，coefplot也可以并列输出：

reg price mpg rep78 headroom trunk weight length turn ifdisplacement<200

est store low

reg price mpg rep78 headroom trunk weight length turn if displacement>=200

est store high

coefplot high low,drop(_cons) xline(0)

 ****

![](/images/640/10.png)

****  

以上就是小编为大家带来的介绍。这是政文观止创号以来第一篇纯技术贴，欢迎各位读者多多拍砖，也希望读者朋友能和政观分享你们学习计量工具过程中的心得与体会。

  

![]()

政文观止编辑部

![赞赏二维码]() **微信扫一扫赞赏作者** __赞赏

已喜欢，[对作者说句悄悄话](javascript:;)

取消 __

#### 发送给作者

发送

最多40字，当前共字

[](javascript:;) 人赞赏

上一页 [1](javascript:;)/3 下一页

长按二维码向我转账

![赞赏二维码]()

受苹果公司新规定影响，微信 iOS 版的赞赏功能被关闭，可通过二维码转账支持公众号。

