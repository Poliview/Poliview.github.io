---
title: 方法论衡|黑科技：用R语言画时间进度表
date: 2019-10-23 09:04:30
author: 政文观止
tags: 
---


收录于合集

  

10月，保研的厮杀刚刚落幕，紧张的申请季又来临了。各位读者是否和小编一样对着手里的proposal抓耳挠腮、心急如焚？又是否和小编一样完全不知道研究设计的timetable如何规划？不要慌！

  

本期政文观止Poliview送你一份黑科技，用R语言画出的时间进度表一定会让你的申请如虎添翼！

  

首先，我们手工录入数据，把自己预期的研究进度分项列出来。为了方便R语言识别，我们多加了一列Ranks来和目标任务（Tasks）匹配起来。此处需要注意填写时间单元格时的格式。

  

  

![](/images/380/2.png)

  

下一步，我们将所有单元格堆砌成tidy数据的要求。（小伙伴们可自行探索R语言中堆砌数据的功能，不会的朋友就和小编一起人工堆砌吧）

  

![](/images/380/3.png)

  

接下来，用RStudio导入数据，调整一下Time变量的格式，检查一下有无错漏。看起来没有什么问题，那么接着载入需要用的ggplot2和scales包。

  

![](/images/380/4.png)

  

![](/images/380/5.png)

  

接下来开始基本的作图。在时间进度表里，我们需要用直线来表示时间跨度，同时在末尾加一个箭头表示时间的方向。首先，我们指定映射关系和坐标图层。

  

![](/images/380/6.png)

  

![](/images/380/7.png)

  

在此基础上，根据Ranks分组连线，形成基本的时间跨度。

  

![](/images/380/8.png)

  

![](/images/380/9.png)

  

此处需要一个箭头，于是更新一下代码。

  

![](/images/380/10.png)

  

![](/images/380/11.png)

  

修饰一下细节。大功告成！

事实上，在上述操作中，Tasks和Type基本属于冗余信息。有兴趣的小伙伴可以自行探索如何利用这两列来简化代码或修饰图形。

  

![](/images/380/12.png)

  

![](/images/380/13.png)

  

点击“在看”，再点击“阅读原文”可以拿到代码哦。

撰文：陆屹洲

审读：杨端程  

编辑：康张城

  

![](/images/380/14.jpeg)![](/images/380/15.jpeg)

  

