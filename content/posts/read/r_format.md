---
title: "R语言代码规范"
date: 2018-11-05T16:22:51+08:00
lastmod: 2024-02-29T16:22:51+08:00
author: "Jeason"
categories:
  - R
tags:
  - R
description: "本文主要介绍了在书写R语言代码时的标准规范"
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

R语言是一门主要用于统计计算和绘图的高级编程语言. 在使用R时，好的代码编写和注释规范，可以提高代码的可读性和工作效率. 然而，什么样的规范更有利于我们自己使用和第三方解读？在此，建议使用《来自Google的R语言编码风格指南》一文中推荐的代码写作风格，这个规则是由Google的R用户群体协同设计而成.

### 1. 表示和命名  

#### 文件命名

文件名应以 .R (大写) 结尾, 文件名本身要有意义.
如: <font color = "green">predict_ad_revenue.R</font>

#### 标识符命名

在标识符中不要使用下划线 ( _ ) 或连字符 ( - ). 标识符应根据如下惯例命名. 变量名应使用点 (.) 分隔所有的小写字母或单词; 函数名首字母大写, 不用点分隔 (所含单词首字母大写); 常数命名规则同函数, 但需使用一个 k 开头.

- 变量命名
  正例: <font color = "green">avg.clicks</font>
  反例: <font color = "red">avg_Clicks , avgClicks</font>
- FunctionName
  正例: <font color = "green">CalculateAvgClicks</font>
  反例: <font color = "red">calculate_avg_clicks , calculateAvgClicks</font>
  函数命名应为动词或动词性短语.

*例外: 当创建一个含类 (class) 属性的对象时, 函数名 (也是constructor) 和类名 (class) 应当匹配 (例如, lm).*

- <font color = "green">kConstantName</font>

### 2. 语法

#### 单行长度

最大单行长度为 80 个字符.

#### 缩进

使用两个空格来缩进代码. 永远不要使用制表符或混合使用二者.
*例外: 当括号内发生折行时, 所折行与括号内的第一个字符对齐.*

#### 空白

在所有二元操作符 (=, +, -, <-, 等等) 的两侧加上空格.
*例外: 在函数调用中传递参数时 = 两边的空格可加可不加*.

不可在逗号前加空格, 逗号后总须加空格.

例如:  

```R
tabPrior <- table(df[df$daysFromOpt < 0, "campaignid"])
        total <- sum(x[, 1])
        total <- sum(x[1, ])
```

在前括号前加一个空格, 函数调用时除外.
例如：  

```R
if (debug)
```

多加空格 (即, 在行内使用多于一个空格) 也是可以的, 如果这样做能够改善等号或箭头 (<-) 的对齐效果.  

```R
plot(x    = xCoord,
     y    = dataMat[, makeColName(metric, ptiles[1], "roiOpt")],
     ylim = ylim,
     xlab = "dates",
     ylab = metric,
     main = (paste(metric, " for 3 samples ", sep="")))
```

不要向圆括号或方括号中的代码两侧加入空格.
*例外: 逗号后总须加空格.*

以下写法就是错误的：  

```R
if ( debug )  # debug 的两边不要加空格
x[1,]  # 需要在逗号后加一个空格
```

#### 花括号

**前括号永远不应该独占一行; 后括号应当总是独占一行**. 您可以在代码块只含单个语句时省略花括号; 但在处理这类单个语句时, 您必须 前后一致地 要么全部使用花括号, 或者全部不用花括号.

例如：  

```R
if (is.null(ylim)) {
  ylim <- c(0, 0.06)
}
```

或者：  

```R
if (is.null(ylim))
  ylim <- c(0, 0.06)
```

总在新起的一行开始书写代码块的主体。  

#### 赋值  

使用 `<-` 进行赋值, 不用` =` 赋值  

#### 分号  

不要以分号结束一行, 也不要利用分号在同一行放多于一个命令. (分号是毫无必要的, 并且为了与其他Google编码风格指南保持一致, 此处同样略去.)  

### 3. 代码组织  

#### 总体布局和顺序  

如果所有人都以相同顺序安排代码内容, 我们就可以更加轻松快速地阅读并理解他人的脚本了.  

1. 版权声明注释
2. 作者信息注释
3. 文件描述注释, 包括程序的用途, 输入和输出
4. <font color = "green">source()</font> 和 <font color = "green">library()</font> 语句
5. 函数定义
6. 要执行的语句, 如果有的话 (例如, <font color = "green">print, plot</font>)
   单元测试应在另一个名为 <font color = "green">原始的文件名_unittest.R</font> 的独立文件中进行  

#### 注释准则  

注释您的代码. 整行注释应以 `# `后接一个空格开始.  

行内短注释应在代码后接两个空格, `#`, 再接一个空格.  

```R
# Create histogram of frequency of campaigns by pct budget spent.
    hist(df$pctSpent,
    breaks = "scott",  # method for choosing number of buckets
    main   = "Histogram: fraction budget spent by campaignid",
    xlab   = "Fraction of budget spent",
    ylab   = "Frequency (count of campaignids)")
```

#### 函数的定义和调用  

**函数定义应首先列出无默认值的参数, 然后再列出有默认值的参数.**  

函数定义和函数调用中, 允许每行写多个参数; 折行只允许在赋值语句外进行.  

例如：  

```R
PredictCTR <- function(query, property, numDays,
    showPlot = TRUE)
```

理想情况下, 单元测试应该充当函数调用的样例 (对于包中的程序来说).  

#### 函数文档  

函数在定义行下方都应当紧接一个注释区. 这些注释应当由如下内容组成: 此函数的一句话描述; 此函数的参数列表, 用 Args: 表示, 对每个参数的描述 (包括数据类型); 以及对于返回值的描述, 以 Returns: 表示. 这些注释应当描述得足够充分, 这样调用者无须阅读函数中的任何代码即可使用此函数.  

#### 示例函数  

```R
CalculateSampleCovariance <- function(x, y, verbose = TRUE) {
        # Computes the sample covariance between two vectors.
        #
        # Args:
        #   x: One of two vectors whose sample covariance is to be calculated.
        #   y: The other vector. x and y must have the same length, greater than one,
        #      with no missing values.
        #   verbose: If TRUE, prints sample covariance; if not, not. Default is TRUE.
        #
        # Returns:
        #   The sample covariance between x and y.
        n <- length(x)
        # Error handling
        if (n <= 1 || n != length(y)) {
        stop("Arguments x and y have invalid lengths: ",
        length(x), " and ", length(y), ".")
        }
        if (TRUE %in% is.na(x) || TRUE %in% is.na(y)) {
        stop(" Arguments x and y must not have missing values.")
        }
        covariance <- var(x, y)
        if (verbose)
        cat("Covariance = ", round(covariance, 4), ".\n", sep = "")
        return(covariance)
        }
```

#### TODO书写风格

编码时通篇使用一种一致的风格来书写TODO.
<font color = "green">TODO(您的用户名): 所要采取行动的明确描述</font>  

-----------------------------------------------------

**Reference：**  

[来自Google的R语言编码风格指南](https://nanx.me/rstyle/)  

