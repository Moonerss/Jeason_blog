---
title: "Linux下百度云快速下载"
date: 2019-09-19T20:57:38+08:00
lastmod: 2024-02-28T20:57:38+08:00
author: "Jeason"
categories:
  - software
tags:
  - software
description: "这里介绍了在Linux环境下如何通过BaiduPCS-Go软件快速下载百度云文件"
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

记录一下找到的linux可用的百度云下载工具，项目地址：https://github.com/iikira/BaiduPCS-Go

直接下载文件解压，进入文件夹，命令行下运行：  

```sh
./BaiduPCS-Go
```

即可进入BaiduPCS-Go的加护界面，使用`help`命令查看相关命令。  

## 初级使用教程

新手建议: **双击运行程序**, 进入仿 Linux shell 的 cli 交互模式;

cli交互模式下, 光标所在行的前缀应为 `BaiduPCS-Go >`, 如果登录了百度帐号则格式为 `BaiduPCS-Go:<工作目录> <百度ID>$ `

以下例子的命令, 均为 cli交互模式下的命令

运行命令的正确操作: **输入命令, 按一下回车键 (键盘上的 Enter 键)**, 程序会接收到命令并输出结果

## 1. 查看程序使用说明

cli交互模式下, 运行命令 `help`

## 2. 登录百度帐号 (必做)

cli交互模式下, 运行命令 `login -h` (注意空格) 查看帮助

cli交互模式下, 运行命令 `login` 程序将会提示你输入百度用户名(手机号/邮箱/用户名)和密码, 必要时还可以在线验证绑定的手机号或邮箱

## 3. 切换网盘工作目录

cli交互模式下, 运行命令 `cd /我的资源` 将工作目录切换为 `/我的资源` (前提: 该目录存在于网盘)

目录支持通配符匹配, 所以你也可以这样: 运行命令 `cd /我的*` 或 `cd /我的??` 将工作目录切换为 `/我的资源`, 简化输入.

将工作目录切换为 `/我的资源` 成功后, 运行命令 `cd ..` 切换上级目录, 即将工作目录切换为 `/`

为什么要这样设计呢, 举个例子,

假设 你要下载 `/我的资源` 内名为 `1.mp4` 和 `2.mp4` 两个文件, 而未切换工作目录, 你需要依次运行以下命令:

```
d /我的资源/1.mp4
d /我的资源/2.mp4
```

而切换网盘工作目录之后, 依次运行以下命令:

```
cd /我的资源
d 1.mp4
d 2.mp4
```

这样就达到了简化输入的目的

## 4. 网盘内列出文件和目录

cli交互模式下, 运行命令 `ls -h` (注意空格) 查看帮助

cli交互模式下, 运行命令 `ls` 来列出当前所在目录的文件和目录

cli交互模式下, 运行命令 `ls /我的资源` 来列出 `/我的资源` 内的文件和目录

cli交互模式下, 运行命令 `ls ..` 来列出当前所在目录的上级目录的文件和目录

## 5. 下载文件

说明: 下载的文件默认保存到 download/ 目录 (文件夹)

cli交互模式下, 运行命令 `d -h` (注意空格) 查看帮助

cli交互模式下, 运行命令 `d /我的资源/1.mp4` 来下载位于 `/我的资源/1.mp4` 的文件 `1.mp4` , 该操作等效于运行以下命令:

```
cd /我的资源
d 1.mp4
```

现在已经支持目录 (文件夹) 下载, 所以, 运行以下命令, 会下载 `/我的资源` 内的所有文件 (违规文件除外):

```
d /我的资源
```

参见 例6 设置下载最大并发量

## 6. 设置下载最大并发量

cli交互模式下, 运行命令 `config set -h` (注意空格) 查看设置帮助以及可供设置的值

cli交互模式下, 运行命令 `config set -max_parallel 250` 将下载最大并发量设置为 250

下载最大并发量建议值: 50~500, 太低下载速度提升不明显甚至速度会变为0, 太高可能会导致程序出错被操作系统结束掉.

## 7. 退出程序

运行命令 `quit` 或 `exit` 或 组合键 `Ctrl+C` 或 组合键 `Ctrl+D`


