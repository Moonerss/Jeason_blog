---
title: "测序质控软件trim_galore安装"
date: 2020-09-23T20:42:14+08:00
lastmod: 2024-02-28T20:42:14+08:00
author: "Jeason"
categories:
  - biosoftware
tags:
  - biosoftware
description: "这里描述了测序去接头软件trim galore的安装及其使用方法"
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

`trim_galore`可以用来对原始的测序数据进行质控

安装 `trim_galore`需要提前安装两个依赖软件：`Fastqc`和 `cutadapt`

`Fastqc`的安装已经提及，这里不再详述

`cutadapt`的安装过程如下：

```sh
sudo apt install python3-pip # install pip3
python3 -m pip install --user --upgrade cutadapt  # install cutadapt
echo "export PATH=$PATH:$HOME/.local/bin" >> ~/.bashrc
source ~/.bashrc
```

`trim_galore` 安装

```sh
# Check that cutadapt is installed
cutadapt --version
# Check that FastQC is installed
fastqc -v
# Install Trim Galore
cd ~/software
curl -fsSL https://github.com/FelixKrueger/TrimGalore/archive/0.6.6.tar.gz -o trim_galore.tar.gz
tar xvzf trim_galore.tar.gz 
# soft link
sudo ln -s /home/ubuntu/software/trim_galore/trim_galore /usr/bin/trim_galore
```

使用示例：

```sh
cd ~/SingleCell
mkdir fastqc_trimmed_results
trim_galore --nextera -o fastqc_trimmed_results Share/ERR522959_1.fastq Share/ERR522959_2.fastq
```

**参数说明：**

> --quality：设定Phred quality score阈值，默认为20。
> --phred33：：选择-phred33或者-phred64，表示测序平台使用的Phred quality score。
> --adapter：输入adapter序列。也可以不输入，Trim Galore会自动寻找可能性最高的平台对应的adapter。自动搜选的平台三个，也直接显式输入这三种平台，即--illumina、--nextera和--small_rna。一般使用fastqc软件能够判断出来，sanger/illumina 1.9为phred33，illumina 1.3/1.5为phred64。
> --stringency：设定可以忍受的前后adapter重叠的碱基数，默认为1（非常苛刻）。可以适度放宽，因为后一个adapter几乎不可能被测序仪读到。
> --length：设定输出reads长度阈值，小于设定值会被抛弃。
> --paired：对于双端测序结果，一对reads中，如果有一个被剔除，那么另一个会被同样抛弃，而不管是否达到标准。
> --retain_unpaired：对于双端测序结果，一对reads中，如果一个read达到标准，但是对应的另一个要被抛弃，达到标准的read会被单独保存为一个文件。
> --gzip和--dont_gzip：清洗后的数据zip打包或者不打包。
> --output_dir：输入目录。需要提前建立目录，否则运行会报错。
> -- trim-n : 移除read一端的reads
