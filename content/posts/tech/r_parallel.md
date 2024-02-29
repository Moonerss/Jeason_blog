---
title: "R并行运算"
date: 2019-05-14T20:55:34+08:00
lastmod: 2024-02-28T20:55:34+08:00
author: "Jeason"
category: 
  - R
tags: 
  - R
description: "本文介绍了在R语言中如何实现并行运算的方法"
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

## R的并行运算  

R作为主要流行的统计分析软件之一，其处理数据一般都是单线程的。随着数据量的增大，R运行的时间会大大增加；为了充分发挥计算机的性能和进一步提升R运行速度，使用并行运算不失为一种有效的手段。  

简单的说R的并行化计算其实没有改变其整个并行环境，而是先启动N个附属进程，然后将数据分割成N块在N个核上进行运行，等待附属进程结束返回结果。与单线程运算相比，会大大节省运算时间  

### parallel  

R内置了parallel包（R version > 2.14），强化了R的并行计算能力。parallel包的思路和lapply函数很相似，都是将输入数据分割、计算、整合结果。只不过并行计算是用到了不同的cpu内核来运算。两个核心的函数为`mclapply`和`parlapply`。  

```R
## lapply
system.time({
res <- lapply(1:5000000, function(x) x+1);
});

user  system elapsed
21.42    1.74   25.70

## parlapply

# load parallel
library(parallel)

# check numrber of cores
clnum<-detectCores() 

#设置参与并行的CPU核数目
cl <- makeCluster(getOption("cl.cores", clnum));

#运行
system.time({
res <- parLapply(cl, 1:5000000,  function(x) x + 1)
});
user system elapsed
6.54 0.34 19.95

#关闭并行计算
stopCluster(cl);

## mclapply ==> mclapply适用于类Unix系统的操作
mc <- getOption("mc.cores", 3)
system.time({
res <- mclapply(1:5000000, function(x){x+1}, mc.cores = mc);
});
user system elapsed
6.657 0.500 7.181
```

### furrr  

`furrr`是`future`和`purrr`包的结合，提供了一系列的并行操作适用于`*map`家族函数  

```R
library(furrr)
library(tictoc)

# This should take 6 seconds in total running sequentially
plan(sequential)
tic()
nothingness <- future_map(c(2, 2, 2), ~Sys.sleep(.x))
toc()
#> 6.08 sec elapsed
# This should take ~2 seconds running in parallel, with a little overhead
plan(multiprocess)
tic()
nothingness <- future_map(c(2, 2, 2), ~Sys.sleep(.x))
toc()
#> 2.212 sec elapsed
```

与`parallel`相比更喜欢`furrr`的傻瓜式操作，且其与其他R包的交互使用更为方便。   

### 其他

其他一些并行相关的R包：  
[future](https://github.com/HenrikBengtsson/future) : 强大的分布式处理并行包  
[future.apply](https://github.com/HenrikBengtsson/future.apply) : 基于`future`包开发的`*apply`家族函数并行包  
