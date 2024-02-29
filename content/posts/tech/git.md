---
title: "git | 基本用法"
date: 2018-05-23T16:15:39+08:00
lastmod: 2024-02-29T16:15:39+08:00
author: "Jeason"
categories:
  - git
tags:
  - git
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

## Git的基本使用方法  

## Git简介  

Git是一个功能强大的分布是版本控制系统，由linux系统的创始人Linus创建，用于优化管理linux源码。Git具有强大的分支管理能力，对于代码集成管理具有超越其他集中式版本控制系统的优势。  

## Git安装  

在windows上安装Git，只需要从Git官网下载[安装程序](https://git-scm.com/)，按照默认选项安装即可。  

最终，安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！  

安装完成后需要进行一步设置，在Git Bash中输入命令行  

```bash
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

因为Git是分布式版本控制系统，所以，每个电脑必须有自己的ID：你的名字和Email地址。  

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。  

### Git的基本原理  

在Git中,主要有两部分组成：工作区和版本库，而版本库又分为暂存区和分支。  

当你创建编辑一个文件后，首先需要将文件从工作区添加到版本库的暂存区，然后再从暂存区提交的Git的分支系统上。  

> 个人认为，从工作区到暂存区的过程相当于将文件的所有更改日志依次保存在暂存区，而从暂存区提交到Git分支的过程则是一次性提交所有的更改  

### 本地仓库的使用  

> 版本库又名仓库，英文名repository，可以理解成一个文件夹,这个文件夹下的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。  

#### 创建仓库  

找合适的地方，创建一个空目录，创建并进入，一般不要用中文的目录名，容易出现问题.  

```bash
makdir learngit
cd learngit
```

#### 初始化仓库  

`git init` 命令将这个目录变成可以管理的版本仓库（git repository）  

```bash
git init
Initialized empty Git repository in /home/deepin/learngit/.git/
```

此时你已经创建了一个空的版本库，可以发现当前目录下多了一个.git目录，这个目录就是git用来跟踪管理版本仓库的，不要去手动修改。  

```bash
deepin@deepin-PC:~/learngit$ ls -a
.  ..  .git
```

#### 添加文件到仓库  

把文件添加到版本库，如果没有任何提示信息则为成功添加  

```bash
git add README.md
```

把文件提交到版本库  

```bash
git commit -m "this is a README.md file"
```

双引号中输入的是文件描述信息，是对此次文件提交的说明  

同样，你也可以一次提交多个文件  

```bash
git add file1.txt
git add file2.txt
git add file3.txt
git commit -m "add 3 files."
```

### 添加远程仓库  

当你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，使两个仓库能够远程同步，以下操作可以帮你实现  

1. 首先需要在远端github账户中创建自己的仓库  
2. 在本地使用git命令添加远程仓库  

```bash
git remote add origin <adress> #最后是远程仓库地址
```

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库  

3. 然后就可以向远端仓库`push`文件  

```bash
git push -u origin master
```

这条命令实际上是把本地`master`分支推送到远程仓库上，使用`-u`参数Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`maste`r分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。  


### Git的基本命令  

#### 添加文件  

+ 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；  
+ 使用命令`git commit -m <message>` 提交文件到仓库，添加提交注释信息  

#### 状态查看  

`git status`查看repository的状态，`git diff`查看修改了哪些内容  

#### 文件修改  

+ `git status`可以查看当前版本库中文件的提交状态  
+ `git rm`可以删除某一文件  

#### 版本退回  

+ `git reset`命令可以使文件退回之前的版本或者从之前的版本回到修改后的版本  


  > 在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`。   


  + `git log`命令可以查看提交历史，以便确定要回退到哪个版本。  
  + `git reflog`可以查看命令历史，以便确定要回到未来的哪个版本。  

  > 回退到以前的版本A之后，用`git log`命令就找不到A之后的版本了，如果想回到之后的版本，需要`git reset --hard commitID`，但是如果不记得`commitID`,可以用`git reflog`，它记录了每一个命令  

### 常用的git命令  

```bash
git init #使当前目录变成可以管理的版本仓库（git repository)
git add filename #将文件添加到版本仓库
git commit -m "description" #把文件提交到仓库
git status #查看repository的状态
git diff #查看修改了哪些内容
git log #查看提交日志
git log --pretty=oneline #简洁地显示提交日志
git reset --hard HEAD~<3> #回退到某个版本，比如这里回退到第前3个版本
git reset --hard <commit ID> #回退到特定ID的版本
git reflog #记录了每个命令，可以用来查看每个操作的编号
git checkout -b <branch> #创建并切换分支
git branch #查看当前分支
```

## Git 配置多个账户  

这里分两种情况：  

> 1. 一个账户已经进行了配置，添加另外一个账户
> 2. 同时配置两个git账户

假如之前你已经有账户进行了git的全局设置，那么首先需要解除git全局`用户名/邮箱`的设置  

~~~bash
git config --global --unset "user.name"
git config --global --unset "user.email"
```   
如果之前并未进行全局设置，则直接开始设置每个仓库独立的用户和邮箱  
```shell
#设置第一个用户/邮箱
git config user.email "A@gmail.com"
git config user.name "A"

#设置第二个用户/邮箱
git config user.email "B@163.com"
git config user.name "B"
~~~

在进行单用户操作时我们只需要通过：  

```bash
ssh-keygen -t rsa -f -C "yourmail@xxx.com"
```

命令直接产生相应的`密钥/公钥`，但是在多用户的情况下,直接产生两次`密钥/公钥`会将第一次产生的`密钥/公钥`覆盖掉。因此，进行多用户操作时，需要指定产生的密钥名称，避免覆盖。  

```bash
#创建A账户的密钥、公钥
ssh-keygen -t rsa -f ~/.ssh/id_rsa_A -C "A@gmail.com"
ssh-keygen -t rsa -f ~/.ssh/id_rsa_B -C "B@163.com"
```  
此时会在用户的`.ssh`文件夹下产生`id_rsa_A`、`id_rsa_A.pub`和`id_rsa_B`、`id_rsa_B.pub`两对`密钥/公钥`。  
##### 3. 设置config文件  
在.ssh文件夹下创建config文件,配置不同的仓库指向不同的密钥文件。

```shell
# A账号
Host A #host别名
HostName github.com #服务器域名
User git #A账号用户名
IdentityFile ~/.ssh/id_rsa
# B账号
Host B  #host别名
HostName github.com #服务器域名
User git #B账号用户名
IdentityFile ~/.ssh/id_rsa_x
```
> **原理解析**
> 1.ssh 客户端是通过类似 [git@github.com](mailto:git@github.com):githubUserName/repoName.git ** 的地址来识别使用本地的哪个私钥的，地址中的 User 是@前面的git， Host 是@后面的github.com。
> 2.如果所有账号的 User 和 Host 都为 git 和 github.com，那么就只能使用一个私钥。所以要对User 和 Host 进行配置，让每个账号使用自己的 Host，即使用上面的别名来代替域名。
> 3.配置了别名之后，新的地址就是git@A:githubUserName/repName.git**（在添加远程仓库时使用）。这样 ssh 在连接时就可以区别不同的账号了。  

把相应的公钥添加到账户的SSH设置当中，这样git就可以识别不同的账户了。  