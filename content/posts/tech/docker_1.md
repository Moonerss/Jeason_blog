---
title: "Docker | 安装与运行"
date: 2020-06-08T15:15:53+08:00
lastmod: 2024-02-29T15:15:53+08:00
author: "Jeason"
categories:
  - docker
tags:
  - docker
description: "本文介绍一下Docker最基本的安装和使用命令"
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

**Docker** 是一个开源的应用容器引擎，基于 **Go 语言** 并遵从 `Apache2.0` 协议开源。  

**Docker** 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。  

### Docker的应用场景  

+ Web 应用的自动化打包和发布。  
+ 自动化测试和持续集成、发布。  
+ 在服务型环境中部署和调整数据库或其他的后台应用。  
+ 从头编译或者扩展现有的 OpenShift 或 Cloud Foundry 平台来搭建自己的 PaaS 环境。  

## docker 安装  
 
`Docker` 的旧版本被称为 `docker`，`docker.io` 或 `docker-engine` 。如果已安装，请卸载它们：  

```sh
sudo apt-get remove docker docker-engine docker.io containerd runc
```

首先安装依赖:  

```sh
sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
```

添加信任 Docker 的 GPG 公钥:  

```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

对于 amd64 架构的计算机，添加软件仓库:  

```sh
sudo add-apt-repository \
   "deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

最后安装  

```sh
sudo apt-get update
sudo apt-get install docker-ce
```

```sh
docker --version
## Docker version 19.03.11, build 42e35e61f3
```

## Hello world  

`Docker` 允许你在容器内运行应用程序， 使用 `docker run` 命令来在容器内运行一个应用程序。  

```sh
docker run ubuntu:15.10 /bin/echo "Hello world"
## hello world
```

参数解析：  
+ **docker** : Docker 的执行命令  
+ **run** : 与`docker`命令组合运行一个特定的容器  
+ **ubuntu:15.10** : 指定要运行的镜像，`Docker` 首先从本地主机上查找镜像是否存在，如果不存在，`Docker` 就会从镜像仓库 `Docker Hub` 下载公共镜像。  
+ **/bin/echo "Hello world"** : 在启动的容器里执行的命令  

连起来就是：`Docker 以 ubuntu15.10 镜像创建一个新容器，然后在容器里执行 bin/echo "Hello world"，然后输出结果。`  

## 交互运行  

```sh
ubuntu@VM-0-11-ubuntu:~$ sudo docker run -i -t ubuntu:15.10 /bin/bash
root@2401df9ef044:/# 
```

+ **-t**: 在新容器内指定一个伪终端或终端。  
+ **-i**: 允许你对容器内的标准输入进行交互。  

通过运行 `exit` 命令或者使用 `CTRL+D` 来退出容器。  

## 后台运行  

```sh
ubuntu@VM-0-11-ubuntu:~$ sudo docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
9db349c972802d3a7627b27d3c9eb0c58888ad0c80a001b5d7e6f08813eed876
```

输出的长字符串叫做容器 ID，对每个容器来说都是唯一的，我们可以通过容器 ID 来查看对应的容器发生了什么  

可以通过 `docker ps` 来查看容器的运行情况  

```sh
ubuntu@VM-0-11-ubuntu:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
9db349c97280        ubuntu:15.10        "/bin/sh -c 'while t…"   11 seconds ago      Up 10 seconds                           tender_black
```

输出详情介绍:  

+ **CONTAINER ID**: 容器 ID。  
+ **IMAGE**: 使用的镜像。  
+ **COMMAND**: 启动容器时运行的命令。  
+ **CREATED**: 容器的创建时间。  
+ **STATUS**: 容器状态。  
+ **PORTS**: 容器的端口信息和使用的连接类型（tcp\udp）。  
+ **NAMES**: 自动分配的容器名称。  

> docker 运行有七种状态  
> + created（已创建）  
> + restarting（重启中）  
> + running（运行中）  
> + removing（迁移中）  
> + paused（暂停）  
> + exited（停止）  
> + dead（死亡）  

使用`docker logs`查看输出结果  

```sh
ubuntu@VM-0-11-ubuntu:~$ sudo docker logs tender_black
hello world
```

使用`docker stop`停止容器  

```sh
ubuntu@VM-0-11-ubuntu:~$ sudo docker stop tender_black
tender_black
```

## docker 用户  

docker进程使用Unix Socket而不是TCP端口。而默认情况下，Unix socket属于root用户，需要root权限才能访问。  

解决方案：  
1. 使用sudo获取管理员权限，运行docker命令  
2. docker守护进程启动的时候，会默认赋予名字为docker的用户组读写Unix socket的权限，因此只要创建docker用户组，并将当前用户加入到docker用户组中，那么当前用户就有权限访问Unix socket了，进而也就可以执行docker相关命令  
   ```sh
   sudo groupadd docker     #添加docker用户组
   sudo gpasswd -a $USER docker     #将登陆用户加入到docker用户组中
   newgrp docker     #更新用户组
   docker ps    #测试docker命令是否可以使用sudo正常使用
   ```

## 基本概念  

+ **Docker 镜像**（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:18.04 就包含了完整的一套 Ubuntu 18.04 最小系统的 root 文件系统。  
  > **Docker 镜像** 是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像**不包含**任何动态数据，其内容在构建之后也不会被改变。
+ **容器**（Container）是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。  
+ **仓库**（Repository）是用于储存镜像的地址。镜像构建完成后，可以很容易的在当前宿主机上运行，但是，如果需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，[Docker Registry]() 就是这样的服务。一个 **Docker Registry** 中可以包含多个**仓库**（Repository）；每个仓库可以包含多个**标签**（Tag）；每个标签对应一个镜像。通常，一个仓库会包含同一个软件不同版本的镜像，而标签就常用于对应该软件的各个版本。我们可以通过 `<仓库名>:<标签>` 的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以`latest`作为默认标签。  
