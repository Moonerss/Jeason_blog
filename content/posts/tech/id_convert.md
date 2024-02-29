---
title: "基因ID转换相关知识"
date: 2020-05-29T20:04:34+08:00
lastmod: 2024-02-28T20:04:34+08:00
author: "Jeason"
categories:
  - NGS
tags:
  - NGS
description: "这里介绍了与基因ID及其互相转换相关的知识"
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

## Ensembl ID 辨识  

> + 例如ENSG00000279928.1中，**ENS**是**固定字符**，表示它是属于Ensembl数据库的。默认物种是人，如果是小鼠就要用**ENSMUS**开头，关于物种代码：http://www.ensembl.org/info/genome/stable_ids/index.html  
> + **G**表示：这个ID指向一个基因；**E**指向Exon；**FM**指向 protein family；**GT**指向gene tree；**P**指向protein；R指向regulary feature；T指向transcript  
> + 后面11位数字部分如00000279928 表示基因真正的编号  
> + 小数点后不一定每个都有，**表示这个ID在数据库中做了几次变更**，比如.1做了1次变更，在分析时需要去除  

## Ensembl ID 与 Symbol对应关系  

### 一对多  

在转换时会存在一个`Ensembl ID`对应多个`Symbol`的现象。此时，为了保证ID转换的准确性，需要对这些ID进行专门的转换，可以更换注释资源进行操作  

> 不同的注释资源之间会有差别，有时并不可靠  

### 多对一  

同样，会存在一些`Ensembl ID`对应同一个`Symbol`，此时，除了其中一个真正对应的外，其他的可以叫做`haplotypic regions`。像这样的ID需要检查ENSG对应的染色体位置进行辨别  

> **Haplotyptic regions** are defined by the Genome Reference Consortium (GRC) and are visible on all their genome assemblies for human, mouse, and zebrafish.  They can also be called ‘**alternate locus**’, ‘**partial chromosomes**’, and ‘**alternate alleles**’ or ‘**assembly exceptions**’.  
> These regions can have small sequence differences, contain **different gene structures or different genes entirely**, or contain **genes in a different order compared to the reference genome assembly**.  

## 注释途径  

* [x] bitr  
* [x] biomart  






