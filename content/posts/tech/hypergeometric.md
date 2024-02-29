---
title: "超几何分布和二项分布"
date: 2019-02-21T14:21:53+08:00
lastmod: 2024-02-29T14:21:53+08:00
author: "Jeason"
categories:
  - Stats
tags:
  - Stats
description: "本文介绍了超几何分布和二项分布的原理和具体解释"
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


## 超几何分布  

**超几何分布**是统计学上一种离散概率分布。它描述了由有限个物件中抽出$n$个物件，成功抽出指定种类的物件的个数的概率（不归还）。  

**黑白球问题解释**：例如共有$N$个球，其中$m$个白球。超几何分布描述了在该$N$个球中抽出$n$个，其中$k$个是白球的机率：   
$$
f(k;n,m,N) = \frac{\binom{m} {k} \binom {N-m} {n-k}} {\binom {N} {n}}
$$
上式可如此理解：  

$\left( N \over n\right)$表示所有在$N$个样本中抽出$n$个，而抽出的结果不一样的数目。  

$\left(m \over k\right)$表示在$m$个样本中，抽出$k$个的方法数目。剩下来的样本都是及格的，而及格的样本有$N-m$个，剩下的抽法便有$\left({N-m} \over {n-k}\right)$种。  

> 若$n=1$，超几何分布还原为伯努利分布。若$N$接近 $\infty$，超几何分布可视为二项分布  

## 二项分布  

在概率论和统计学中，**二项分布**（英语：Binomial distribution）是**n个独立的是/非试验中成功的次数的离散概率分布**，其中每次试验的成功概率为$p$。这样的单次成功/失败试验又称为伯努利试验。实际上，当$n=1$时，二项分布就是伯努利分布。  

**具体解释：**  

假设进行$n$次独立实验，每次实验“成功”的概率为$p$，失败的概率为$1-p$，所有成功的次数X就是一个参数为$n$和$p$的二项随机变量.数学公式定义为：  
$$
p(k) = \binom {n} {k} * p^k * (1-p)^{n-k}
$$
二项分布公式基于伯努利分布得到，因为二项分布中每项实验都是独立的，因此每一次实验都是一次伯努利实验，在$n$次实验中，成功$k$次，排列方式有$\left( {n \over k} \right)$种，根据乘法原理，即可得到二项分布的公式。  

## 总结  

超几何分布相当于连续抽取$n$次成功的概率（不放回抽样），二项分布是重复抽$n$次成功的概率（放回抽样）。  



