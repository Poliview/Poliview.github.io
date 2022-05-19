---
title: 实用技术|regioncode:中国跨年份行政编码转换工具
date: 
author: 政文观止
tags: ['#实用技术', '#数据可视化']
---
# 实用技术|regioncode:中国跨年份行政编码转换工具


收录于合集

#实用技术 19 个

#数据可视化 15 个

当代中国行政区划的频繁变迁，给数据科学研究者使用跨年数据或数据可视化的过程当中带来了诸多不便，受到 Vincent Arel-Bundock
`countrycode`包的启发，清华大学政治学系胡悦副教授团队创建的`regioncode`包实现了中国各地级市1986-2019年间地区正式名称、常用名称和行政区划代码之间的无缝转换。`regioncode`0.1.1版本已通过CRAN和Github双平台向全球发布，供需要者免费使用。

本文旨在向各位读者介绍`regioncode`的功能和使用方法。

# / 安装 /

  * 最新发布版本：`install.packages("regioncode")`.
  * 最新开发版本：`remotes::install_github("sammo3182/regioncode")`.

# / 基本用法 /

我们结合哈佛大学政府系王裕华教授搜集的数据集`China’s Corruption Investigations
Dataset(CCID)`来介绍`regioncode`的使用方法。

在`regioncode`包中，我们将行政区划代码命名为`code`，地区的正式名称为`name`，它们的常用缩写为`sname`。当前版本支持任意一对之间的相互转换。为此，用户只需将名称的字符向量或地理编码的数字向量传递到函数中。在当前版本中，该函数可以产生三种类型的地级和省级输出：代码（`code`）、名称（`name`）和地区（`area`，如华北、东北、华南等）。只需在参数中指定他们想要的输出类型`convert_to`以及输入和输出的相应年份。例如，以下代码将`CCID`数据中的2019地理编码转换为1989版本：

    
    
    library(regioncode)  
    data("corruption")  
    # Original 2019 version  
    corruption$prefecture_id  
    ##  [1] 370100 321200 310117 420500 451300 431200 350300 511500 469021 420600  
    
    
    
    # 1999 version  
    regioncode(data_input = corruption$prefecture_id,   
               convert_to = "code", # default set  
               year_from = 2019,  
               year_to = 1989)  
    ##  [1] 370100 329001 310227 420500 422700 452200 433000 350300 512500 460025  
    ## [11] 420600  
    

请注意，如果一个区域最初在例如1989年进行了地理编码并包含在新区域中，则在2019年，此后将使用新区域地理编码。如果一个大的地方被分成几个区域，那么晚年的代码将按照区域数字地理编码的升序与第一个区域对齐。

通过将输出格式更改为`name`，可以轻松地将给定年份的代码或地区名称转换为另一年份的地区名称。`regioncode`自动检测输入格式，因此用户只需指定
_输出_
格式（连同输入和输出年份）即可获得所需的内容。在以下示例中，我们将`corruption`数据集中的地理编码变量转换为地区名称，将名称变量转换为另一年的代码和名称。

    
    
    # The original name  
    corruption$prefecture  
    ##  [1] "济南市" "泰州市" "松江区" "宜昌市" "来宾市" "怀化市" "莆田市" "宜宾市"  
    ##  [9] "定安县" "襄阳市"  
    
    
    
    # Codes to name  
    regioncode(data_input = corruption$prefecture_id,   
               convert_to = "name",  
               year_from = 2019,  
               year_to = 1989)  
    ##  [1] "济南市"   "泰州市"   "松江县"   "宜昌市"   "宜昌地区" "柳州地区"  
    ##  [7] "怀化地区" "莆田市"   "宜宾地区" "定安县"   "襄樊市"  
    
    
    
    # Name to codes of the same year  
    regioncode(data_input = corruption$prefecture,   
               convert_to = "code",  
               year_from = 2019,  
               year_to = 2019)  
    ##  [1] 370100 321200 310117 420500 451300 431200 350300 511500 469021 420600  
    
    
    
    # Name to name of a different year  
    regioncode(data_input = corruption$prefecture,   
               convert_to = "name",  
               year_from = 2019,  
               year_to = 1989)  
    ##  [1] "济南市"   "泰州市"   "松江县"   "宜昌市"   "宜昌地区" "柳州地区"  
    ##  [7] "怀化地区" "莆田市"   "宜宾地区" "定安县"   "襄樊市"  
    

# / 进阶应用 /

为进一步帮助用户使用更“杂乱”的数据和多样化的需求，`regioncode`提供五种特殊转换：不完整数据的转换、直辖市、地理区划和方言区域转换、拼音输出。当前版本还允许在省级转换。

## 不完整数据的转换

通常，数据代码在记录地理信息时可能会省略行政级别，例如使用“北京”而不是“北京市”。要完成此类数据的转换，需要指定`incomplete_name`参数。如果输入的数据不完整，用户应将参数设置为“from”；如果他们希望输出名称（when
`convert_to =
"name"`）不包含“city”或“prefecture”，他们可以将参数设置为“to”（参见下面的示例）；如果用户想要为输入和输出名称获得不完整的名称，`incomplete_name
= "both"`. 以上所有转换都可以持续数年。

    
    
    # Full, official names  
    corruption$prefecture  
    ##  [1] "济南市" "泰州市" "松江区" "宜昌市" "来宾市" "怀化市" "莆田市" "宜宾市"  
    ##  [9] "定安县" "襄阳市"  
    
    
    
    regioncode(data_input = corruption$prefecture,   
               convert_to = "name",  
               year_from = 2019,  
               year_to = 1989,  
               incomplete_name = "to")  
    ##  [1] "济南" "泰州" "松江" "宜昌" "柳州" "来宾" "怀化" "莆田" "宜宾" "定安"  
    ## [11] "襄樊" "襄阳"  
    

## 直辖市

直辖市（“直辖市”）在地理上是城市，但在行政上是省级的。因此，这些直辖市内的地区是县级的。不同的分析以不同的方式对待这些地区：一些与其他县平行的地区，而另一些则将整个自治市视为一个县。要处理后一种情况，请`regioncode`设置参数`zhixiashi`。设置参数时`TRUE`，将直辖市视为整个县，并使用其省级代码作为地理编码。

## 地理区划和方言区域

由于多种原因，中国各省级行政单位在地理空间上可以分为七个不同地区，它们如下所示：

地区| 省级行政单位  
---|---  
华北| 北京市、天津市、山西省、河北省、内蒙古自治区  
东北| 黑龙江省、吉林省、辽宁省  
华东| 上海市、江苏省、浙江省、安徽省、福建省、台湾省、江西省、山东省  
华中| 河南省、湖北省、湖南省  
华南| 广东省、海南省、广西壮族自治区、香港特别行政区、澳门特别行政区  
西南| 重庆市、四川省、贵州省、云南省、西藏自治区、云南省  
西北| 陕西省、甘肃省、青海省、宁夏回族自治区、新疆维吾尔自治区  
  
`regioncode` 提供了一种方法，将区域的代码和名称转换为这样的区域。

    
    
    regioncode(data_input = corruption$prefecture,   
               year_from = 2019,  
               year_to = 1989,   
               convert_to="area")  
    ##  [1] "华东" "华东" "华东" "华中" "华南" "华中" "华东" "西南" "华南" "华中"  
    

中国国土广袤，方言多样。这些方言可能会被一个省或省的多个县使用。不同省份的州也可能使用相同的方言。

`regioncode`允许用户获得县所属的语言区域作为输出。用户可以通过将参数设置`to_pinyin`为`dia_group`或来获得两个级别的语言区域，方言组和方言子组`dia_sub_group`。请注意，中国的语言分布过于复杂，无法在地级进行精确衡量。因此，语言区的输出`regioncode`仅供参考，而不是严格的语言研究。

    
    
    regioncode(data_input = corruption$prefecture,   
               year_from = 2019,  
               year_to = 1989,  
               to_dialect = "dia_group")  
    ##  [1] "冀鲁官话" "江淮官话" "吴语"     "西南官话" "西南官话" "湘语"      
    ##  [7] "莆仙区"   "西南官话" "琼文区"   "西南官话"  
    
    
    
    regioncode(data_input = corruption$prefecture,   
               year_from = 2019,  
               year_to = 1989,  
               to_dialect = "dia_sub_group")  
    ##  [1] "沧惠片-1"  "石济片-8"  "泰如片-1"  "太湖片-1"  "成渝片-3"  "成渝片-9"   
    ##  [7] "桂柳片-10" "岑江片-2"  "吉溆片-3"  "娄邵片-1"  "黔北片-3"  "长益片-3"   
    ## [13] "莆仙区-4"  "灌赤片-10" "府城片-1"  "鄂北片-10"  
    

## 汉语拼音

有些数据用汉语拼音而不是汉字来存储地区名称。`regioncode`可以通过设置`to_pinyin =
TRUE`参数获得拼音输出。该参数可应用于正式名称、不完整名称或地理区划和方言区域的输出。

    
    
    regioncode(data_input = corruption$prefecture,   
               year_from = 2019,  
               year_to = 1989,   
               convert_to="name",  
               to_pinyin=TRUE)  
    ##            济南市            泰州市            松江县            宜昌市   
    ##      "ji_nan_shi"    "tai_zhou_shi" "song_jiang_xian"    "yi_chang_shi"   
    ##          宜昌地区          柳州地区          怀化地区            莆田市   
    ##  "yi_chang_di_qu"  "liu_zhou_di_qu"  "huai_hua_di_qu"     "pu_tian_shi"   
    ##          宜宾地区            定安县            襄樊市   
    ##    "yi_bin_di_qu"    "ding_an_xian"   "xiang_fan_shi"  
    
    
    
    regioncode(data_input = corruption$prefecture,   
               year_from = 2019,  
               year_to = 1989,   
               convert_to="name",  
               incomplete_name = "to",  
               to_pinyin=TRUE)  
    ##         济南         泰州         松江         宜昌         柳州         来宾   
    ##     "ji_nan"   "tai_zhou" "song_jiang"   "yi_chang"   "liu_zhou"    "lai_bin"   
    ##         怀化         莆田         宜宾         定安         襄樊         襄阳   
    ##   "huai_hua"    "pu_tian"     "yi_bin"    "ding_an"  "xiang_fan" "xiang_yang"  
    
    
    
    regioncode(data_input = corruption$prefecture,   
               year_from = 2019,  
               year_to = 1989,   
               convert_to="area",  
               to_pinyin=TRUE)  
    ##        华东        华东        华东        华中        华南        华中   
    ##  "hua_dong"  "hua_dong"  "hua_dong" "hua_zhong"   "hua_nan" "hua_zhong"   
    ##        华东        西南        华南        华中   
    ##  "hua_dong"    "xi_nan"   "hua_nan" "hua_zhong"  
    

## 省份

`regioncode`不仅允许在地级市层级对数据进行转换而且还提供在省级转换的功能。通过设置参数`province =
TRUE`，用户可以完成所有省级的代号、名称、区域转换。（注意，在省级层面，语言转换只能是方言组。）而且，由于省份有固定的缩写，当输入是缩写，用户可以设置的`convert_to`参数`abbreTocode`，`abbreToname`或`abbreToarea`。当他们想要省级缩写输出时，只需设置`convert_to
= "abbre"`.

    
    
    regioncode(data_input = corruption$province_id,   
               convert_to = "codeToabbre",  
               year_from = 2019,  
               year_to = 1989,  
               province = TRUE)  
    ##  [1] "鲁" "苏" "沪" "鄂" "桂" "湘" "闽" "蜀" "琼" "鄂"  
    

# / 总结 /

由清华大学政治学系胡悦副教授团队开发的`regioncode`为中国行政区划代码、官方名称、地理区划和方言区域、省份缩写等之间的相互转换提供了一种便捷的方式。本文为用户提供了软件包功能的快速视图和简短的教程。

本软件包的进一步开发正在进行中，之后还会陆续推出县级市转换和更加丰富的功能。欢迎大家使用并提出改进意见（意见和建议可以在软件GitHub主页通过issue方式提出），更欢迎志趣相投的朋友一起加入研发。

撰文：孙宇飞 审读：杨端程 编辑：吴温泉

