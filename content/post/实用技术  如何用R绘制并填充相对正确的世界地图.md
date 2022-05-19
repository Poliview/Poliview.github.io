---
title: 实用技术|如何用R绘制并填充相对正确的世界地图
date: 
author: 政文观止
tags: ['#地图绘制', '#数据可视化', '#实用技术']
---
# 实用技术|如何用R绘制并填充相对正确的世界地图


收录于合集

#地图绘制 6 个

#数据可视化 15 个

#实用技术 19 个

近几年来，随着负笈海外特别是美国的政治学博士陆续学成回国，R逐渐在高校从事政治学量化研究的师生群体中流行起来，形成了与Stata并驾齐驱的局面。与需要付费购买才能使用的商业统计软件Stata不同，R可以终身免费使用。不仅如此，作为开源、共享、共建的社群，R具备非常强大的功能，在各项运算上都不逊于Stata，在可视化方面则更胜一筹。具体来说，R中各种各样的包（package）为我们处理包括地理信息（Geographic
Information）数据在内的各种数据的可视化提供了便利。换言之，即便我们在没有或者不会ArcGIS/QGIS等专业地理信息处理软件的情况下，只要我们稍微学习一下R，拿到相关的文件，就能顺利地处理这些地理信息数据。由于笔者最近一直在接触地理信息可视化的相关知识，对这一领域逐渐起了兴趣，心想在阅读实操的时候不如做一做笔记总结经验。因此，今天的推送与以往的文献编译、方法论衡、学人小传及新刊快递等栏目有所不同，它是一篇为了方便作者向国内期刊投稿而介绍的实用技术小文。在这篇小文章里，我们将同读者朋友分享如何用R绘制并填充相对正确的世界地图。

  

目前网上已经有大量现成的教授大家如何用R绘制各种类型地图的教程，小到绘制一个街区地图，大到绘制全世界的地图教程应有尽有。但本文强调的是“相对正确的世界地图”。看到这里，读者朋友可能会好奇我们为什么还要在“正确”二字前加上“相对”的字样，是多此一举还是故弄玄虚，其实都不然。这是因为即便用R绘制并输出比较正确的地图也需要结合相关软件进行进一步的修改。

  

众所周知，地图是一国国家主权和领土的象征，使用完整正确的地图也是国内科研工作者应尽的责任与义务，因此我们在对待地图绘制时应当十分审慎。令人欣慰的是，近年来，不少技术贴的作者在谈及如何绘制中国地图时，都格外强调使用正确、完整的中国地图的重要性，其在帖中绘制的中国地图也基本正确。之所以说这些制图是“基本正确”是因为按照现行地图出版的标准，用R直接绘制并输出的地图与通过自然资源部审查正式出版的地图还有一些细微的区别。如下图所示，尽管中国与塔吉克斯坦已经正式完成了划界工作，但由于现行出版的纸质或电子地图均未做改动，因此该交界处（帕米尔高原东部倾斜坡）仍应使用未定国界线表示：  

![](/images/211/2.jpeg)

当然这不是特别严重的问题，在用R输出高DPI（每英寸点数）的中国地图图片后，可以请美工编辑或排版编辑利用Adobe
Illustrator、Photoshop等专业制图软件将其修改成虚线即可。  

  

但是在不少介绍绘制世界地图的帖子中，我们会发现其中使用的地图存在大量的错误。持允而论，出现这些错误并不能被归咎到撰写相关技术贴的作者，他们本身是出于好心与大家分享技术经验。但由于他们使用的R包或者shape文件通常都是国外作者开发并且在国际上通行的包，因此难免会出现与国内出版规定不相符合甚至错误的情形。举例来说：这些包或者shp文件中多以0°经线（本初子午线）为地图中心，不符合国内出版的关于世界地图的规定，如果进行投影变换也可能导致出现错误（如在绘制地图上出现冗余线条和色块，这并非是投影变换不对所致，而是原作者在制作shape文件中已经锁定了投影）。当然这些问题倒不是最严重的，最严重的错误是在国家边界划分上的错误。其中R中常用的包和世界地图文件中关于中国地图部分的最严重错误是违背国际社会普遍承认的“一个中国”原则、将中国大陆和中国台湾地区分别作为地位相同的不同“政治实体”对待。其次，这些地图包普遍按照非法的“麦克马洪线”将中国藏南地区划入印度，有些还按照非法的“约翰逊线”将中国阿克赛钦划入印度以及缺乏南海诸岛等等。此外，在这些地图包中，关于印度和巴基斯坦之间的争议地区——克什米尔地区以及埃及和苏丹间的争议地区等地也未按照正确的形式进行绘制。这些错误使得作者绘制出的地图在投稿时极有可能达不到自然资源部审核通过的标准，最后导致精彩的可视化部分被删除，甚为可惜。

  

为了进一步说明，我们以R中常用的地图包maps为例，用红色圆圈标识出一些常见的错误划界，它们分别是：在图A中，红圈1处表示地图按照非法的“麦克马洪线”将中国藏南地区划入印度；在图B中，红圈2处指地图未以未定国界线标识中国与塔吉克斯坦之间的边界；红圈3处表示地图未标识出印度和巴基斯坦之间的争议地区——克什米尔地区；而在图C中，红圈4表示地图未标识出埃及与苏丹之间的争议地区——哈拉伊卜三角区。

![](/images/211/3.jpeg)

这些错误只是其中的一些代表性错误，此外还有别的错误。由于此类地图在绘制输出后需要修改的地方很多，程序十分繁琐，非常不适合向国内的期刊杂志投稿。为了方便大家在今后的日常科研学习过程中使用，我们花了好几天时间搜遍了各大网站，甚至还付费购买了CSDN的一个月会员，终于找到了一个相对适合的shape文件。与前述的种种shape文件相比，我们找到的这一shape文件相对而言需要修改的地方要少很多（缺少的南海诸岛和九段线，需要单独加上，这可以利用ggplot制图层层叠加的原理进行补全），而且基本符合作者向国内期刊投稿时使用东经150°经线为中心的制图要求，但美中不足的是缺乏南极洲。

  

既然找到了shape文件，那么我们该怎么来处理它呢？事实上，shape文件在本质上就是储存地理空间信息的多边形，我们可以在R中安装并加载可以读取多边形的sf包，这样我们可以很容易看到shape文件中储存的数据结构和形式。本文使用的世界地图shape文件的结构并不复杂，它由name,
childNum以及geometry三类变量分别储存对应的信息。其中name和geometry两个字段特别重要，因为如果我们想要在世界地图上实现相关数据的可视化，使用的数据中的国家名称字段一定要和本地图中的name相匹配，这需要大家在使用时仔细校对，而geometry字段中则储存了多边形的信息，反映在地图中就是各个国家的形状。

  

接下来我们以世界银行发布的世界治理指数（Worldwide Governance
Index）为例，在校对各个国家名的基础上，绘制世界地图并按照Regulatory
Quality（各国制定与执行促进私营部门发展能力）的指标进行颜色填充。在此，我们略去了设置工作路径的步骤，读者朋友可以根据平时的工作习惯自行设置，加载并读取数据的代码如下所示：

  *   *   *   *   *   *   *   *   *   *   *   *   * 

    
    
    library(tidyverse)library(sf)library(readxl)# 读取以东经150度经线为中心的世界地图shape文件WorldMap <- st_read("worldmap")# 读取南海诸岛shape文件SCSislands <- st_read("SCSislands")# 读取九段线shape文件NineDashLine <- st_read("NineDashLine")# 读取世界银行发布的各国制定与执行促进私营部门发展能力的评估数据库WGIdata <- read_excel("ex.xlsx", col_types = c("text", "numeric"))# 将上述数据与世界地图数据按照相同的“name”字段合并worlddata <- left_join(WorldMap, WGIdata, by = "name")

看到这里，有些读者可能会疑惑，既然我们已经加载了集ggplot2和readr等包为一体的tidyverse包，那么为什么我们不将世行的数据存储成体积更小、读取更快的csv格式文件呢？事实上，正如下图所示，由于该版本地图shape数据中name字段中一些国家的名字采用的是非英文，在对照国家名时如果用csv格式保存世行数据可能会导致特殊字符遗失，从而导致R中数据框中的国家名匹配不上，进而在绘制出来的地图上可能显示不少处深灰色的状态，这意味着缺失相关数据。

![](/images/211/4.jpeg) 绘制地图部分的代码如下所示：

  *   *   *   *   *   *   *   *   *   *   *   *   *   *   *   *   * 

    
    
    ggplot() +  geom_sf(data = worlddata, aes(fill = estimate), colour = "#525252",size=0.3)+ #绘制世界地图并按照estimate字段中的数据上色  geom_sf(data = SCSislands, colour = "#525252")+ #绘制南海诸岛  geom_sf(data = NineDashLine, color = "#525252", size=0.3)+ #绘制九段线  coord_sf() + #设置投影方式，本处为默认投影  scale_fill_gradient(low="#deebf7", high="#4292c6")+ #设置蓝色的颜色渐变  guides(fill = guide_legend(title="评估指数"))+ #修改图例标签，如在Mac中可能需要设置字体  ggtitle("2018年世界各国制定与执行促进私营部门发展能力的评估")+ #给图片命名，同样在Mac中可能需要设置字体  theme(panel.grid = element_blank(),        panel.background = element_rect(fill = "Aliceblue"), #将背景填充为Aliceblue色模仿大海        axis.text = element_blank(),        axis.ticks = element_blank(),        axis.title = element_blank(),        plot.title = element_text(size = 10, hjust = 0.5),        plot.margin = unit(c(-0.5, -0.5, -0.5, -0.5), "inches"), #设置图像输出的边缘距离        legend.position = "bottom")  #设置图例位于图像正下方  ggsave("2018年世界各国制定与执行促进私营部门发展能力的评估.jpg",dpi=1000,width=13,height=12)

![](/images/211/5.jpeg)由于这里的shape文件采用的是普通圆柱投影，看起来会比较扁。但是我们对比自然资源部提供的带审图号的例图可以发现此时输出的图像除了没有南极洲外，在总体上已经达到了和例图比较接近的程度。但是请注意，千万不要以为此时已经大功告成了，这里直接输出的地图只是在总体上接近，还远远没有达到通过审核的标准。

![](/images/211/6.png)为什么呢？这就呼应了我们反复强调的“相对正确”一词。请仔细阅读自然资源部例图中关于国家边界的划线，特别需要注意朝鲜半岛、克什米尔地区、中东以及非洲大陆国家边界的划线（通常都是未定界限）。这些细致的工作就需要在Adobe
Illustrator等专业软件中完成了。

  

同样，我们还可以对投影视角进行变换，下图显示的是亚洲视角：

  *   *   *   *   *   *   *   *   *   *   *   *   *   *   *   *   * 

    
    
    ggplot() +  geom_sf(data = worlddata,aes(fill = estimate),colour = "#525252",size=0.3)+  geom_sf(data = SCSislands,colour = "#525252")+  geom_sf(data = NineDashLine,color = "#525252",size=0.3)+  scale_fill_gradient(low="#deebf7",high="#4292c6")+  coord_sf(crs= "+proj=ortho +lat_0=20 +lon_0=90")+  guides(fill=guide_legend(title='评估指数'))+  ggtitle("2018年世界各国制定与执行促进私营部门发展能力的评估（亚洲视角）")+  theme(panel.grid = element_blank(),        panel.background = element_rect(fill = "Aliceblue"),        axis.text = element_blank(),        axis.ticks = element_blank(),        axis.title = element_blank(),        plot.title = element_text(size = 15, hjust = 0.5),        legend.position = "right")ggsave("2018年世界各国制定与执行促进私营部门发展能力的评估（亚洲视角）.jpg",dpi=1000,width=15,height=12)

![](/images/211/7.jpeg)

而这行代码显示的则是北极视角：  

  *   *   *   *   *   *   *   *   *   *   *   *   *   *   *   *   * 

    
    
    ggplot() +  geom_sf(data = worlddata,aes(fill = estimate),colour = "#525252",size=0.3)+  geom_sf(data = SCSislands,colour = "#525252")+  geom_sf(data = NineDashLine,color = "#525252",size=0.3)+  coord_sf(crs = "+proj=laea +lat_0=52 +lon_0=10 +x_0=4321000 +y_0=3210000 +ellps=GRS80 +units=m +no_defs ")+ # 变换投影视角为北极视角  scale_fill_gradient(low="#deebf7",high="#4292c6")+  guides(fill=guide_legend(title='评估指数'))+  ggtitle("2018年世界各国制定与执行促进私营部门发展能力的评估（北极视角）")+  theme(panel.grid = element_blank(),        panel.background = element_rect(fill = "Aliceblue"),        axis.text = element_blank(),        axis.ticks = element_blank(),        axis.title = element_blank(),        plot.title = element_text(size = 15, hjust = 0.5, vjust=0.5),        legend.position = "right")ggsave("2018年世界各国制定与执行促进私营部门发展能力的评估（北极视角）.jpg",dpi=1000,width=15,height=12)

![](/images/211/8.jpeg)

  

总而言之，在R中实现地图的数据可视化是一项非常有趣的过程。这一过程虽然十分繁琐，但是可以为我们在从事科研写作的过程中实现地理信息的可视化提供便利，从而增强读者的阅读体验。需要坦诚的是，作为非地理专业的学生，我们对地图绘制的理解是十分粗浅的，同时我们找到的这个shape文件肯定不是最好的，我们欢迎各位使用者与我们分享更适合国内投稿的shape文件，也虚心接受任何善意的批评。

  

最后，我们将本文的相关地图shape文件以及案例数据文件都存入网盘，感兴趣的读者朋友可以点击“ **阅读原文**
”进行下载（提取码为wv07）。未来我们还将与大家分享如何绘制并填充以0°经线为中心的世界地图、当代中国地图和中国历史地图的经验，欢迎继续关注我们。

  

 **参考文献：**

1\. World map and Map projections,https://semba-
blog.netlify.app/01/26/2020/world-map-and-map-projections/

2\. How to plot a full hemispheric orthographic projection in R,

https://gis.stackexchange.com/questions/115818/how-to-plot-a-full-hemispheric-
orthographic-projection-in-r  

3\.
《以中国为中心Echart世界地图实例》，https://download.csdn.net/download/liming1016/12718849  

4\. 自然资源部地图技术审查中心：《地图表示错误实例》，https://www.zrzyst.cn/dtbscwsl/index.jhtml

  

感谢周源在本文写作过程中给予的建议与帮助。

‍

撰文：杨端程 审读：陆屹洲 编辑：康张城

【政文观止Poliview】系头条号签约作者

  

![](/images/211/9.jpeg)

  

