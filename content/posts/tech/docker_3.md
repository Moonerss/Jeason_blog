---
title: "Docker | 常规操作"
date: 2023-03-23T15:38:36+08:00
lastmod: 2024-02-29T15:38:36+08:00
author: "Jeason"
categories:
  - docker
tags: 
  - docker
description: "本文介绍了Docker的常规操作使用命令"
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
# 启动
启动容器有两种方式，一种是基于镜像新建一个容器并启动，另外一个是将在终止状态（exited）的容器重新启动。

## 新建并启动
所需要的命令主要为 `docker run`

```sh
## 启动并执行echo命令
$ docker run ubuntu:18.04 /bin/echo 'Hello world'
Hello world
## 启动交互式终端
$ docker run -t -i ubuntu:18.04 /bin/bash
root@af8bae53bdd3:/#
```

当利用 `docker run` 来创建容器时，Docker 在后台运行的标准操作包括：
+ 检查本地是否存在指定的镜像，不存在就从 registry 下载
+ 利用镜像创建并启动一个容器
+ 分配一个文件系统，并在只读的镜像层外面挂载一层可读写层
+ 从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去
+ 从地址池配置一个 ip 地址给容器
+ 执行用户指定的应用程序
+ 执行完毕后容器被终止

## 启动已终止容器
可以利用 `docker container start` 命令，直接将一个已经终止（`exited`）的容器启动运行。

# 守护态运行
更多的时候，需要让 Docker 在后台运行而不是直接把执行命令的结果输出在当前宿主机下。此时，可以通过添加 `-d` 参数来实现。
```sh
docker run ubuntu:18.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
hello world
hello world
hello world
hello world

## 如果使用-d参数程序会后台运行
docker run -d ubuntu:18.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
77b2dc01fe0f3f1265df143181e7b9af5e05279a884f4776ee75350ea9d8017a
```

使用 `-d` 参数启动后会返回一个唯一的 id，也可以通过 `docker container ls` 命令来查看容器信息。

```sh
docker container ls
CONTAINER ID  IMAGE         COMMAND               CREATED        STATUS       PORTS NAMES
77b2dc01fe0f  ubuntu:18.04  /bin/sh -c 'while tr  2 minutes ago  Up 1 minute        agitated_wright
```

要获取容器的输出信息，可以通过 `docker container logs` 命令。
```
docker container logs [container ID or NAMES]
hello world
hello world
hello world
. . .
```

# 终止
可以使用 `docker container stop` 来终止一个运行中的容器。
终止状态的容器可以用 `docker container ls -a` 命令看到。例如
```sh
docker container ls -a
CONTAINER ID        IMAGE                    COMMAND                CREATED             STATUS                          PORTS               NAMES
ba267838cc1b        ubuntu:18.04             "/bin/bash"            30 minutes ago      Exited (0) About a minute ago                       trusting_newton
```

处于终止状态的容器，可以通过 `docker container start` 命令来重新启动。
`docker container restart` 命令会将一个运行态的容器终止，然后再重新启动它.

# 进入容器

## `attach`命令

```sh
docker run -dit ubuntu
243c32535da7d142fb0e6df616a3c3ada0b8ab417937c853a9e1c251f499f550

docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
243c32535da7        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           nostalgic_hypatia

docker attach 243c
root@243c32535da7:/#
```
> 注意： 如果从这个 stdin 中 exit，会导致容器的停止。

## `exec` 命令

```sh
docker run -dit ubuntu
69d137adef7a8a689cbcb059e94da5489d3cddd240ff675c640c8d96e84fe1f6

docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
69d137adef7a        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           zealous_swirles
## 只使用-i参数没有终端，只会返回运行结果
docker exec -i 69d1 bash
ls
bin
boot
dev
...
## 使用-it参数可进入终端
docker exec -it 69d1 bash
root@69d137adef7a:/#
```

> 如果从这个 stdin 中 exit，不会导致容器的停止。

> ⚠️使用`exec`进入容器，相当于重新打开一个终端，并不影响原来运行的程序，使用`attach`相当于连接到正在执行的终端，不会启动新的进程。

# 导出和导入

## 导出容器

```sh
docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                    PORTS               NAMES
7691a814370e        ubuntu:18.04        "/bin/bash"         36 hours ago        Exited (0) 21 hours ago                       test
docker export 7691a814370e > ubuntu.tar
```
这样将导出容器快照到本地文件。

## 导入容器
```sh
cat ubuntu.tar | docker import - test/ubuntu:v1.0

docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
test/ubuntu         v1.0                9d37a6082e97        About a minute ago   171.3 MB

## 也可以通过指定 URL 或者某个目录来导入
docker import http://example.com/exampleimage.tgz example/imagerepo
```

# 删除
## 删除容器
```sh
docker container rm trusting_newton
trusting_newton
```

如果要删除一个运行中的容器，可以添加 `-f` 参数。Docker 会发送 SIGKILL 信号给容器。

## 清理所有处于终止状态的容器
用 `docker container ls -a` 命令可以查看所有已经创建的包括终止状态的容器，如果数量太多要一个个删除可能会很麻烦，用`docker container prune`命令可以清理掉所有处于终止状态的容器。

