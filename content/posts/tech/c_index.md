---
title: "C-index计算的相关问题"
date: 2024-02-29T15:08:18+08:00
lastmod: 2024-02-29T15:08:18+08:00
author: "Jeason"
categories: 
  - Stats
tags: 
  - Stats
description: ""
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


## 背景

在进行Cox模型的预后能力评估比较时，C-index作为一个重要的评价指标被广泛的使用。目前有多个R包
可以实现C-index的计算，如：Hmisc，survcomp等。但是，当进行计算时，发现一个问题，使用不同的
R包计算出的C-index值可能不同，虽然这一结果可能是由于对于[tied risk][1]事件的处理手段不同所造成的；
但是为了尽量获得可靠的C-index，决定采用Harrell本人开发的Hmisc包进行计算

## 计算过程

```r
> library(survival)
> library(Hmisc)
> data(colon)
> d <- colon
> surv <- y <- Surv(d $time, 1-(d$ status))
> m1 <- coxph(surv ~ rx+sex, data=d)
> m2 <- coxph(surv ~ rx+sex+age, data=d)
> m1.cindex <- rcorr.cens(-predict(m1), surv)
#m1的C-index和置信区间
> m1.cindex
       C Index            Dxy           S.D.              n        missing     uncensored 
  5.266315e-01   5.326294e-02   2.170980e-02   1.858000e+03   0.000000e+00   9.380000e+02 
Relevant Pairs     Concordant      Uncertain 
  9.379880e+05   4.939740e+05   2.510688e+06

> CIndex <- round(m1.cindex['C Index'], digits = 4)
[1] 0.5266 
> Lower <- round(CIndex - 1.96*m1.cindex['S.D.']/2, digits = 4)
[1] 0.5053
> Upper <- round(CIndex + 1.96*m1.cindex['S.D.']/2, digits = 4)
[1] 0.5479

# C-index比较的p值
> (r <- rcorrp.cens(predict(m1), predict(m2), surv))
         Dxy        S.D. x1 more concordant x2 more concordant         n 
     6.069800e-02    2.345218e-02    5.282221e-01    4.675241e-01    1.858000e+03 
       missing     uncensored   Relevant Pairs     Uncertain        C X1 
     0.000000e+00    9.380000e+02    9.379880e+05    2.510688e+06    4.733685e-01 
         C X2       Dxy X1       Dxy X2 
     4.614846e-01   -5.326294e-02   -7.703084e-02 
> (p.value <- round(2*(1 - pnorm(r[['Dxy']] / r[['S.D.']])), digits=4))
[1] 0.0096 
```

## 参考

https://stats.stackovernet.com/cn/q/30724  

http://zeegroom.com/
