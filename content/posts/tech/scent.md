---
title: "SCENT包Debug"
date: 2020-11-20T20:53:51+08:00
lastmod: 2024-02-28T20:53:51+08:00
author: "Jeason"
categories: 
  - SingleCell
tags:
  - SingleCell
description: "本文记录了在使用`SCENT`包进行分析时出现的错误，以及解决方法。"
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

使用`SCENT`包进行分析时出现如下错误：  

```sh
OpenBLAS blas_thread_init: pthread_create: Resource temporarily unavailable
OpenBLAS blas_thread_init: RLIMIT_NPROC 1024 current, 2583728 max
OpenBLAS blas_thread_init: pthread_create: Resource temporarily unavailable
OpenBLAS blas_thread_init: RLIMIT_NPROC 1024 current, 2583728 max
```

## 解决过程  

+ BLAS

  BLAS（Basic Linear Algebra Subprograms，基础线性代数程序集）是一个应用程序接口，里面拥有大量已经编写好的关于线性代数运算的程序。OpenBLAS是一个开源的BLAS，可以用于相关的运算。 

+ 相关问题

  搜索到别人在运用其他程序时出现类似的问题：

  + [https://github.com/davidemms/OrthoFinder/issues/68](https://github.com/davidemms/OrthoFinder/issues/68)
  + [https://www.jianshu.com/p/981f67e5fb82](https://www.jianshu.com/p/981f67e5fb82)

## 解决方法  

在运行程序或者进入R环境之前设置环境变量：

```sh
export OPENBLAS_NUM_THREADS=1
```

> 原理：在其他程序调用BLAS进行线性运算时，可能创建了过多的BLAS线程，造成环境过负荷，超过系统限制，在此设置调用线程为1即可。  

