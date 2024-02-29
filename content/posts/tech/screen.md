---
title: "linux screen 命令使用"
date: 2019-04-04T20:14:50+08:00
lastmod: 2024-02-28T20:14:50+08:00
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


`screen` 是一个非常有用的命令，提供从单个 SSH 会话中使用多个 shell 窗口（会话）的能力。当会话被分离或网络中断时，screen 会话中启动的进程仍将运行，你可以随时重新连接到 screen 会话。如果你想运行一个持久的进程或者从多个位置连接到 shell 会话，这也很方便。  

#### 安装screen  

`screen` 在一些流行的发行版上已经预安装了。你可以使用下面的命令检查是否已经在你的服务器上安装了。  

```shell
screen -v
Screen version 4.04.00 (GNU) 19-Jun-16
```

如果没有，可以直接使用包管理器进行安装  

#### 启动 screen 会话  

你可以在命令行中输入 `screen` 来启动它，接着会有一个看上去和命令行提示符一样的 `screen` 会话启动  

```shell
screen
```

使用描述性名称启动屏幕会话是一个很好的做法，这样你可以轻松地记住会话中正在运行的进程。要使用会话名称创建新会话，请运行以下命令：  

```shell
screen -S yourname
```

#### screen 会话分离  

要从当前的 screen 会话中分离，你可以按下 `Ctrl-A` 和 `d`。所有的 screen 会话仍将是活跃的，你之后可以随时重新连接。  

#### 重新连接到 screen 会话  

如果你从一个会话分离，或者由于某些原因你的连接被中断了，你可以使用下面的命令重新连接：  

```shell
screen -r
```

如果你有多个 screen 会话，你可以用 `ls` 参数列出它们。  

```shell
screen -ls
There are screens on:
7880.session    (Detached)
7934.session2   (Detached)
2 Sockets in /var/run/screen/S-root.
```

如果你想要还原 “session2” 会话，你可以执行：  

```shell
screen -r 7934
```  

或者：  

```shell
screen -r -S session2
```

#### 中止 screen  

你可以按下 `Ctrl+d`，或者在命令行中使用 `exit` 命令。  

#### 快捷键  

+ `ctrl-a c`：创建一个新的 Shell  
+ `ctrl-a`：在 Shell 间切换  
+ `ctrl-a n`：切换到下一个 Shell  
+ `ctrl-a p`：切换到上一个 Shell  
+ `ctrl-a 0…9`：同样是切换各个 Shell  
+ `ctrl-a d`：退出 Screen 会话  


#### 更多语法  

```shell
screen -S  # 创建一个screen（可以用screen -S ID，打开一个指定的ID session）
screen -ls # 显示所有的screen会话  
screen -d  # 脱离当前screen ,返回正常shell  
screen -r  # 进入指定screen  
screen -S SEEION_NAME -X quit #删除镜像  
```
