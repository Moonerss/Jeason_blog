---
title: "HTML基本语法"
date: 2019-06-10T17:31:46+08:00
lastmod: 2024-02-28T17:31:46+08:00
author: "Jeason"
categories:
  - HTML
tags:
  - HTML
description: "这里汇总了一些常用的HTML语法"
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
# HTML  

超文本标记语言（英语：HyperText Markup Language，简称：HTML）是一种用于创建网页的标准标记语言。HTML可以用 来建立自己的 WEB 站点，HTML 运行在浏览器上，由浏览器来解析。

HTML 不是一种编程语言，而是一种**标记**语言，它使用一套标记标签(markup tag)来描述网页；HTML文档包含了HTML标签和文本内容，HTML文档也被称为web页面。  



## html 元素  

HTML文档由 **HTML 元素**定义。HTML元素包括：**开始标签**，**元素内容**，**结束标签**  

### HTML 元素语法  

- HTML 元素以**开始标签**起始
- HTML 元素以**结束标签**终止
- **元素的内容**是开始标签与结束标签之间的内容
- 某些 HTML 元素具有**空内容（empty content）**
- 空元素**在开始标签中进行关闭**（以开始标签的结束而结束）
- 大多数 HTML 元素可拥有**属性**

> HTML 文档是由嵌套的 HTML 元素构成的  

## HTML 属性  

属性是 HTML 元素提供的附加信息。  

- HTML 元素可以设置**属性**
- 属性可以在元素中添加**附加信息**
- 属性一般描述于**开始标签**
- 属性总是以名称/值对的形式出现，**比如：name="value"**。

```html
<a href="http://www.runoob.com">这是一个链接</a>
```

## HTML 标题    

在 HTML 文档中，标题很重要。标题（Heading）是通过 `<h1> - <h6>` 标签进行定义的.`<h1>` 定义最大的标题。 `<h6>` 定义最小的标题。  

```html
<h1>这是一个标题。</h1>
<h2>这是一个标题。</h2>
<h3>这是一个标题。</h3>
```

> 请确保将 HTML 标题 标签**只用于标题**。不要仅仅是为了生成**粗体**或**大号**的文本而使用标题。  

### HTML 水平线  

`<hr>`标签在 HTML 页面中创建水平线。  用于分隔内容。  

### HTML注释  

可以将注释插入 HTML 代码中，这样可以提高其可读性，使代码更易被人理解。浏览器会忽略注释，也不会显示它们。

注释写法如下:  

```html
<!-- 这是一个注释 -->
```

## HTML 段落  

段落是通过 `<p> `标签定义的。如果在不产生新段落的情况下换行需要使用`<br>`  

```html
<p>这个<br>段落<br>演示了分行的效果</p>
```

> `<br />`元素是一个空的 HTML 元素。由于关闭标签没有任何意义，因此它没有结束标签。  

## HTML 格式化标签  

### HTML 文本格式化标签  

| 标签       | 描述         |
| ---------- | ------------ |
| `<b>`      | 定义粗体文本 |
| `<em>`     | 定义着重文字 |
| `<i>`      | 定义斜体字   |
| `<small>`  | 定义小号字   |
| `<strong>` | 定义加重语气 |
| `<sub>`    | 定义下标字   |
| `<sup>`    | 定义上标字   |
| `<ins>`    | 定义插入字   |
| `<del>`    | 定义删除字   |

### HTML "计算机输出" 标签  

| 标签     | 描述               |
| -------- | ------------------ |
| `<code>` | 定义计算机代码     |
| `<kbd>`  | 定义键盘码         |
| `<samp>` | 定义计算机代码样本 |
| `<var>`  | 定义变量           |
| `<pre>`  | 定义预格式文本     |

### HTML 引文, 引用, 及标签定义  

| 标签           | 描述             |
| -------------- | ---------------- |
| `<abbr>`       | 定义缩写         |
| `<address>`    | 定义地址         |
| `<bdo>`        | 定义文字方向     |
| `<blockquote>` | 定义长的引用     |
| `<q>`          | 定义短的引用语   |
| `<cite>`       | 定义引用、引证   |
| `<dfn>`        | 定义一个定义项目 |

## HTML头部信息  

### HTML `<head>` 元素  

`<head>`元素包含了所有的头部标签元素。在 `<head>`元素中你可以插入脚本（scripts）, 样式文件（CSS），及各种meta信息。

可以添加在头部区域的元素标签为: `<title>`,` <style>`, `<meta>`, `<link>`,` <script>`,`<noscript>`, and `<base>`.  

### HTML `<title>` 元素  

`<title>` 标签定义了不同文档的标题，在HTML/XHTML 文档中是必须的。  

- 定义了浏览器工具栏的标题
- 当网页添加到收藏夹时，显示在收藏夹中的标题
- 显示在搜索引擎结果页面的标题

### HTML `<base>` 元素  

`<base>` 标签描述了基本的链接地址/链接目标，该标签作为HTML文档中所有的链接标签的默认链接.  

### HTML `<link>` 元素  

`<link>` 标签定义了文档与外部资源之间的关系，通常用于链接到样式表.  

### HTML `<style>` 元素  

`<style>` 标签定义了HTML文档的样式文件引用地址，也可以直接添加样式来渲染 HTML 文档  

### HTML `<meta>` 元素  

`<meta>` 标签提供了元数据.**元数据也不显示在页面上，但会被浏览器解析**。META 元素通常用于指定网页的描述，关键词，文件的最后修改时间，作者，和其他元数据。元数据可以使用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他Web服务。`<meta>` 一般放置于 `<head>` 区域  

### HTML `<script>` 元素  

`<script>`标签用于加载脚本文件，如： JavaScript  

### HTML head 元素汇总  

| 标签       | 描述                               |
| ---------- | ---------------------------------- |
| `<head>`   | 定义了文档的信息                   |
| `<title>`  | 定义了文档的标题                   |
| `<base>`   | 定义了页面链接标签的默认链接地址   |
| `<link>`   | 定义了一个文档和外部资源之间的关系 |
| `<meta>`   | 定义了HTML文档中的元数据           |
| `<script>` | 定义了客户端的脚本文件             |
| `<style>`  | 定义了HTML文档的样式文件           |

## HTML 样式- CSS  

CSS (Cascading Style Sheets) 用于渲染HTML元素标签的样式.CSS 可以通过以下方式添加到HTML中:  

- 内联样式- 在HTML元素中使用"style" **属性**
- 内部样式表 -在HTML文档头部 `<head>` 区域使用`<style>` **元素** 来包含CSS
- 外部引用 - 使用外部 CSS **文件**

> 使用CSS最好的方式是通过外部引用CSS文件.

## HTML 图像  

在 HTML 中，图像由`<img>` 标签定义。`<img>` 是空标签，意思是说，它只包含属性，并且没有闭合标签。  

> 空格`<br>`也是没有闭合标签的  

定义图像的一般语法：  

```html
<img src="url" alt="some_text">
```

### Alt属性  

alt 属性用来为图像定义一串预备的可替换的文本。在浏览器无法载入图像时，替换文本属性告诉读者她们失去的信息。此时，浏览器将显示这个替代性的文本而不是图像。  

```html
<img src="boat.gif" alt="Big Boat">
```

### 设置图像的高度与宽度  

height（高度） 与 width（宽度）属性用于设置图像的高度与宽度。

属性值默认单位为像素:  

```html
<img src="pulpit.jpg" alt="Pulpit rock" width="304" height="228">
```

## HTML 表格  

表格由 `<table>` 标签来定义。每个表格均有若干行（由 `<tr>` 标签定义），每行被分割为若干单元格（由`<td>` 标签定义）。字母 td 指表格数据（table data），即数据单元格的内容。数据单元格可以包含文本、图片、列表、段落、表单、水平线、表格等等。  

表格的表头使用 `<th>` 标签进行定义。  

```html
<table border="1">
    <tr>
        <th>Header 1</th>
        <th>Header 2</th>
    </tr>
    <tr>
        <td>row 1, cell 1</td>
        <td>row 1, cell 2</td>
    </tr>
    <tr>
        <td>row 2, cell 1</td>
        <td>row 2, cell 2</td>
    </tr>
</table>
```

<table border="1">
    <tr>
        <th>Header 1</th>
        <th>Header 2</th>
    </tr>
    <tr>
        <td>row 1, cell 1</td>
        <td>row 1, cell 2</td>
    </tr>
    <tr>
        <td>row 2, cell 1</td>
        <td>row 2, cell 2</td>
    </tr>
</table>

> 大多数浏览器会把表头显示为粗体居中的文本  

| 标签         | 描述                 |
| ------------ | -------------------- |
| `<table>`    | 定义表格             |
| `<th>`       | 定义表格的表头       |
| `<tr>`       | 定义表格的行         |
| `<td>`       | 定义表格单元格       |
| `<caption>`  | 定义表格标题         |
| `<colgroup>` | 定义表格列的组       |
| `<col>`      | 定义用于表格列的属性 |
| `<thead>`    | 定义表格的页眉       |
| `<tbody>`    | 定义表格的主体       |
| `<tfoot>`    | 定义表格的页脚       |

## HTML 列表  

### HTML无序列表  

无序列表是一个项目的列表，此列项目使用粗体圆点（典型的小黑圆圈）进行标记。

无序列表使用 `<ul>` 标签  

```html
<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>
```

<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>

### HTML 有序列表  

有序列表也是一列项目，列表项目使用数字进行标记。 有序列表始于 `<ol>` 标签。每个列表项始于 `<li>` 标签。列表项使用数字来标记。  

```html
<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>
```

<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>

### HTML 自定义列表  

自定义列表不仅仅是一列项目，而是项目及其注释的组合。

自定义列表以 `<dl>` 标签开始。每个自定义列表项以 `<dt>` 开始。每个自定义列表项的定义以 `<dd>` 开始。  

```html
<dl>
<dt>Coffee</dt>
<dd>- black hot drink</dd>
<dt>Milk</dt>
<dd>- white cold drink</dd>
</dl>
```

<dl>
<dt>Coffee</dt>
<dd>- black hot drink</dd>
<dt>Milk</dt>
<dd>- white cold drink</dd>
</dl>

## HTML 区块  

大多数 HTML 元素被定义为**块级元素**或**内联元素**。

**块级元素**在浏览器显示时，通常会以新行来开始（和结束）。（如：`<h1>`, `<p>`, `<ul>`, `<table>`）

**内联元素**在显示时通常不会以新行开始。（如：`<b>`, `<td>`, `<a>`, `<img>`）  

**`<div>` 元素是块级元素**，它可用于组合其他 HTML 元素的容器。`<div>` 元素没有特定的含义。除此之外，由于它属于块级元素，浏览器会在其前后显示折行。如果与 CSS 一同使用，`<div>` 元素可用于对大的内容块设置样式属性。  

**`<span>` 元素是内联元素**，可用作文本的容器。`<span>` 元素也没有特定的含义。当与 CSS 一同使用时，`<span>` 元素可用于为部分文本设置样式属性。  

## HTML 布局  

大多数网站可以使用 `<div>` 或者 `<table>` 元素来创建多列。CSS 用于对元素进行定位，或者为页面创建背景以及色彩丰富的外观。  

例如：  

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Demo</title>
</head>
<body>
	<div id="maxname" style="width:500px;">
      <div id="hname" style="background-color:#666FFF;">
        <h1 style="margin-bottom:0;text-align:center;">Div布局实例</h1>
      </div>
      <div id="menu" style="background-color:#FFFFFF;height:200px;width:100px;float:left;">    
          菜单<br/>
          num1<br/>
          num2<br/>
          num3
      </div>
      <div id="content" style="background-color:#FFFF00;height:200px;width:300px;float:left;">
        中间信息
      </div>
      <div id="right" style="background-color:#000000;color:#FFFFFF;height:200px;width:100px;float:right;">
          右侧信息
      </div>
      <div id="bool" style="background-color:#F0F8FF;clear:both;text-align:center;">
        整体布局测试
      </div>
  </div>
</body>
</html>
```


<html>
<head>
<meta charset="utf-8">
<title>Demo</title>
</head>
<body>
	<div id="maxname" style="width:500px;">
      <div id="hname" style="background-color:#666FFF;">
        <h1 style="margin-bottom:0;text-align:center;">Div布局实例</h1>
      </div>
      <div id="menu" style="background-color:#FFFFFF;height:200px;width:100px;float:left;">    
          菜单<br/>
          num1<br/>
          num2<br/>
          num3
      </div>
      <div id="content" style="background-color:#FFFF00;height:200px;width:300px;float:left;">
        中间信息
      </div>
      <div id="right" style="background-color:#000000;color:#FFFFFF;height:200px;width:100px;float:right;">
          右侧信息
      </div>
      <div id="bool" style="background-color:#F0F8FF;clear:both;text-align:center;">
        整体布局测试
      </div>
  </div>
</body>
</html>



## HTML 表单  

表单是一个包含表单元素的区域。

表单元素是允许用户在表单中输入内容,比如：文本域(textarea)、下拉列表、单选框(radio-buttons)、复选框(checkboxes)等等。  

表单的构建用`<form>`元素  

### 输入元素  

多数情况下被用到的表单标签是输入标签（`<input>`）。输入类型是由类型属性（type）定义的。  

#### 文本域（Text Fields）  

文本域通过`<input type="text">` 标签来设定，用户可以在表单中键入字母、数字等内容  

```html
<form>
First name: <input type="text" name="firstname"><br>
Last name: <input type="text" name="lastname">
</form>
```

<form>
First name: <input type="text" name="firstname"><br>
Last name: <input type="text" name="lastname">
</form>

#### 密码字段  

```html
<form>
Password: <input type="password" name="pwd">
</form>
```

<form>
Password: <input type="password" name="pwd">
</form>

#### 单选按钮（Radio Buttons）  

```html
<form>
<input type="radio" name="sex" value="male">Male<br>
<input type="radio" name="sex" value="female">Female
</form>
```

<form>
<input type="radio" name="sex" value="male">Male<br>
<input type="radio" name="sex" value="female">Female
</form>

#### 复选框（Checkboxes）  

```html
<form>
<input type="checkbox" name="vehicle" value="Bike">I have a bike<br>
<input type="checkbox" name="vehicle" value="Car">I have a car 
</form>
```

<form>
<input type="checkbox" name="vehicle" value="Bike">I have a bike<br>
<input type="checkbox" name="vehicle" value="Car">I have a car 
</form>

#### 提交按钮(Submit Button)  

`<input type="submit">` 定义了提交按钮. 当用户单击确认按钮时，表单的内容会被传送到另一个文件。表单的动作属性定义了目的文件的文件名。由动作属性定义的这个文件通常会对接收到的输入数据进行相关的处理。:  

```html
<form name="input" action="html_form_action.php" method="get">
Username: <input type="text" name="user">
<input type="submit" value="Submit">
</form>
```

<form name="input" action="html_form_action.php" method="get">
Username: <input type="text" name="user">
<input type="submit" value="Submit">
</form>

### HTML 表单标签  

| 标签         | 描述                                         |
| ------------ | -------------------------------------------- |
| `<form>`     | 定义供用户输入的表单                         |
| `<input>`    | 定义输入域                                   |
| `<textarea>` | 定义文本域 (一个多行的输入控件)              |
| `<label>`    | 定义了 `<input>` 元素的标签，一般为输入标题  |
| `<filedset>` | 定义了一组相关的表单元素，并使用外框包含起来 |
| `<legend>`   | 定义了 `<fieldset>` 元素的标题               |
| `<select>`   | 定义了下拉选项列表                           |
| `<optgroup>` | 定义选项组                                   |
| `<option>`   | 定义下拉列表中的选项                         |
| `<button>`   | 定义一个点击按钮                             |
| `<datalist>` | 指定一个预先定义的输入控件选项列表           |
| `keygen`     | 定义了表单的密钥对生成器字段                 |
| `<output>`   | 定义一个计算结果                             |

## HTML 框架  

通过使用框架，你可以在同一个浏览器窗口中镶嵌其他页面。  

```html
<iframe src="demo_iframe.htm" width="200" height="200" frameborder="0"></iframe>
```

## HTML 脚本   

`<script>` 标签用于定义客户端脚本，比如 JavaScript。`<script>` 元素既可包含脚本语句，也可通过 src 属性指向外部脚本文件。`<noscript>` 标签提供无法使用脚本时的替代内容，比方在浏览器禁用脚本时，或浏览器不支持客户端脚本时。`<noscript>`元素可包含普通 HTML 页面的 body 元素中能够找到的所有元素。  

```html
<script>
document.write("Hello World!")
</script>
<noscript>抱歉，你的浏览器不支持 JavaScript!</noscript>
```

-------------------------------------------------------------------

### Reference    

[菜鸟教程](https://www.runoob.com/html/html-tutorial.html)

