---
title: "nohup用法"
date: 2023-04-04T16:24:59+08:00
lastmod: 2024-02-29T16:24:59+08:00
author: "Jeason"
categories:
  - software
tags:
  - software
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


# `nohup`和`&`使用方法  

## nohup(不挂断)

> `nohup`是`no hung up`的缩写，意思是不挂断 。

nohup 语法格式：

```sh
nohup  command  [arg...]
```

说明：
+ 除了无法进行输入操作（比如输入命令、换行、打空格等） 外 ，标准输出 保存到 nohup.out文件中。
+ 关闭客户端后，命令仍然会运行，不会挂断。

例如：
执行 `nohup sh test.sh`脚本命令后，终端不能接收任何输入，标准输出 会输出到当前目录的`nohup.out`文件。即使关闭xshell退出后，当前session依然继续运行。

## `&`（可交互）

`&`语法格式：

```sh
command   [arg...]   &
```

说明：
+ 能进行输入操作（比如输入命令、换行、打空格等），即可进行交互 输入和输出的操作。
+ 标准输出 保存到 nohup.out文件中。
+ 但是 关闭客户端后，程序会就马上停止。

例如：

执行`sh test.sh &`脚本命令后 ，关闭 xshell，脚本程序也立刻停止。


## `nohup`和`&`一块使用（不挂断，可交互）

语法格式：
```sh
nohup   command  [arg...]  &
```

说明：
+ 能进行输入操作（比如输入命令、换行、打空格等），即 可进行交互 输入和输出的操作。
+ 标准输出保存到 nohup.out 中，
+ 关闭客户端后命令仍然会运行。

例子：
执行`nohup sh test.sh &`命令后，能进行输入操作，标准输出的日志写入到`nohup.out`文件，即使关闭xshell，退出当前session后，脚本命令依然继续运行。

# 日志的重定向`>`

上面提到的日志文件默认名称是`nohup.out`，如果修改日志文件的名称，则用到重定向 ，符号是 `>` ，语法格式是
```sh
> logFile
```

说明：
+ `>` 是重定向的符号。
+ `logFile` 是日志文件名称，最好是英文、数字。

此时， `nohup`、 `&` 、 `>` 三者一块使用的 语法格式 ：

```sh
nohup  command >logFile  &
```

示例：

```sh
nohup  start.sh > aa.log  &
```
说明：执行上面的命令后，可以进行输入，也能在后台运行，运行的日志输出到 `aa.log` 日志中。

# 错误信息的处理

```sh
nohup  command >logFile  &
```

虽然解决输入输出，后台也能运行问题，但是还有一项是**错误信息**无法输出到 日志文件中，要解决这个问题，需要增加命令`2 > file`。

标准输出 和 错误信息 同时使用，语法格式如下：

```sh
>logFile1   2 >logFile2
```

## Linux 标准输入、输出、错误信息的符号
Linux 标准输入、输出、错误信息的符号：
+ `0`表示 `stdin (standard input)` 标准信息输入 ；
+ `1`表示 `stdout (standard output)` 标准信息输出 ；
+ `2`表示 `stderr (standard error)` 错误信息 ；

`/dev/null` 表示空设备文件。 如果不想输出任何的日志时，使用此参数 。

再来回顾上面的示例：

```sh
logFile1   2 >logFile2
```
+ `> logFile1` ：即 `1 >logFile1`，`1`是标准信息输出，是默认的，可以省略，`logFile1`是 日志文件名字。
+ `2 >logFile2` ：`2` 是错误信息，即将 **错误信息** 输出 到 `logFile2` 文件中 。

## 错误信息 和 标准输出 输出在同一个文件中
如果想把 错误信息 和 标准输出 在同一个文件中 ，使用 `2>&1` 。 语法如下：

```sh
>logFile   2>&1
```

说明：
+ `>logFile` 表示 标准信息 输出到 `logFile` 文件中；
+ `2>&1` 表示 把 `2`（错误信息） 重定向， 输出到 `1`（标准输出） 中 。
两者的共同使用，表示 把 `2`（错误信息） 、`1`（标准输出） 都输出到同一个文件（`logFile`）中。

# 综合使用（推荐）
综上所述， 功能最全、推荐语法如下：
```sh
nohup  command  >logFile   2>&1  &
```

示例：
```sh
nohup  start.sh > mySysLog.log  2>&1   &
```
说明： 执行命令后，并且将 标准输出(1)、错误信息(2) 写入到 mySysLog.log 文件中。


