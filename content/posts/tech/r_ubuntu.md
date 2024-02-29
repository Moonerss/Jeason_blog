---
title: "从源码安装R3.6(Ubuntu 18.04LTS)"
date: 2019-08-28T20:44:21+08:00
lastmod: 2024-02-28T20:44:21+08:00
author: "Jeason"
categories:
  - software
tags:
  - software
description: "本文介绍了在Ubuntu系统下如何通过手动编译的方式从源码安装R"
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

根据R官方提供的安装方法，感觉在Ubuntu18.04上毫无作用，因此尝试从源码手动编译安装R  

```sh
## 安装依赖
sudo apt-get install xorg-dev libreadline-dev
sudo apt-get install libcurl4-openssl-dev
sudo apt-get install libbz2-dev
sudo apt-get install libcairo2-dev libgtk2.0-dev
sudo apt-get install texinfo texlive
wget http://mirrors.ctan.org/fonts/inconsolata.zip
sudo cp -Rfp inconsolata/* /usr/share/texmf/
# 或者 sudo cp -r inconsolata/ /usr/share/texlive/texmf-dist/tex/latex/
sudo mktexlsr # 刷新一下

# 如果没有java解释器，安装下
sudo apt-get install default-jdk

# 下载与安装R
curl -O https://cran.r-project.org/src/base/R-3/R-3.6.1.tar.gz
tar -zxvf R-3.6.1.tar.gz
cd R-3.6.1/
./configure --prefix=$HOME/local/R --enable-R-shlib --with-cairo=yes
# 或者
# ./configure --with-cairo --with-libpng --with-libtiff --with-jpeglib --enable-R-shlib --prefix=$HOME/local/R
make
make install

# 创建符号链接
cd /usr/bin/
sudo ln -s $HOME/local/R/bin/Rscript Rscript
sudo ln -s $HOME/local/R/bin/R R
```

在编译过程中需要使用`./configure`命令检测环境及其依赖包，检测过程中可能遇到的报错：  

报错一：  

```sh
configure: error: No F77 compiler found
```

解决办法：  

```sh
sudo apt-get install gfortran build-essential
```

报错二：

```sh
configure: error: "liblzma library and headers are required"
```

解决办法：安装xz  

```sh
sudo wget http://tukaani.org/xz/xz-5.2.4.tar.gz
sudo tar xzvf xz-5.2.4.tar.gz
sudo cd xz-5.2.4
sudo ./configure
sudo make
sudo make install
```

