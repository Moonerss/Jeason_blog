---
title: "SSH连接自动断开问题"
date: 2020-11-16T16:11:25+08:00
lastmod: 2024-02-29T16:11:25+08:00
author: "Jeason"
categories: 
  - ssh
tags:
  - ssh
description: "本文主要介绍了在使用ssh连接时如何解决自动断开的问题"
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

平时在命令行下ssh连接了远程服务器，经常才几分钟没操作就被自动断线了，不能进行任何操作，其实这是因为ssh没有设置心跳检测，可以通过以下两种方法解决。  



+ 本地机最常用的方法  

  依赖ssh客户端定时发送心跳检测，配置/etc/ssh/ssh_config文件，在末尾添加上：  

  ```sh
  ServerAliveInterval 20
  ServerAliveCountMax 999
  ```

  > 每隔20秒向服务器发出一次心跳检测，若超过999次请求都没有成功，就主动断开与服务器端的连接。  

+ 适用于服务器端  

  依赖ssh服务器端定时发送心跳检测，配置/etc/ssh/sshd_config文件(注意：这里是sshd_config，不是ssh_config)，在末尾添加上:  

  ```sh
  ClientAliveInterval 30
  ClientAliveCountMax 6
  ```

  > 每隔30秒向客户端发出一次心跳检测，若超过6次请求都没有成功，就会主动断开与客户端的连接。 

  设置了ssh的心跳检测后，重启ssh服务才能生效，执行命令

  ```sh
  service ssh restart
  ```

  