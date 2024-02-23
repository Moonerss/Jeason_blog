---
title: "R语言高级程序设计"
date: 2019-05-27
lastmod: 2024-02-22
author: "Jeason"
categories:
  - R
tags:
  - R
description: "介绍一下《R语言高级程序设计》中的一些容易忽略的点"
weight:
comments: true # 本页面是否显示评论
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

## 数据结构  

R 语言的**基础数据结构**可以按照**维度**来划分(1 维、 2 维...n 维)；也可以按照它们**所包含的数据类型是否相同**来划分。 这样就产生了五种数据类型，它们是数据分析中最常用的：  

|       |  Homogeneous  | Heterogeneous |
| :---: | :-----------: | :-----------: |
|  1d   | Atomic vector |     List      |
|  2d   |    Matrix     |  Data frame   |
|  nd   |     Array     |

### 向量（vector）  

向量包括：原子向量 （Atomic vector） 和列表 （List）。兩者都属于**一维数据**， 两者的区别在于： **原子向量中的所有元素都必须是相同的类型；而列表中的元素可以是不同的类型**  

> `is.vector()`并不能测试一个对象是不是向量。 相反，仅当对象是除了名字以外，不包含其它属性时，它才返回 `TRUE`。 所以，请使用 `is.atomic(x) || is.list(x)` 来测试一个对象是不是向量。  

向量具有三个共同属性：  

1. **类型**， `typeof()`，它是什么  
2. **长度**， `length()`，它包含有多少元素  
3. **属性**， `attributes()`，额外的任意元数据  

#### 原子向量 (Atomic vector)  

+ 原子向量类型：**逻辑类型** (logical)、 **整数类型** (integer)、 **双精度数值类型** (double)、 **字符类型** (character)、 复数类型 (complex)、 raw类型。  
+ 缺失值用 `NA` 表示，是一个长度为1的逻辑向量，NA可根据所处整体环境强制转换成正确的类型，或者使用 `NA_real_`(双精度浮点数向量)、 `NA_integer_` 和 `NA_character_` 来创建一系列确定类型的 `NA`  
+ `typeof` 确定具体的数据类型，`is`开头的函数（如：`is.integer`）来确定是否属于某一特定类型  
+ `is.atomic()` 是检测原子向量最通用的  
+ 强制转换：一个原子向量中的所有元素都必须是相同的类型。 所以，当你试图合并不同类型的数据时，将向最灵活的类型进行强制转换。 以灵活程度排序，从小到大依次为：逻辑、 整数、双精度浮点数和字符。  
+ 如果转换遇到歧义，则需要使用 `as.character()`、 `as.double()`、 `as.integer()`或者 `as.logical()`等函数，进行明确的强制转换。  

#### 列表(List)  

c()可以将几个向量合并成一个。 如果原子向量和列表同时存在，那么在合并之前， c()会将原子向量强制转换成列表  

```R
x <- list(list(1, 2), c(3, 4))
y <- c(list(1, 2), c(3, 4))
str(x)
#> List of 2
#> $ :List of 2
#> ..$ : num 1
#> ..$ : num 2
#> $ : num [1:2] 3 4
str(y)
#> List of 4
#> $ : num 1
#> $ : num 2
#> $ : num 3
#> $ : num 4
```

列表用来建立 R 语言中的许多更加复杂的数据结构。 比如， **数据框**和其他复杂对象  

```R
is.list(mtcars)
#> [1] TRUE
```

> 这里说明一点：data.frame是比list更为复杂的数据结构  

### 属性(Attributes)  

所有的对象都可以拥有任意多个附加属性， 附加属性用来存取与该对象相关的元数据。 属性可以看做是已命名的列表(带有不重复的名字)。 属性可以使用 `attr()`函数一个一个的访问，也可以使用 attributes()函数一次性访问。  

```R
y <- 1:10
attr(y, "my_attribute") <- "This is a vector"
attr(y, "alpha") <- "This is a vector"
attr(y, "my_attribute")
#> [1] "This is a vector"
attributes(y)
#> $my_attribute
#> [1] "This is a vector"
#> 
#> $alpha
#> [1] "This is a vector"
attr(y, "my_attribute") <- "alpha"
attributes(y)
#> $my_attribute
#> [1] "alpha"
#> 
#> $alpha
#> [1] "This is a vector"
```

`structure()` 函数返回一个带有属性的对象， 也可以用来修改属性  

```R
structure(y, alpha = "alpha")
#>  [1]  1  2  3  4  5  6  7  8  9 10
#> attr(,"my_attribute")
#> [1] "alpha"
#> attr(,"alpha")
#> [1] "alpha"
```

默认情况下，当修改向量时，数据的大多数属性会丢失，但有三种重要的属性不会丢失：**名字**(name)，**维度**(dimension)，**类**(class)  

> 三个特殊属性需要使用特定函数`names(x)`、 `class(x)`和 `dim(x)` 访问，而不是 `attr(x, "names")`、 `attr(x, "class")` 和 `attr(x, "dim")`  

### 数据框(data frame)  

数据框是**由等长向量构成的列表**。 它也是二维结构，所以它具有**矩阵**和**列表**双重属性。也就是说，数据框拥有 `names()`、 `colnames()`和 `rownames()`，尽管 `names()`和 `colnames()`对数据框来说是一样的。 数据框的 `length()`是列表的长度，所以和 `ncol()`相同； `nrow()`则得到行数。  

由于数据框是一个包含向量的列表，所以数据框的某个是列表类型是有可能的：  

```R
df <- data.frame(x = 1:3)
df$y <- list(1:2, 1:3, 1:4)
df
#> x y
#> 1 1 1, 2
#> 2 2 1, 2, 3
#> 3 3 1, 2, 3, 4
```

> 当把列表传入 `data.frame()`函数时，该函数将试图把列表的每一个元素都放到单独的一列中。一种绕开的方法是使用 `I()`函数，它使得 `data.frame()`把列表看成一个整体单元  

### R中的对象  

#### S3对象  

S3 对象是由原子向量、数组和列表组成的，所以你可以使用上面描述的技术对 S3 对象进行取子集操作。 你可以通过 `str()`函数获得的它们的结构信息。  

#### S4对象  

对 S4 对象来说，有另外两种取子集操作符: `@`(相当于`$`)和 `slot()`(相当于`[[`)。 `@`比`$`更加严格，如果槽(slot)不存在，那么它会返回错误。  


--------------------
[Advanced R](http://adv-r.had.co.nz/)

