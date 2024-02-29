---
title: "ggplot2 | 小部件参考"
date: 2020-09-22T15:55:09+08:00
lastmod: 2024-02-29T15:55:09+08:00
author: "Jeason"
categories:
  - ggplot2
  - R
tags: 
  - ggplot2
description: "本文主要介绍了在使用ggplot2绘图时会修改到的部分小细节"
weight:
slug: "" # 使用 slug属性 来作为当前文章的有效 url 的末尾部分 ；如果没有提供 slug 则使用 title 代替。
draft: false # 是否为草稿
comments: true # 本页面是否显示评论
reward: true # 打赏
mermaid: false #是否开启mermaid
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示路径
cover:
    image: "https://source.unsplash.com/random/400x200?code" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
    hidden: true         # 隐藏cover图片，但是不在格式化数据中隐藏
    hiddenInList: false   # 在列表页和文章内容页隐藏cover图片
    hiddenInSingle: true # 只在文章内容页隐藏cover图片
---

### 添加原件  

+ 添加竖线或者横线  

  ```R
  geom_abline(mapping = NULL, data = NULL, ..., slope, intercept, na.rm = FALSE, show.legend = NA)
  geom_hline(mapping = NULL, data = NULL, ..., yintercept, na.rm = FALSE, show.legend = NA)
  geom_vline(mapping = NULL, data = NULL, ..., xintercept, na.rm = FALSE, show.legend = NA)
  ```

  三个函数的作用分别是，geom_abline添加斜线， geom_hline添加水平线，geom_vline添加垂直线  

  参数slope 表示斜率  intercept表示截距  

  参数yintercept  表示y轴截距或直线所在位置  

  参数xintercept  表示x轴截距或直线所在位置  
  
+ 添加线段  

  ```R
  geom_segment(aes(x = x1, y = y1, xend = x2, yend = y2)
  ```

  x, y控制线段起始位置，xend，yend控制线段终止位置  

### 背景布局  

+ 去掉网格  

  ```R
  theme(panel.grid = element_blank())
  ```

+ 使用与plot相似的背景  

  ```R
  theme_classic()
  ```

+ x，y轴转换  

  ```R
  coord_flip()
  ```

+ 去掉图形和坐标轴间隙  

  ```R
  scale_y_continuous(expand = c(0,0))//这个可以去掉与X轴间隙
  scale_x_continuous(expand = c(0,0))//这个可以去掉与Y轴间隙
  ```

### 添加阴影  

```r
geom_ribbon(mapping = NULL, data = NULL, stat = "identity", position = "identity", ..., na.rm = FALSE, orientation = NA,
show.legend = NA, inherit.aes = TRUE,outline.type = "both")

geom_area(mapping = NULL, data = NULL, stat = "identity", position = "stack", na.rm = FALSE, orientation = NA, show.legend = NA, inherit.aes = TRUE, ..., outline.type = "upper")
```

### 添加箭头和线段  

添加箭头或线段需要使用`annotate('segment')`  

```R
annotate('segment', x=#, xend=#, y=#, yend=#, arrow=arrow())
```

- 没有添加`arrow`参数时是绘制线段  
- 如果添加了`arrow`参数，需要提前加载`library(grid)`包才能调用`arrow()`函数  

### 画图理解  

+ 使用ggplot绘图，图层是最重要的，要弄清参数是否写进了图层里。只有在图层中的对象才能进行后续的修改和操作。  

### 标题居中  

```R
theme(plot.title = element_text(hjust = 0.5))
```

