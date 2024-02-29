---
title: "R的文件操作"
date: 2019-04-01T19:56:06+08:00
lastmod: 2024-02-28T19:56:06+08:00
author: "Jeason"
categories:
  - R
tags:
  - R
description: "这里介绍了在R语言中与文件和文件夹操作相关的函数及其使用的实例"
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


## 目录操作  

### 查看目录  

`getwd()` 列出当前工作路径  

`setwd()` 设置工作路径  

`list.files()`和`dir()`的用法相同，可以列出路径下的文件和目录  

```R
list.files(path =".", pattern = NULL, all.files = FALSE,

           full.names = FALSE, recursive =FALSE,

           ignore.case = FALSE, include.dirs =FALSE, no.. = FALSE)

dir(path =".", pattern = NULL, all.files = FALSE,

    full.names = FALSE, recursive = FALSE,

    ignore.case = FALSE, include.dirs = FALSE,no.. = FALSE)
```

函数`list.dirs()`只列出路径下所有目录,默认是递归进行的  

```R
list.dirs(path = ".", full.names = TRUE, recursive = TRUE)
```

参数介绍：

`path` 路径，如不填则默认为当前工作路径，即`getwd()`得到的路径；
`pattern` 正则表达式，若`pattern`不为`NULL`，返回文件（目录）名满足该正则表达式的文件（目录）；
`all.files `若为`FALSE`则不显示隐藏文件（目录），若为`TRUE`则返回所有文件（目录）；
`full.names` 若为`FALSE`则只返回文件（目录）名，若为`TRUE`则返回文件（目录）路径；
`recursive` 若为`FALSE`则只返回该路径的子级文件（目录），若为`TRUE`则递归返回所有子、孙文件（目录）；
`ignore.case` 若为`TRUE`则在匹配pattern时不区分大小写；
`include.dirs` 在recursive为TURE，即显示所有子、孙文件时，若`include.dirs`为FALSE则只列出最终端的文件名，而不列出中间层级的目录名；
`no.. `若为TRUE，则不显示“.”和“..”。  

### 查看目录的权限  

```R
> file.info(".")
  size isdir mode               mtime               ctime               atime exe
.    0  TRUE  555 2019-03-13 08:10:19 2018-10-13 17:14:40 2019-04-01 18:54:24  no
```

### 创建目录  

#### 创建单个目录

```R
> dir.create("create")
> list.dirs()
[1] "."        "./create" "./tmp"
```

#### 递归创建目录  

```R
> dir.create(path="a1/b2/c3",recursive = TRUE)
> list.dirs()
[1] "."          "./a1"       "./a1/b2"    "./a1/b2/c3" "./create"  "./tmp"
```

#### 检测目录是否存在  

```R
> file.exists("a")
[1] FALSE
```

#### 检查目录的权限  

```R
> df <- dir(file.path(R.home(), "bin"), full.names = T)
> file.access(df, mode = 0) == 0
  D:/Rcore/bin/config.sh       D:/Rcore/bin/R.exe D:/Rcore/bin/Rscript.exe 
                    TRUE                     TRUE                     TRUE 
        D:/Rcore/bin/x64 
                    TRUE
```

mode = 0, 检查文件或目录是否存在
mode = 1, 检查文件或目录是否可执行
mode = 2, 检查文件或目录是否可写
mode = 4, 检查文件或目录是否可读  

> file.access()返回的是逻辑值，file.info()返回的是详细的权限信息  

#### 修改目录权限  

```R
Sys.chmod(list.dirs("."), "777")
f <- list.files(".", all.files = TRUE, full.names = TRUE, recursive = TRUE)
Sys.chmod(f, (file.info(f)$mode | "664"))
```

#### 对目录重名  

```R
# 对tmp目录重命名
> file.rename("tmp", "tmp2")
[1] TRUE
```

#### 删除目录  

```R
# 删除tmp2目录
> unlink("tmp2", recursive = TRUE)
```

#### 其他功能  

```R
# 拼接目录字符串
> file.path("p1","p2","p3")
[1] "p1/p2/p3"
# 最底层子目录或文件名
> filename<-'C:/test/lalala.txt'
> basename(filename)
[1] "lalala.txt"
# 最底层子目录
> dirname(filename)r
[1] "C:/test"
# 转换扩展路径
> path.expand("~/foo")
[1] "/home/conan/foo"
```

## 常规文件操作  

cat(“file A\n”, file=”A”) #创建一个文件A，文件内容是’file A’,’\n’表示换行，这是一个很好的习惯

cat(“file B\n”, file=”B”) #创建一个文件B

file.append(“A”, “B”) #将文件B的内容附到A内容的后面，注意没有空行

file.create(“A”) #创建一个文件A, 注意会覆盖原来的文件

file.append(“A”, rep(“B”, 10)) #将文件B的内容复制10便，并先后附到文件A内容后

file.show(“A”) #新开工作窗口显示文件A的内容

file.copy(“A”, “C”) #复制文件A保存为C文件，同一个文件夹

dir.create(“tmp”) #创建名为tmp的文件夹

file.copy(c(“A”, “B”), “tmp”) #将文件夹拷贝到tmp文件夹中

list.files(“tmp”) #查看文件夹tmp中的文件名

unlink(“tmp”, recursive=F) #如果文件夹tmp为空，删除文件夹tmp

unlink(“tmp”, recursive=TRUE) #删除文件夹tmp，如果其中有文件一并删除

file.remove(“A”, “B”, “C”) #移除三个文件  

## 特殊目录  

- R.home() 查看R软件的相关目录
- .Library 查看R核心包的目录
- .Library.site 查看R核心包的目录和root用户安装包目录
- .libPaths() 查看R所有包的存放目录
- system.file() 查看指定包所在的目录  

