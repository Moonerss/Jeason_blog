---
title: "计算R代码运行的时间"
date: 2021-10-04T20:07:03+08:00
lastmod: 2024-02-28T20:07:03+08:00
author: "Jeason"
categories:
  - R
tags: 
  - R
  - benchmark
description: "这里主要介绍了在R语言中如何实现计算运行时间的问题，以及不同方法的使用示例"
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

## 使用`Sys.time`  

可以根据程序运行起始和终止的时间差计算运行时间


```r
sleep_for_a_minute <- function() { Sys.sleep(10) }

start_time <- Sys.time()
sleep_for_a_minute()
end_time <- Sys.time()

end_time - start_time

## Time difference of 10.15231 secs
```

## 使用`tictoc`

`tictoc`包中的`tic`和`toc`函数和`Sys.time`函数的使用方法是类似的。但是`tictoc`提供了更加方便的使用形式，便于操作。

+ 可以对单个代码块的运行时间进行测试

```r
library(tictoc)

tic("sleeping")
print("falling asleep...")
sleep_for_a_minute()
print("...waking up")
toc()

## [1] "falling asleep..."
## [1] "...waking up"
## sleeping: 10.19 sec elapsed
```




+ 也可以同时对多段代码进行测试


```r
tic("total")
tic("data generation")
X <- matrix(rnorm(50000*1000), 50000, 1000)
b <- sample(1:1000, 1000)
y <- runif(1) + X %*% b + rnorm(50000)
toc()
tic("model fitting")
model <- lm(y ~ X)
toc()
toc()
## data generation: 3.83 sec elapsed
## model fitting: 46.69 sec elapsed
## total: 50.52 sec elapsed
```

## 使用`system.time`  

使用`system.time`函数直接包裹运行代码测试运行时间，简单快捷


```r
system.time({ sleep_for_a_minute() })

##    user  system elapsed 
##    0.00    0.00   10.14
```

其中的`elapsed`表示运行代码真正花费的时间

## 使用`rbenchmark`

`rbenchmark`是基于`system.time`的封装，提供了高效的用法进行专业的代码测试与比较


```r
library(rbenchmark)

benchmark("lm" = {
            X <- matrix(rnorm(1000), 100, 10)
            y <- X %*% sample(1:10, 10) + rnorm(100)
            b <- lm(y ~ X + 0)$coef
          },
          "pseudoinverse" = {
            X <- matrix(rnorm(1000), 100, 10)
            y <- X %*% sample(1:10, 10) + rnorm(100)
            b <- solve(t(X) %*% X) %*% t(X) %*% y
          },
          "linear system" = {
            X <- matrix(rnorm(1000), 100, 10)
            y <- X %*% sample(1:10, 10) + rnorm(100)
            b <- solve(t(X) %*% X, t(X) %*% y)
          },
          replications = 1000,
          columns = c("test", "replications", "elapsed",
                      "relative", "user.self", "sys.self"))

##            test replications elapsed relative user.self sys.self
## 3 linear system         1000    0.14    1.000      0.14        0
## 1            lm         1000    1.17    8.357      1.18        0
## 2 pseudoinverse         1000    0.19    1.357      0.19        0
```

结果中的`elapsed`、`user.self`、`sys.self`与`system.time`函数结果中表示的意义相同，`relative`表示函数运行的时间利用率

## 使用`microbenchmark`

`microbenchmark`和`rbenchmark`一样，可以用来比较多个代码之间的运行速率，而`microbenchmark`提供了更加人性化的绘图功能可以直观的比较运行时间的差异。


```r
library(microbenchmark)

set.seed(2017)
n <- 10000
p <- 100
X <- matrix(rnorm(n*p), n, p)
y <- X %*% rnorm(p) + rnorm(100)

check_for_equal_coefs <- function(values) {
  tol <- 1e-12
  max_error <- max(c(abs(values[[1]] - values[[2]]),
                     abs(values[[2]] - values[[3]]),
                     abs(values[[1]] - values[[3]])))
  max_error < tol
}

mbm <- microbenchmark("lm" = { b <- lm(y ~ X + 0)$coef },
               "pseudoinverse" = {
                 b <- solve(t(X) %*% X) %*% t(X) %*% y
               },
               "linear system" = {
                 b <- solve(t(X) %*% X, t(X) %*% y)
               },
               check = check_for_equal_coefs)

mbm

## Unit: milliseconds
##           expr      min       lq     mean   median       uq      max neval cld
##             lm 101.7592 114.4618 127.4006 122.7064 129.7980 430.7439   100  b 
##  pseudoinverse 173.2464 189.6997 201.1508 195.0444 207.6843 269.1587   100   c
##  linear system  97.0324 106.8963 113.9581 112.2040 117.1509 193.9128   100 a
```

```r
library(ggplot2)
autoplot(mbm)
```

![](https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/time_r/unnamed-chunk-7-1.png)

