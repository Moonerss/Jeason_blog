---
title: "hexo 博客搭建"
date: 2019-12-17T21:10:26+08:00
lastmod: 2024-02-28T21:10:26+08:00
author: "Jeason"
categories:
  - hexo
tags:
  - hexo
description: "这里介绍了如何使用hexo框架搭建个人博客"
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


# 前言  

使用hexo + github pages 搭建博客的好处:  

1. 全是静态文件，访问速度快；  
2. 免费方便，不用花一分钱就可以搭建一个自由的个人博客，不需要服务器不需要后台；  
3. 可以随意绑定自己的域名，不仔细看的话根本看不出来你的网站是基于github的  
4. 数据绝对安全，基于github的版本管理，想恢复到哪个历史版本都行；  
5. 博客内容可以轻松打包、转移、发布到其它平台  

# 准备工作  

有一个github账号，没有的话去注册一个；  
安装了`node.js`、`npm`  
安装了git  

本文环境：  
> Windows 10
> hexo: 4.1.1
> node: 12.13.1
> git: 2.24.0

# 创建github仓库  

## 创建仓库  

新建一个名为你的用户名`.github.io`的仓库，比如说，如果你的github用户名是`test`，那么你就新建`test.github.io`的仓库（必须是你的用户名，其它名称无效），将来你的网站访问地址就是 http://test.github.io 了  

由此可见，每一个github账户最多只能创建一个这样可以直接使用域名访问的仓库。  

几个注意的地方：  

> 1. 注册的邮箱一定要验证，否则不会成功；  
> 2. 仓库名字必须是：`username.github.io`，其中`username`是你的用户名；  

创建成功后，以后博客所有代码都是放在这个仓库里了  

## 配置SSH key  

配置ssh密钥是为了能够将本地仓库连接到github，详情请见：[同一电脑配置多个Github账户](https://mooner.netlify.app/2018/09/25/more-github/)  

## 测试是否连接成功  

使用命令行  

```shell
ssh -T git@github.com # 注意邮箱地址不用改
```

如果提示`Are you sure you want to continue connecting (yes/no)?`，输入`yes`，然后会看到：  

> Hi xxx! You’ve successfully authenticated, but GitHub does not provide shell access.  

# 搭建hexo博客  

## hexo简介  

Hexo是一个简单、快速、强大的博客发布工具，支持Markdown格式，有众多优秀插件和主题。  

官网： http://hexo.io  

github: https://github.com/hexojs/hexo  

## 原理  

由于github pages存放的都是静态文件，博客存放的不只是文章内容，还有文章列表、分类、标签、翻页等动态内容，假如每次写完一篇文章都要手动更新博文目录和相关链接信息，相信谁都会疯掉，所以hexo所做的就是将这些md文件都放在本地，每次写完文章后调用写好的命令来批量完成相关页面的生成，然后再将有改动的页面提交到github。  

## 安装  

### 注意事项  

> 1. 很多命令既可以用Windows的cmd来完成，也可以使用git bash来完成，但是部分命令会有一些问题，为避免不必要的问题，建议全部使用git bash来执行  
> 2. hexo有2种`_config.yml`文件，一个是根目录下的全局的`_config.yml`，一个是各个theme下的`_config.yml`。为了描述方便，在以下说明中，将前者称为 **站点配置文件**， 后者称为 **主题配置文件**  

### 安装hexo  

```shell
npm install -g hexo
```

### 初始化  

在电脑的某个地方新建一个名为`hexo`的文件夹（名字可以随便取），比如我的是`F:\Workspaces\hexo`，由于这个文件夹将来就作为你存放代码的地方，所以最好不要随便放。  

hexo会自动下载一些文件到这个目录，包括node_modules，目录结构如下图:  

```shell
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

```shell
hexo g # 生成
hexo s # 启动服务
```

执行以上命令之后，hexo就会在public文件夹生成相关html文件，这些文件将来都是要提交到github去的：  

`hexo s`是开启本地预览服务，打开浏览器访问 http://localhost:4000 即可看到内容  

## 主题  

### 更改主题  

默认主题很丑，那我们别的不做，首先来替换一个好看点的主题  

[官方主题](https://hexo.io/themes/)  

首先下载主题  

```shell
cd /f/Workspaces/hexo/
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```

下载以后的主题全在`theme`文件夹下边  

打开根目录下站点配置文件`_config.yml`， 找到 `theme` 字段，并将其值更改为 `next`, 然后重新执行`hexo g`来重新生成。  

如果出现一些莫名其妙的问题，可以先执行`hexo clean`来清理一下`public`的内容，然后再来重新生成和发布。  

## github上传  

如果你一切都配置好了，发布上传很容易，一句`hexo d`就搞定，当然关键还是你要把所有东西配置好。  

首先，`ssh key`肯定要配置好。其次，配置站点配置文件`_config.yml`中有关`deploy`的部分:  

```shell
deploy:
  type: git
  repo: https://github.com/Jeason-Zhao/Jeason-Zhao.github.io.git
  branch: master
```

> 注意：最新版本一定要是这种格式  

最后需要安装一个插件:  

```shell
npm install hexo-deployer-git --save
```

打开`git bash`，输入`hexo d`就会将本次有改动的代码全部提交，没有改动的不会  

## 保留CNAME、README.md等文件  

hexo默认会把所有md文件都转换成html，包括README.md，所以在上传之前配置站点配置文件`_config.yml`的`skip_render`部分：  

```shell
skip_render: README.md
```

这样就会跳过该文件。  

## 常用hexo命令  

```shell
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
```

缩写：  

```shell
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

组合命令：  

```shell
hexo s -g #生成并本地预览
hexo d -g #生成并上传
```

# 写博客  

## 写博客  

定位到我们的hexo根目录，执行命令：  

```shell
hexo new 'my-first-blog'
```

hexo会帮我们在`_posts`下生成相关md文件：  

我们只需要打开这个文件就可以开始写博客了，

当然你也可以直接自己新建md文件，用这个命令的好处是帮我们自动生成了时间。一般完整格式如下：  

```md
---
title: postName #文章页面上的显示名称，一般是中文
date: 2013-12-02 15:30:16 #文章生成时间，一般不改，当然也可以任意修改
categories: 默认分类 #分类
tags: [tag1,tag2,tag3] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面
---

以下是正文
```

## 新建页面  

假如我新建一个名字为`about`的页面：  

```shell
hexo new page "about"
```

在`source`文件夹下，将生成`about`文件夹  

部署后将在`public`文件夹生成一个新的`html`页面：hexo\public\about\index.html，通过访问https://用户名.github.io/about/访问这个页面：  









