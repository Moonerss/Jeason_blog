---
title: "浅识apply家族"
date: 2024-02-23T19:46:13+08:00
lastmod: 2024-02-23T19:46:13+08:00
author: "Jeason"
categories:
  - R
tags:
  - R
description: "介绍R语言apply家族函数的相关应用"
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

### apply函数  

apply函数是最常用的代替for循环的函数。apply函数可以对矩阵、数据框、数组(二维、多维)，按行或列进行循环计算，对子元素进行迭代，并把子元素以参数传递的形式给自定义的FUN函数中，并以返回计算结果。  

#### 函数定义  


```r
apply(X, MARGIN, FUN, ...)
```

#### 参数列表  

`X`：矩阵、数据框、数组  

`MARGIN`：按行计算或按按列计算，1表示按行，2表示按列  

`FUN`: 自定义函数  

`…`: 自定义参数  

#### 应用  

1. 对于矩阵行求和    

   
   ```r
   x<-matrix(1:12,ncol=3)
   apply(x,1,sum)
   ```
   
   ```
   ## [1] 15 18 21 24
   ```

2. 自定义函数的使用  

   
   ```r
   #  生成data.frame  
   x <- cbind(x1 = 3, x2 = c(4:1, 2:5)); x
   ```
   
   ```
   ##      x1 x2
   ## [1,]  3  4
   ## [2,]  3  3
   ## [3,]  3  2
   ## [4,]  3  1
   ## [5,]  3  2
   ## [6,]  3  3
   ## [7,]  3  4
   ## [8,]  3  5
   ```
   
   ```r
   # 自定义函数myFUN，第一个参数x为数据
   # 第二、三个参数为自定义参数，可以通过apply的'...'进行传入。
   myFUN<- function(x, c1, c2) {
      c(sum(x[c1],1), mean(x[c2])) 
   }
   # 把数据框按行做循环，每行分别传递给myFUN函数，设置c1,c2对应myFUN的第二、三个参数
   apply(x,1,myFUN,c1='x1',c2=c('x1','x2'))
   ```
   
   ```
   ##      [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8]
   ## [1,]  4.0    4  4.0    4  4.0    4  4.0    4
   ## [2,]  3.5    3  2.5    2  2.5    3  3.5    4
   ```

### lapply函数  

lapply函数是一个最基础循环操作函数之一。主要操作对象是list、data.frame，并返回和X长度同样的list结构作为结果集。  

#### 函数定义  


```r
lapply(X, FUN, ...)
```

#### 参数列表

`X`：矩阵、数据框、数组
`FUN`: 自定义函数
`…`: 自定义参数  

#### 应用  

1. 对list进行操作  

   
   ```r
   x <- list(a = c(1:3), b = c(4:6), d = c(10:20))
   lapply(x, sum)
   ```
   
   ```
   ## $a
   ## [1] 6
   ## 
   ## $b
   ## [1] 15
   ## 
   ## $d
   ## [1] 165
   ```

2. 对data.frame进行操作  

   
   ```r
   x <- data.frame(x1=3, x2=c(2:1,4:5))
   lapply(x, sum)
   ```
   
   ```
   ## $x1
   ## [1] 12
   ## 
   ## $x2
   ## [1] 12
   ```

### sapply函数  

sapply函数是一个简化版的lapply，sapply增加了2个参数simplify和USE.NAMES，主要就是让输出看起来更友好，返回值为向量，而不是list对象。  

#### 函数定义  


```r
sapply(X, FUN, ..., simplify=TRUE, USE.NAMES = TRUE)
```

#### 参数列表  

`X`：矩阵、数据框、数组
`FUN`: 自定义函数
`…`: 自定义参数
`simplify`：是否数组化，当值是array时，输出结果按数组进行分组
`USE.NAMES`：如果X为字符串，TRUE设置字符串为数据名，FALSE不设置  

#### 应用  

对上面的数据框`x`进行操作：  


```r
sapply(x, sum)
```

```
## x1 x2 
## 12 12
```

```r
class(lapply(x, sum)) # lapply返回list，sapply返回vector
```

```
## [1] "list"
```

```r
class(sapply(x, sum))
```

```
## [1] "numeric"
```

### vapply函数  

vapply类似于sapply，提供了FUN.VALUE参数，用来控制返回值的行名。  

#### 函数定义  


```r
vapply(X, FUN, FUN.VALUE, ..., USE.NAMES = TRUE)
```

#### 参数列表  

`X`：矩阵、数据框、数组
`FUN`: 自定义函数
`FUN.VALUE`：定义返回值的行名
`…`: 自定义参数
`USE.NAMES`：如果X为字符串，TRUE设置字符串为数据名，FALSE不设置  

#### 应用  


```r
x <- data.frame(cbind(x1=3, x2=c(2:1,4:5)))
vapply(x,cumsum,FUN.VALUE = c('a'=0,'b'=0,'c'=0,'d'=0)) #注意FUN.VALUE的用法
```

```
##   x1 x2
## a  3  2
## b  6  3
## c  9  7
## d 12 12
```

### mapply函数  

相当于多参数版本的 sapply。第一次计算传入各组向量的第一个元素到FUN，进行结算得到结果；第二次传入各组向量的第二个元素，得到结果；第三次传入各组向量的第三个元素…以此类推。  

#### 函数定义  


```r
mapply(FUN, ..., MoreArgs = NULL, SIMPLIFY = TRUE,USE.NAMES = TRUE)
```

#### 参数列表  

主要使用的参数有三个
`FUN`: 自定义函数
`FUN.VALUE`：定义返回值的行名
`…`: 自定义参数  

#### 应用  


```r
mapply(rep, 1:4, 4:1) #每一组参数都是对应的
```

```
## [[1]]
## [1] 1 1 1 1
## 
## [[2]]
## [1] 2 2 2
## 
## [[3]]
## [1] 3 3
## 
## [[4]]
## [1] 4
```

### tapply函数  

tapply用于分组的循环计算，通过INDEX参数可以把数据集X进行分组，相当于group by的操作。  

#### 函数定义  


```r
tapply(X, INDEX, FUN = NULL, ..., simplify = TRUE)
```

#### 参数列表  

`X`：向量
`INDEX`: 分组索引
`FUN`: 自定义函数
`…`: 接收多个数据
`simplify`：是否数组化，当值array时，输出结果按数组进行分组  

#### 应用  


```r
# 对样本分组计数
tapply(warpbreaks$breaks, warpbreaks[,-1], sum)
```

```
##     tension
## wool   L   M   H
##    A 401 216 221
##    B 254 259 169
```
