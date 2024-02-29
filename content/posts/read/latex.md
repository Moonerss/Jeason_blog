---
title: "Latex 公式语法参考"
date: 2021-10-12T21:16:51+08:00
lastmod: 2024-02-28T21:16:51+08:00
author: "Jeason"
categories:
  - latex
tags:
  - latex
description: "本文介绍了latex公式的基本组成和各种用法"
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
math: true
cover:
    image: "https://source.unsplash.com/random/400x200?code" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
    hidden: true         # 隐藏cover图片，但是不在格式化数据中隐藏
    hiddenInList: false   # 在列表页和文章内容页隐藏cover图片
    hiddenInSingle: true # 只在文章内容页隐藏cover图片
---
## Mathjax简介

Mathjax是一款运行在浏览器中的开源数学符号渲染引擎，使用MathJax可以方便的在浏览器中显示数学公式，不需要使用图片。

## 基本语法

+ 在正文中同一行内插入LaTex公式用 `$...$`来定义

  + 例如公式 `$\sum_{i=0}^N\int_{a}^{b}g(t,i)\text{d}t$`可以显示为$\sum_{i=0}^N\int_{a}^{b}g(t,i)\text{d}t$
+ 另起一行显示LaTex公式则需要使用 `$$...$$`来表示

  + 例如公式 `$$W_G^{mn}=max\{0,W_G.\xi_G(f_G^m,f_G^n)\}$$`可以显示为
    $$
    W_G^{mn}=max\{0,W_G.\xi_G(f_G^m,f_G^n)\}
    $$

## 常用字符表示

### 希腊字母

|     显示     |   命令   |    显示    |  命令  |
| :----------: | :------: | :--------: | :----: |
|  $\alpha$  |  \alpha  | $\beta$ | \beta |
|  $\gamma$  |  \gamma  | $\delta$ | \delta |
| $\epsilon$ | \epsilon | $\zeta$ | \zeta |
|   $\eta$   |   \eta   | $\theta$ | \theta |
|  $\iota$  |  \iota  | $\kappa$ | \kappa |
| $\lambda$ | \lambda |  $\mu$  |  \mu  |
|   $\nu$   |   \nu   |  $\xi$  |  \xi  |
|   $\pi$   |   \pi   |  $\rho$  |  \rho  |
|  $\sigma$  |  \sigma  |  $\tau$  |  \tau  |
| $\upsilon$ | \upsilon |  $\phi$  |  \phi  |
|   $\chi$   |   \chi   |  $\psi$  |  \psi  |
|  $\omega$  |  \omega  |            |        |

> + 如果需要大写希腊字母，只需将命令首字母大写即可。如 `$\Gamma$`表示$\Gamma$
> + 如果需要斜体则添加上 `var`即可。如 `$\varGamma$`表示$\varGamma$

### 关系运算符

|    显示    |  命令  |    显示    |  命令  |
| :---------: | :-----: | :--------: | :-----: |
|  $\mid$  |  \mid  | $\nmid$ |  \nmid  |
|  $\cdot$  |  \cdot  |  $\leq$  |  \leq  |
|  $\geq$  |  \geq  |  $\neq$  |  \neq  |
| $\approx$ | \approx | $\equiv$ | \equiv |
|  $\prec$  |  \prec  |    ⪯⪯    | \preceq |

### 算术运算符

|    显示    |  命令  |    显示    |  命令  |
| :---------: | :-----: | :---------: | :-----: |
|   $\pm$   |   \pm   |   $\mp$   |   \mp   |
| $\times$ | \times |  $\ast$  |  \ast  |
|  $\star$  |  \star  |  $\circ$  |  \circ  |
| $\bullet$ | \bullet |  $\cdot$  |  \cdot  |
|  $\div$  |  \div  |  $\sum$  |  \sum  |
|  $\prod$  |  \prod  | $\coprod$ | \coprod |

### 上下标

+ 上标用 `^`
+ 下标用 `_`

### 矢量

+ `\vec{a}`显示为$\vec{a}$
+ `\overrightarrow{xy}`显示为$\overrightarrow{xy}$

### 字体

+ 打印机字体Typewriter：`\mathtt{A}`显示为$\mathtt{A}$
+ 黑板粗体字Blackboard Bold：`\mathbb{A}`呈现为$\mathbb{A}$
+ 无衬线字体Sans Serif：`\mathsf{A}`呈现为$\mathsf{A}$
+ 手写体:`\mathscr{A}`呈现为$\mathscr{A}$
+ 罗马字体:`\mathrm{A}`呈现为$\mathrm{A}$

### 括号

+ 小括号：`()`
+ 中括号：`[]`
+ 尖括号：`\langle` `\rangle`分别显示为$\langle$和$\rangle$
+ 自适应括号：`\left(...\right)`可以自动适配周围公式环境

> 可以比较以下公式:
>
> 1. `(\frac{x}{y})`:$(\frac{x}{y})$
> 2. `\left(\frac{x}{y}\right)`:$\left(\frac{x}{y}\right)$

### 求和\极限\积分

+ 求和:`\sum` 如:`\sum_{i=1}^n{a_i}`显示为$\sum_{i=1}^n{a_i}$
+ 极限:`\lim` 如:`\lim_{x \to 0}`显示为$\lim_{x \to 0}$
+ 积分:`\lint` 如: `\int_0^xf(x)dx`显示为$\int_0^xf(x)dx$

### 分式和根式

+ 分式: `\frac`
+ 根式: `\sqrt`

### 特殊函数

+ `\sin` $\sin$
+ `\cos` $\cos$
+ `\ln` $\ln$
+ `\max` $\max$

### 空格

LaTex语法空格需要转义

+ `a\ b`表示$a\ b$
+ `a\quad b`表示$a\quad b$

### 矩阵

+ 矩阵起始用 `\begin{matrix}`,  结束用 `\end{matrix}`
+ 每一行末尾用 `\\\\`标记, 每个元素用 `$`分割

例子:

```mathematica
$$
\begin{matrix}
1&0&0\\
0&1&0\\
0&0&1\\
\end{matrix}
$$
```

$$
\begin{matrix}
1&0&0\\\
0&1&0\\\
0&0&1\\\
\end{matrix}
$$

### 矩阵边框

+ 与普通矩阵的写法类似,不过在起始和末尾处使用不同的标记进行替换

  + `pmatrix`:小括号包括的矩阵
  + `bmatrix`:中括号包括的矩阵
  + `Bmatrix`:大括号包括的矩阵
  + `vmatrix`:单竖线包括的矩阵
  + `Vmatrix`:双竖线包括的矩阵
  + 省略符号:

    + `\cdots`:横省略号
    + `\vdots`:竖省略号
    + `\ddots`:斜省略号

例子:

```mathematica
$$
\begin{bmatrix}
{a_{11}}&{a_{12}}&{\cdots}&{a_{1n}}\\
{a_{21}}&{a_{22}}&{\cdots}&{a_{2n}}\\
{\vdots}&{\vdots}&{\ddots}&{\vdots}\\
{a_{m1}}&{a_{m2}}&{\cdots}&{a_{mn}}\\
\end{bmatrix}
$$

```

$$
\begin{bmatrix}
{a_{11}}&{a_{12}}&{\cdots}&{a_{1n}}\\
{a_{21}}&{a_{22}}&{\cdots}&{a_{2n}}\\
{\vdots}&{\vdots}&{\ddots}&{\vdots}\\
{a_{m1}}&{a_{m2}}&{\cdots}&{a_{mn}}\\
\end{bmatrix}
$$

### 方程组

+ 以 `cases`进行声明

例子:

```mathematica
$$
\begin{cases}
a_1x+b_1y+c_1z=d_1\\
a_2x+b_2y+c_2z=d_2\\
a_3x+b_3y+c_3z=d_3\\
\end{cases}
$$
```

$$
\begin{cases}
a_1x+b_1y+c_1z=d_1\\
a_2x+b_2y+c_2z=d_2\\
a_3x+b_3y+c_3z=d_3\\
\end{cases}
$$

### 公式编号

+ 用 `\tag`标记

例子:

```mathematica
f(x)=x\tag{1}
```

$$
f(x)=x\tag{1}
$$
