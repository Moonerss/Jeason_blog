---
title: "SRAtoolkit 安装与使用"
date: 2019-10-03T21:07:07+08:00
lastmod: 2024-02-28T21:07:07+08:00
author: "Jeason"
categories:
  - biosoftware
tags:
  - biosoftware
description: "本文介绍了SRAtoolkit的安装与使用"
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


SRAtoolkit是NCBI开发的一个用于SRA文件处理的软件包，包含许多有用的工具。  

### **下载SRAtoolkit软件包**    

软件包下载地址在NCBI网站：https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software  

目前最新版是2.10.8；有多个操作系统版本，我们选Ubuntu 64bit版；  

```sh
wget -P ~/download https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.10.8/sratoolkit.2.10.8-ubuntu64.tar.gz
```

### **解压压缩包**  

```sh
tar zvxf ~/download/sratoolkit.2.10.8-ubuntu64.tar.gz -C ~/software
mv ~/software/sratoolkit.2.10.8-ubuntu64 ~/software/sratoolkit
```

### **将sratoolkit安装文件路径加入环境变量**  

```sh
echo "export PATH=$PATH:/home/ubuntu/software/sratoolkit/bin" >> ~/.zshrc
source ~/.zshrc
```

使用`fastq-dump`和`prefetch`进行测试  

更详细步骤和说明，参见官网Documentation：
https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=std  

### 下载SRA文件  

```SH
prefetch SRR10107270
2020-10-03T06:03:13 prefetch.2.9.6: 1) Downloading 'SRR10107270'...
2020-10-03T06:03:13 prefetch.2.9.6:  Downloading via https...
2020-10-03T06:09:01 prefetch.2.9.6:  https download succeed
2020-10-03T06:09:01 prefetch.2.9.6: 1) 'SRR10107270' was downloaded successfully
2020-10-03T06:09:01 prefetch.2.9.6: 'SRR10107270' has 0 unresolved dependencies
```

### 解压SRA文件  

```sh
for i in *sra
do
echo $i
fastq-dump --split-3 $i
done
```

### 查看文件  

```sh
ls
SRR10107270_1.fastq  SRR10107270_2.fastq  SRR10107270.sra
```

```sh
head SRR10107270_1.fastq
@SRR10107270.1 1 length=72
TCGGGNAGTGCTAGCTCGCGATTCCAGGATGTAGTTAACCTTGAGCACAATTTCATTGACGNNAGCAGCNNN
+SRR10107270.1 1 length=72
AAAAA#EEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEE##EEEEEE###
@SRR10107270.2 2 length=76
CATGTNATTGTTGTAGGAATCAAAGTCAAACACATTTCGAACTACACTGGAGAGACCTTCANNCGGAAANTNNNGT
+SRR10107270.2 2 length=76
AAAAA#EEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEAEEEEEEEEEEEEE/EEEEEE##EEEEEA#E###EE
@SRR10107270.3 3 length=76
AGACGNTGGAGGATGAAGGGCTGGCTGTTGGGTCTGTTCTTGCTCTAAGGCCACATCCTAGNAAAGCAGGGNNNGT   
```

可以看到测序read的读长是七十多bp  

