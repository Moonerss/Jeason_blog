---
title: "Conda使用指南（安装部署篇）"
date: 2020-11-10T19:43:36+08:00
lastmod: 2024-02-28T19:43:36+08:00
author: "Jeason"
categories:
  - software
tags:
  - software
  - conda
description: "这里介绍了conda软件在安装和基本使用场景中的用法"
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

## 关于 Conda  

> Conda is an open source package management system and environment management system that runs on Windows, macOS and Linux. Conda quickly installs, runs and updates packages and their dependencies. Conda easily creates, saves, loads and switches between environments on your local computer. It was created for Python programs, but it can package and distribute software for any language.  

简单来说，Conda 就是专门用于管理 Python 包环境以及部署的工具。  

## Anaconda、Conda 、Miniconda  

+ Anaconda是一个包含180+的科学包及其依赖项的发行版本。其包含的科学包包括：conda, numpy, scipy, ipython notebook等。  
+ conda是包及其依赖项和环境的管理工具。适用多种语言（Python, R, Ruby, Lua等）
+ Miniconda是一个最小安装版本的conda，只包含了少量的依赖和软件包

> 一般来说，三者并没有那么明显，只是在于依赖等信息的囊括问题  

## 安装 Conda  

都可以下载安装包进行安装  

## Conda使用  

### 查看版本

```python
conda -V
```

conda在安装时默认创建Base虚拟环境，并且自带了一系列包可以使用，可以使用`conda list` 查看当前环境中安装的包

> Base 虚拟环境:实际开发中，每个 Python 项目依赖的包都不同，Python 解释器版本也可能不同；每个Python 项目可能是你一人开发，也可能是多人开发；为了保证每个 Python 项目的环境（Python 解释器和项目依赖包）独立，互不干预，以及同一个Python 项目的所有人开发环境一致，Anaconda 可以为每一个项目单独配置Python 的开发和运行环境，也就是 Anaconda 中的虚拟环境（可以类比为仓库）  

### 创建 conda 虚拟环境  

```python
conda create -n test python=3.6
```

### 切换环境  

```python
conda activate test
```

### 退出和删除环境  

退出当前环境回到默认的 Base

```python
conda deactivate test
```

删除环境  

```python
conda remove -n env_name –-all 
```

### 查看系统中的所有环境  

```python
conda info -e
```

## 环境安装包管理  

### 安装指定环境的包  

```python
conda install -n <env_name> <package_name>
```

### 当前环境安装包

```python
conda install <package_name>
```

### 删除指定环境的包

```
conda remove -n <env_name> <package_name>
```

### 删除指定环境的包

```python
conda remove <package_name>
```

### 更新当前环境的包

```python
conda update <package_name>
```

### 更新当前环境所有包

```python
conda update --all
```

### 更新conda

```python
conda update conda
```

## 环境复制和导出

### 环境复制

```python
conda create --name new_env_name --clone copied_env_name
```

### 导出环境

```python
conda env export > environment.yaml
```

该命令会生成一个`environment.yaml`文件，包含了当前环境中的配置

### 导入环境  

```python
conda env create -f environment.yaml
```

## 修改conda镜像源

```python
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ 
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/ 
conda config --set show_channel_urls yes
```

或者手动修改`.condarc`，可以使用`conda config --set show_channel_urls yes` 生成该文件之后再修改。  

```python
channels:
  - defaults
show_channel_urls: true
channel_alias: https://mirrors.tuna.tsinghua.edu.cn/anaconda
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/pro
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

## 修改pip镜像  

### 临时使用pip  

```python
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```

### 设为默认

```python
pip install pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

## conda自动激活环境  

安装conda后在进入终端时可能会自动激活base环境，有以下解决方法：

+ 每次在命令行通过`conda deactivate`退出base环境回到系统自带的环境  

+ 通过将`auto_activate_base`参数设置为`false`实现：  

  ```python
  conda config --set auto_activate_base false
  ```

  也可以设置为`ture`取消更改

## conda安装一些软件  

+ R  

  ```sh
  conda install -c conda-forge r-base
  ```

+ tidyverse包  

  ```sh
  conda install r-tidyverse
  ```

  > 注意，使用conda安装R包会存在版本的问题，因为conda的包版本不一定是最新的，所以最好还是进入R环境进行安装。  

  