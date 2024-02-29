---
title: "归一化与标准化"
date: 2021-10-11T14:33:21+08:00
lastmod: 2024-02-29T14:33:21+08:00
author: "Jeason"
categories:
  - Stats
tags:
  - Stats
description: "本文主要介绍了数据预处理中的归一化和标准化之间的异同以及使用的场景"
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

### 归一化  

归一化（`normalization`）特指将**数据进行缩放到一个固定的区间内**。通常来讲，这个区间是[0，1]，但是在实际的情况中，这个区间可以是[-1,1]。总之符合使用的需求。  

通过归⼀化，我们可以**消除不同量纲下的数据对最终结果的影响**。例如，我们通过身高（单位：⽶）和体重（单位：公 ⽄）来衡量两个⼈之间的差异，两个⼈的的体重相差 20 公⽄，⾝⾼相差 0.1 ⽶，因此在这样的量纲下衡量这两个⼈的差 异时，体重的差异会把⾝⾼的差异遮盖掉，但这往往不是我们想要的结果。但通例如我们假设体重的最小值和最⼤值分 别为 0 和 200 公⽄，⾝⾼的最小值和最⼤值分别为 0 和 2 ⽶，因此归⼀化后体重和⾝⾼的差距变为 0.1 和 0.05，因此通过归⼀下则可以避免这样的问题的出现。  

#### 归一化目的  

1. 把数据缩放到特定区间内

   主要是为了数据处理方便提出来的，把数据映射到0～1范围之内处理，更加便捷快速，应该归到数字信号处理范畴之内。

2. 数据无量纲化

   归一化是一种简化计算的方式，即将有量纲的表达式，经过变换，化为无量纲的表达式，成为纯量，降低了数据的复杂度。

#### 归一化计算  

针对一般的情况，归一化的结果可以表示为：
$$
x'=\frac{x-x_{min}}{x_{max}-x_{min}}
$$
其中$x_{min}$表示$x$的最小值，$x_{max}$表示$x$中的最大值

这种归一化方法一般叫做**min-max归一化**，归一化之后数据的区间为[0,1]

另外，使用均值进行归一化操作的方式叫做**均值归一化**，归一化之后数据的区间为[-1,1]

公式表示为：
$$
x'=\frac{x-x_{mean}}{x_{max}-x_{min}}
$$  

#### 归一化的应用 

归一化应用于在对数据范围有严格要求的情况下。在不涉及距离度量和数据整体协方差分布的情况下可以使用归一化。如果数据有异常值，一定不要用归一化。  

### 标准化  

标准化(`Standardization`)特指将数据进行变化，使其符合均值为0，标准差为1的分布。**注意：这里的分布并非一定是正态分布**  

> 标准化会改变数据的均值、标准差，也就是说改变了原来的分布，但是分布的类型依然是不变的，原来是什么类型，标准化之后依然是什么类型。标准化后的分布并不一定就是标准正态，完全取决于原始数据的分布类型  

#### 标准化目的 

1. 消除量纲影响，利于不同级别数据的比较，得到合理的结果  

2. 加速建模的建立与求解，提高运算速度

#### 标准化的计算  

最常见的标准化方法是标准差标准化：
$$
x'=\frac{x-{\mu}}{\sigma}
$$
标准化过后的数据符合取值范围无固定区间

#### 标准化的应用

数据的标准化广泛的运用于数据建模和机器学习当中。在分类、聚类算法中，需要使用距离来度量相似性的时候、或者使用PCA技术进行降维的时候，标准化表现更好  

### 归一化和标准化的差异与关联  

#### 差异  

1. 结果差异  

+ Normalization会严格的限定变换后数据的范围，如[0,1],[-1,1]。Standardization变换后无严格的区间限制，只要求其均值为0，标准差为1。  
+ Normalization对数据的缩放比例仅仅和极值有关，Standardization受到极值影响比较大。  

#### 关联  

Normalization和Standardization在本质上都是对数据的线性变换，两者都不会改变原始的数据排布顺序  

### 标准化和归一化的原因及用途  

1. 统计建模中，如回归模型，自变量X XX的量纲不一致导致了回归系数无法直接解读或者错误解读；需要将X XX都处理到统一量纲下，这样才有可比性；
2. 机器学习和统计学中有很多地方要用到“距离”的计算，比如PCA，KNN，kmeans等等，不同维度量纲不同可能会导致距离的计算依赖于量纲较大的那些特征而得到不合理的结果；
3. 参数估计时使用梯度下降，在使用梯度下降的方法求解最优化问题时， 归一化/标准化后可以加快梯度下降的求解速度，即提升模型的收敛速度。  

### 标准化和归一化的使用场景

1. 如果你对处理后的数据范围有严格要求，需要使用归一化；
2. 如果数据不稳定，存在过大或过小的值（也可以说是异常值），不能使用归一化操作；
3. 标准化是机器学习中最常用的方式，如果无法判断可以直接使用标准化；
4. 对于分类、聚类算法，需要使用距离来度量相似性的时候、或者使用PCA技术进行降维的时候，标准化表现更好；
5. 在不涉及距离度量、协方差计算的时候，可以使用归一化方法。  

### 不需要使用标准化和归一化的场景

1. 数据不同特征之间存在较大的差异，数据量纲不一致时需要预处理，反之则不需要；
2. 当要使用的模型不涉及到距离和标准差异的衡量时，可以不使用标准化和归一化操作；
3. 使用概率模型时可以不进行操作，因为它不关注数据本身，关注的是出现概率。


### Reference

+ [标准化和归一化，请勿混为一谈，透彻理解数据变换](https://blog.csdn.net/weixin_36604953/article/details/102652160)
+ [标准化和归一化什么区别？--知乎](https://www.zhihu.com/question/20467170) 
+ [R 语言数据科学导论](https://ds-r.leovan.tech/feature-engineering/feature-engineering.pdf)
+ [机器学习中常见的几种归一化方法以及原因](https://blog.csdn.net/UESTC_C2_403/article/details/75804617) 
+ [Normalization vs Standardization — Quantitative analysis](https://towardsdatascience.com/normalization-vs-standardization-quantitative-analysis-a91e8a79cebf)
+ [Feature Scaling for Machine Learning: Understanding the Difference Between Normalization vs. Standardization](https://www.analyticsvidhya.com/blog/2020/04/feature-scaling-machine-learning-normalization-standardization/)


