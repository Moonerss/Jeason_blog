---
title: "Markdown常用语法"
date: 2018-12-20T17:43:54+08:00
lastmod: 2024-02-28T17:43:54+08:00
author: "Jeason"
categories:
  - Markdown
tags:
  - Markdown
description: "这里是markdown标记语言的常用语法参考"
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


### 什么是Markdown  

**Markdown**是一种[轻量级标记语言](https://zh.wikipedia.org/wiki/轻量级标记语言)，创始人为[约翰·格鲁伯](https://zh.wikipedia.org/wiki/約翰·格魯伯)（英语：John Gruber）。它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的[XHTML](https://zh.wikipedia.org/wiki/XHTML)（或者[HTML](https://zh.wikipedia.org/wiki/HTML)）文档”。作为一种小型标记语言，Markdown很容易阅读，也很容易使用普通的文本编辑器编辑。  

**Markdown**也可以理解为将以MARKDOWN语法编写的语言转换成HTML内容的工具。  

### 亚伦·斯沃茨的传奇人生  

**Markdown**是由[**Aaron Swartz**](http://www.aaronsw.com/)和**John Gruber**共同设计的，**Aaron Swartz**就是那位于2013年自杀,有着**开挂**一般传奇人生经历的美国程序员。死后也被追授入[互联网名人堂](https://zh.wikipedia.org/wiki/網際網路名人堂)。维基百科对他的评价是：[美国](https://zh.wikipedia.org/wiki/美国)[计算机程序员](https://zh.wikipedia.org/wiki/电脑程序员)、[企业家](https://zh.wikipedia.org/wiki/企业家)、[作家](https://zh.wikipedia.org/wiki/作家)、[政治活动者](https://zh.wikipedia.org/w/index.php?title=政治活动者&action=edit&redlink=1)和互联网[黑客主义者](https://zh.wikipedia.org/w/index.php?title=黑客主义&action=edit&redlink=1)  

他的一生足以让我们膜拜：  

- 13岁时，斯沃茨赢得为创办“有用、有教育意义、协作而非商业的网站”的年轻人而设的[ArsDigita](https://zh.wikipedia.org/w/index.php?title=ArsDigita_Prize&action=edit&redlink=1)奖。
- 14岁时，斯沃茨加入编写早期版本的[RSS](https://zh.wikipedia.org/wiki/RSS)1.0版本的工作组。
- **2004**年入读**斯坦福**，2005年暑假之后就退学创业。。。
- **2005**年创建[Infogami](http://infogami.org/)，之后与[Reddit](http://www.reddit.com/)合并成为其合伙人，斯沃茨被给予公司联合创始人的头衔。
- 2008 年，斯沃茨创办了 Watchdog.net，“会咬人的优秀政府类网站”（”the good government site with teeth”），用于汇集政治人物相关的数据起来并进行数据可视化。此外，他最著名的项目之一是《DeadDrop》（之后更名为《[SecureDrop](https://zh.wikipedia.org/w/index.php?title=SecureDrop&action=edit&redlink=1)》），一个致力于帮助记者和线人（[告密者](https://zh.wikipedia.org/wiki/告密者)）安全沟通的平台。
- 2010年创立求进会（Demand Progress），积极参与禁止网络盗版法案（SOPA）活动，最终该提案被撤回。
  - **2011**年7月19日，因被控从MIT和JSTOR下载480万篇学术论文并以免费形式上传于网络被捕。
  - **2013**年1月自杀身亡，死后仍然影响着世界各地的黑客。
  - **科里·多克托罗的评价**：亚伦拥有无以伦比的政治洞见力、技术能力和处理人、事的智能。我认为他本可以彻底改变美国（及世界）政治。不过——他的遗产仍然可以在将来对政治做出影响。  

<img src = "/images/markdown/220px-Aaron_Swartz_profile.jpg" align = "middle"> 




### Markdown的优点  

- 易读（看起来舒服）、易写（语法简单）、易更改**纯文本**
- 与HTML兼容，是属于程序员的Word
- 排版方便
- 越来越多的网站支持Markdown。（主要还是写博客方便😜）  

### Markdown常用语法  

#### 标题  

标题一般有两种形式：
1）使用`=`和`-`标记一级和二级标题。

> 一级标题  
>
> `========= `  
>
> 二级标题  
>
> `---------`  



效果：

> # 一级标题
>
> ## 二级标题  

2）使用`#`，可表示1-6级标题。



> `# 一级标题` 
>
> `## 二级标题`    
>
> `### 三级标题`  
>
> `#### 四级标题`  
>
> `##### 五级标题`  
>
> `###### 六级标题`

效果:

> # 一级标题
>
> ## 二级标题
>
> ### 三级标题
>
> #### 四级标题
>
> ##### 五级标题
>
> ###### 六级标题

#### 段落  

Markdown的段落前后都要有空行，如果要段内强制换行，需要使用**两个以上**`空格键`加上`回车键`  

#### 区块引用  

在段落的每行或者只在第一行使用符号`>`,还可使用多个嵌套引用，如：

> \>区块引用
> \>> 嵌套引用

效果：

> 区块引用
>
> > 嵌套引用  

#### 插入代码块  

代码区块的建立有两种方法：
1）在每行加上4个空格或者一个制表符，如：

```markdown
Hello Markdown
```

**注意**:和普通段落之间需要单独保留空行。  

2）使用三个**```** 来作为代码开始的标志，如：

> \```
> Hello Markdown
> \```

效果如下：  

```markdown
Hello Markdown
```

#### 黑体和斜体  

在强调内容两侧分别加上`*`或者`_`，如：

> \*斜体\*，*斜体*    
>
> \**粗体\*\*，**粗体**

#### 列表  

**无序列表**
使用`·`、`+`、或`-`标记无序列表，如：    

> \-（+\*） 第一项  
>
> -（+\*） 第二项  
>
> -（+\*）第三项  

**注意**：标记后面最少有一个_空格_或_制表符_。若不在引用区块中，必须和前方段落之间存在空行。  

效果：  

> + 第一项
> + 第二项
> + 第三项  

有序列表的标记方式是将上述的符号换成数字,并辅以`.`，如：  

> 1 . 第一项   
> 2 . 第二项    
> 3 . 第三项      

效果：  

> 1. 第一项
> 2. 第二项
> 3. 第三项  

#### 分割线  

分割线最常使用就是三个或以上`*`，还可以使用`-`和`_`。  

**注意**： 第一二级标题会自带分割线  

#### 链接  

链接可以由两种形式生成：**行内式**和**参考式**。  

**行内式**：  

>  [\**Aaron Swartz**\](http://www.aaronsw.com/\)    

效果：  

[**Aaron Swartz**](http://www.aaronsw.com/)  

**参考式**：
就是以参考的形式给出链接  

> [\**Aaron Swartz**\][1\]  
>
> [\**亚伦·斯沃茨**\][2\]  
>
> [1]:http://www.aaronsw.com/
> [2]:http://www.aaronsw.com/

效果：  

[**Aaron Swartz**](http://www.aaronsw.com/)
[**亚伦·斯沃茨**](http://www.aaronsw.com/)  

#### 图像插入  

插入图片的语法和插入超链接的语法基本一致，只是在最前面多一个 `!`。也可分为行内式和参考式两种。   

> \![亚伦·斯沃茨\](https://upload.wikimedia.org/wikipedia/commons/thumb/0/06/Aaron_Swartz_profile.jpg/220px-Aaron_Swartz_profile.jpg\)

Markdown 不支持指定图片的显示大小，不过可以通过直接插入`<img />`标签来指定相关属性。  

#### 字符转义  

反斜线（`\`）用于插入在 Markdown 语法中有特殊作用的字符。  

#### 行内代码块  

起到标记作用。如：  

> \`hello`

效果：  

> `hello`  

### 扩展语法  

#### 删除线  

使用`~~`表示  

> \~~删除线~~  

效果：

> ~~删除线~~

#### 表格  

使用`|`表示表格纵向边界，表头和表内容用`-`隔开，并可用`:`进行对齐设置，两边都有`:`则表示居中，若不加`:`则默认左对齐。  

```tex
| left | center | right |
| :--- | :----: | ----: |
| aaaa | bbbbbb | ccccc |
| a    | b      | c     |
```

效果如下：

| left | center | right |
| :--- | :----- | :---- |
| aaaa | bbbbbb | ccccc |
| a    | b      | c     |

表格中不仅可以加入字符，还可以是链接等信息。  



