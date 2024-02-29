---
title: "tryCatch抓取错误"
date: 2019-03-06T19:48:49+08:00
lastmod: 2024-02-28T19:48:49+08:00
author: "Jeason"
categories:
  - R
tags:
  - R
description: "这里介绍了在R语言中如何使用tryCatch函数抓取错误，并根据场景返回结果"
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

在使用R进行循环操作时，出现错误，为了对错误进行分析，使用`tryCatch()`函数抓取处理错误  

## 使用  

tryCatch常用的三种用法:  

### 抓取错误  

```r
tryCatch(libray(test), error = function(e){print("出现错误")} )
```

```
## [1] "出现错误"
```


释义：当加载`test`包时，出现错误，执行`error`函数  

> 这里很适合在循环中出现错误的情况，可以对错误的循环结果进行赋值  

### 抓取警告和错误  

```r
tryCatch(stop("test"), warning = function(w){print("出现警告")},
          finally={
            print("test")
          })
## Error in doTryCatch(return(expr), name, parentenv, handler) : test
## [1] "test"
```

释义：执行语句出错，打印出错误，并执行`finally`函数  

> 如果单独只有warning函数，则只是打印错误  

### 继续执行  

```r
tryCatch(a = 1, finally={print("我是个测试")})
## Error in tryCatchList(expr, classes, parentenv, handlers) : 
##   argument "expr" is missing, with no default
## [1] "我是个测试"
```

> `finally`函数无论错与对都会执行  

## 封装函数  

```r
log_calculator <- function(x){
    tryCatch(
        expr = {
            message(log(x))
            message("Successfully executed the log(x) call.")
        },
        error = function(e){
            message('Caught an error!')
            print(e)
        },
        warning = function(w){
            message('Caught an warning!')
            print(w)
        },
        finally = {
            message('All done, quitting.')
        }
    )    
}
```

测试结果如下：  

+ 如果`x`是有效数字，则`expr`和`finally`被执行：  

```r
log_calculator(-10)
```

```
## Caught an warning!
```

```
## <simpleWarning in log(x): NaNs produced>
```

```
## All done, quitting.
```

+ 如果`x`不是有效数字，则`expr`会报错，`warning`和`finally`被执行：  

```r
log_calculator("10")
```

```
## Caught an error!
```

```
## <simpleError in log(x): non-numeric argument to mathematical function>
```

```
## All done, quitting.
```


