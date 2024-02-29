---
title: "PCA分析原理"
date: 2021-11-27T21:14:44+08:00
lastmod: 2024-02-28T21:14:44+08:00
author: "Jeason"
categories:
  - Stats
tags:
  - Stats
  - StatQuest
description: "本文介绍了PCA分析的原理和基本思想，以及在R语言中的实现过程"
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


## PCA的基本思想

我们先从最简单的案例入手，先看下面的一堆绿色的圆圈，如下所示：	

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p1.jpg' width = '80%' heigh = '80%'>

我们可以把这些绿色圆圈当成一群正常的细胞,虽然这一群细胞看上去是一样的（至少表面上一样），但是，它们本质上也有可能不一样，例如像下面的这个样子：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p2.jpg' width = '80%' heigh = '80%'>

这群细胞分成了三群，不过从直观上来看，你太不容易能看出这一群细胞划分成了三群。

为了能够严格地划分这些细胞，我们可以对这些细胞进行测序，确认这些细胞中有哪些基因处于活跃状态，我们就能知道这些细胞的功能，这样就知道了这些细胞的种类，当然了，如果这是一群人，我们可以测量他们的身高，血压等，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p3.jpg' width = '80%' heigh = '80%'>

现在我们测了这些细胞的基因，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p4.jpg' width = '80%' heigh = '80%'>

假设，我们此时没有那么多细胞，只有2个细胞，那么我们就可以把这两个细胞的所有基因画在一个坐标上，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p5.jpg' width = '80%' heigh = '80%'>

就像上图显示的这样，基因1在细胞1中的高表达，而在细胞2中低表达，而基因9正好相反，在细胞1中低表达，而在细胞2中高表达，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p6.jpg' width = '80%' heigh = '80%'>

从我们所列出的数据来看，细胞1和细胞2的基因表达水平很多呈相反的水平，那么我们很容易就知道，这两个细胞必定不是同一种类的细胞，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p7.jpg' width = '80%' heigh = '80%'>

那么现在假设我们有3个细胞，现在利用二维坐标系分别绘制出这三个细胞两两之间的基因表达水平，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p8.jpg' width = '80%' heigh = '80%'>

我们可以直接把它们放到一个三维坐标系中进行比较，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p9.jpg' width = '80%' heigh = '80%'>

在这个三维坐标系中，它的三个坐标轴分别表示细胞1，细胞2，细胞3，我们通过旋转坐标轴就可以发现每两个细胞之间的关系。

现在把问题再进一步，如果我们有4个细胞，比较直观的手段就是分别画出这4个细胞两两的平面坐标系，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p10.jpg' width = '80%' heigh = '80%'>

或者是画出一个四维的坐标系，像下面的这个样子：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p11.jpg' width = '80%' heigh = '80%'>

但是随着细胞数目越来越多，上述的方法已经不太能够适用于计算和展示，此时我们就需要使用主成分分析（PCA）对细胞进行分类

PCA可以将不同细胞之间的关系转换到一个平面坐标系中，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p12.jpg' width = '80%' heigh = '80%'>

此时我们可以发现，通过PCA这种方法，可以把一群细胞中高度相关的细胞聚集起来，像下面的这个样子：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p13.jpg' width = '80%' heigh = '80%'>

为了能够更直观地说明这种分析方法，我们可以把不同的细胞换成不同的颜色，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p14.jpg' width = '80%' heigh = '80%'>

此时，我们再回到最初的问题上来，通过PCA方法可以把一群细胞中高度相关的细胞给区分开来，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p15.jpg' width = '80%' heigh = '80%'>

## PCA的展示形式

一般我们使用散点图的形式来展示PCA的结果，在PCA图中，它的坐标轴是按重要性进行排序的。其中PC1是第一主成分轴，它的重要性要强于PC2，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p16.jpg' width = '80%' heigh = '80%'>

假如一个PCA长得是下面的这个样子，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p17.jpg' width = '80%' heigh = '80%'>

此时，如果左上角的绿点与左下角的红点的距离是d1，右下角的黄点与左下角红点的距离是d2，如果d1=d2的话，那么我们就可以认为，黄色点与红色点的差异要大于绿色点与红色点的差异（因为PC1的坐标是第一主成分），如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p18.jpg' width = '80%' heigh = '80%'>

在我们了解了PCA之后，我们应该知道，PCA仅仅是一种划分不同数据类型的方法之一，此外，还有其他基于降维的统计学方法（dimension reduction），例如热图，t-SNE图，多维缩放（multi-dimensional scaling，MDS）等，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p19.jpg' width = '80%' heigh = '80%'>

## PCA的计算原理

### 简单示例

在进行主成分分析(PCA)时，我们一般采用奇异值分解（singular value decomposition，SVD）的方法进行计算。我们先看一个简单的案例，在这个案例中，我们检测了6只不同小鼠的2个基因，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p20.jpg' width = '80%' heigh = '80%'>

如果我们只检测1个基因的话（Gene 1），那么我们根据基因1表达的情况，把小鼠的绘制到数轴上，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p22.jpg' width = '80%' heigh = '80%'>

其中小鼠1，小鼠2，小鼠3的Gene 1表达水平比较高，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p23.jpg' width = '80%' heigh = '80%'>

而小鼠4，小鼠5，小鼠6的Gene 1水平则较低，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p24.jpg' width = '80%' heigh = '80%'>

虽然这个图形比较简单，但是，我们从中还是能得到一些信息的，例如小鼠1，小鼠2和小鼠3比较接近，小鼠4，小鼠5和小鼠6比较接近，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p25.jpg' width = '80%' heigh = '80%'>

如果我们检测了2个基因，那么我们可以绘制一个二维坐标系，横轴是Gene 1，纵轴是Gene 2，那么小鼠1，小鼠2和小鼠3会聚在一起，小鼠4，小鼠5和小鼠6会聚在一起，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p26.jpg' width = '80%' heigh = '80%'>

如果我们检测了3个基因，那么我们可以绘制三维的坐标系，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p27.jpg' width = '80%' heigh = '80%'>

再进一步，如果我们检测了4个基因，此时我们很难绘制出四维的坐标系，那么我们就需要进行PCA分析了，PCA可以把超过4个的基因降维成二维的坐标系，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p28.jpg' width = '80%' heigh = '80%'>

在这个PCA的二维坐标系中，我们可以发现，小鼠4、小鼠5和小鼠6是一类，小鼠1，小鼠2和小鼠3是一类，它们的各自的基因表达模式也类似，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p29.jpg' width = '80%' heigh = '80%'>

PCA在对数据进行聚类（clustering）时有很大的价值，例如，经过PCA分析，在它的二维坐标轴上我们可以发现，Gene 3在x轴上对样本的区分有贡献最大，如图所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p30.jpg' width = '80%' heigh = '80%'>

### PCA计算

为了理解PCA是如何进行计算的，我们还回最初2个基因的案例上来，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p31.jpg' width = '80%' heigh = '80%'>

首先，我们根据所有的样本的这两个基因的表达情况绘制出它们的平面坐标系。然后，计算Gene 1表达水平的均值，如下图x轴上红叉的所在位置如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p32.jpg' width = '80%' heigh = '80%'>

同样的，计算Gene 2表达水平的均值，如下图中y轴上红叉的所在位置，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p33.jpg' width = '80%' heigh = '80%'>·

有了Gene 1和Gene 2的这两个数值，我们就得到了整体数值的中心。我们有了这个点（蓝叉所在位置）后，我们此时就不需要再看原始数据了如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p34.jpg' width = '80%' heigh = '80%'>

我们把所有的数据点各左下角移动，将数据的中心与原点(0,0)重合，就像下面的这个样子，从左图移成右图的样子：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p35.jpg' width = '80%' heigh = '80%'>

此时，虽然所有的数据点都移动了，但是每个点之间的相对距离是不变的，整体数据的中心就变成了原点(0,0)，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p36.jpg' width = '80%' heigh = '80%'>

我们可以找一条通过原点的直线，然后旋转这条直线（红色虚线），使这条直线尽量匹配这些数据，最匹配数据的直线就是下面的这个样子：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p37.jpg' width = '80%' heigh = '80%'>
<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p38.jpg' width = '80%' heigh = '80%'>
<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p39.jpg' width = '80%' heigh = '80%'>

#### PC1 计算思路

那么我们如何得到这条最优的曲线呢？我们可以先回到直线原始的位置
<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p40.jpg' width = '80%' heigh = '80%'>

为了准确地找出最佳匹配所有数据的直线，PCA会将所有数据点都役射到这条直线上来，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p41.jpg' width = '80%' heigh = '80%'>

此时，可以计算这些数据点到投射到这条直线上的距离，并且使这些距离最小，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p42.jpg' width = '80%' heigh = '80%'>

除了要保持数据点到直线的距离最小外，还要计算所有数据点投射到这条直线上的点（图中绿叉位置所在点）到原点的距离，使这个距离最大，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p43.jpg' width = '80%' heigh = '80%'>

当直线进行旋转，这些数据点到这条直线的距离是在缩小的，同时这些数据点投射到直线上的点到原点的距离是加大的：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p44.jpg' width = '80%' heigh = '80%'>
<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p45.jpg' width = '80%' heigh = '80%'>

为了更好地以数学思维来理解这个过程，此时我们先看一个点的情况，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p46.jpg' width = '80%' heigh = '80%'>

这个点是固定的，它到原点的距离也是固定的：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p47.jpg' width = '80%' heigh = '80%'>

当直线（红色虚线）在旋转的过程中，这个点到原点的距离是不变的：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p48.jpg' width = '80%' heigh = '80%'>

当直线旋转到某一位置时，我们将这个点投射到直线上，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p49.jpg' width = '80%' heigh = '80%'>

此时，我们会得到一个直角，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p50.jpg' width = '80%' heigh = '80%'>

此时，我们把这个直角三角形的三条边命名为 `a`，`b`，`c`，那么根据勾股定理，我们就会得到这个公式，即$a^2 = b^2 + c^2$

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p52.jpg' width = '80%' heigh = '80%'>

三角形的斜边 `a` 是不会变的，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p53.jpg' width = '80%' heigh = '80%'>

如果直角边 `b` 变大的话, 那么直角边 `c`就会变小:

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p54.jpg' width = '80%' heigh = '80%'>
<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p55.jpg' width = '80%' heigh = '80%'>

反之，也成立。而PCA计算的则是降低直角边 `b`，也就是降低数据点投射到直线上的距离，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p56.jpg' width = '80%' heigh = '80%'>

同时，PCA还要保证投到到直线上的点到原点的距离最大，也就是说要求c边最大，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p57.jpg' width = '80%' heigh = '80%'>

通过上面利用直角三角形的解释，就比较容易理解PCA计算最佳匹配直线的思路了。此时，再回到起点，将所有的数据点投射到直线上，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p58.jpg' width = '80%' heigh = '80%'>

然后计算某个点到原点为的距离，我们把它命名为 `d1`，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p59.jpg' width = '80%' heigh = '80%'>

同理，计算第2点投射到直线上的点到原点的距离，命名为 `d2`，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p60.jpg' width = '80%' heigh = '80%'>

依此类推，计算剩余的投射点到原点的距离

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p61.jpg' width = '80%' heigh = '80%'>

然后把这些值的平方加起来，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p62.jpg' width = '80%' heigh = '80%'>

我们旋转直线，直到最终这个 `SS` 的距离最大（这个距离最大，也就是说数据点到直线的距离最小）为止

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p63.jpg' width = '80%' heigh = '80%'>

最终，得到了我们想要的直线，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p64.jpg' width = '80%' heigh = '80%'>

此时，我们称这条直线为第一主成分（Principal Component 1，简称PC1）

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p65.jpg' width = '80%' heigh = '80%'>

有了这个直线，它的斜率就很容易计算出来了，经计算，PC1的斜率为0.25

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p66.jpg' width = '80%' heigh = '80%'>

它表示每当我们在Gene 1（x轴坐标）前进4个单位时，在这个直线上的数据点在Gene 2上就增加1个单位

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p67.jpg' width = '80%' heigh = '80%'>

如果按照生物学意义来理解就是，这几个样本在Gene 1坐标轴上更加分散，而在Gene 2上的分散程度则较小

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p68.jpg' width = '80%' heigh = '80%'>
<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p69.jpg' width = '80%' heigh = '80%'>

我们可以根据鸡尾酒配方（cocktail recipe）来思考这个PC1，为了生成PC1，我们可以加入4份的Gene 1，再加入1份的Gene 2，另外，我们可以发现，Gene 1与Gene 2的比值是4，它就说明在描述数据的分散程度方面，Gene 1这个基因更加重要

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p70.jpg' width = '80%' heigh = '80%'>

但在数学上，把这种鸡尾酒配方（cocktail recipe）称为是Gene 1和Gene 2的线性组合（linear combination），在统计学上，描述PC1也可以这样，即“PC1是几个变量的一个线性组合”，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p71.jpg' width = '80%' heigh = '80%'>

#### PC1 计算

接下来我们需要计算PC1的特征值。对于PC1来说，它的斜率是0.25，那么我们在Gene 1增加4个单位，在Gene 2上就会增加1个单位

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p72.jpg' width = '80%' heigh = '80%'>

根据勾股定理，我们就可以计算出红色箭头的长度，长度为4.12

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p73.jpg' width = '80%' heigh = '80%'>

我们使用奇异值分解（Singular-value decomposition，SVD）进行PCA计算时，需要把这个红色箭头定义为1个单位

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p74.jpg' width = '80%' heigh = '80%'>

为了将这个红色箭头定义为1个单位，那么我们就要把这个三角形的各个边长都除以4.12

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p75.jpg' width = '80%' heigh = '80%'>

除以4.12后，各个三角形的连长就成了这个样子

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p76.jpg' width = '80%' heigh = '80%'>

也就是说三角形的三个边长分别变成了1，0.242，0.97，不过构成PC1的线性组合，即Gene 1和Gene 2的比值还是不变的，仍旧为4，那么此时我们就可以说取0.97份的Gene 1和0.242份的Gene 2，就构成了PC1，如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p77.jpg' width = '80%' heigh = '80%'>

回想一下我们计算PC1的整个过程：

1. 原始数据；
2. 最佳匹配直线；
3. 单位向量计算

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p78.jpg' width = '80%' heigh = '80%'>

图中的红色箭头是1个长度单位，它由0.97个Gene 1和0.242个Gene 2构成，称为PC1的奇异向量（Singular vector），或者是特征向量（Eigenvector），而每个基因的比例则被称为载荷得分（Loading Scores）

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p79.jpg' width = '80%' heigh = '80%'>

此外，在PCA中，将SS（也就是各个点投射到最佳匹配的直线上的点到原点的距离的平方和）称为PC1的特征值（Eigenvalue）

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p80.jpg' width = '80%' heigh = '80%'>

PC1特征向量的值（Eigenvalue for PC1）的平方根被称为PC1的奇异值（Singular Value for PC1）

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p81.jpg' width = '80%' heigh = '80%'>

#### PC2 计算

接下来我们计算第二主成分（PC2），由于我们使用的二维坐标，PC2仅仅是一条通过了原点，并且与PC1垂直的直线，PC2并没有像PC1那样复杂的优化

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p82.jpg' width = '80%' heigh = '80%'>

由于PC2与PC1垂直，那么很容易地就能计算出PC2的斜率为-4，这也就是说，PC2是由-1个Gene 1和4个Gene 2构成的

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p83.jpg' width = '80%' heigh = '80%'>

当我们在PC2所在直线上定义一个单位长度的向量时，那么PC2就是由-0.242个Gene 1和0.97个Gene 2构成了，此时这个单位向量（蓝色箭头）就被称为PC2的奇异向量（Singular Vector）或特征向量（Eigenvector）

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p84.jpg' width = '80%' heigh = '80%'>

而-0.242和0.97就是PC2的载荷分数，它表示的就是，基因2相当于4位的基因1

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p85.jpg' width = '80%' heigh = '80%'>

PC2的特征值则是数据点投到到PC2上的点到原点的距离平方和

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p86.jpg' width = '80%' heigh = '80%'>

此时，PC1和PC2的计算结束，绘制最终的PCA图

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p87.jpg' width = '80%' heigh = '80%'>

然后旋转这个坐标，让PC1水平，PC2垂直

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p88.jpg' width = '80%' heigh = '80%'>

在这个新的坐标系中，图中黑色的叉就表示原始的样本6（Sample 6）

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p89.jpg' width = '80%' heigh = '80%'>

而Sample 6位于这个点上

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p90.jpg' width = '80%' heigh = '80%'>

同理，我们可以计算其他的点

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p91.jpg' width = '80%' heigh = '80%'>

### 各主成分的变异

此时，再讲一些术语，先回忆一下前面的PC1的特征值和PC2的特征值

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p92.jpg' width = '80%' heigh = '80%'>

我们把原始的数据点投射到主成分上

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p93.jpg' width = '80%' heigh = '80%'>

计算投射到PC1上的点到原点的距离

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p94.jpg' width = '80%' heigh = '80%'>

所有的这些数值的平方和就是PC1的特征值

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p95.jpg' width = '80%' heigh = '80%'>

我们也可以将特征值转为PC1到原点的变异，方法就是SS除以样本数减1

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p96.jpg' width = '80%' heigh = '80%'>

以这个案例为例说明，假如PC1的变异（Variation）为15，PC2的变异为3，这就表明主成分的总变异为18，那么也就是说PC1在总变异中所占的比值为83%（15/18=0.83）

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p97.jpg' width = '80%' heigh = '80%'>

而PC2占的总变异为17%（3/18=0.17）

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p98.jpg' width = '80%' heigh = '80%'>

此时，我们把这两个主成分所占的比例画出来，这种图与条形图很像，不过在PCA中，这叫碎石图（scree plot），每个矩形表示每个PC所占总变异的比值

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p99.jpg' width = '80%' heigh = '80%'>

## 复杂案例

我们看一个比较复杂的案例，在这个案例中，6只小鼠，检测了3个基因

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p100.jpg' width = '80%' heigh = '80%'>

按照前面的思路，找到数据的中心，并将其移到原点

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p101.jpg' width = '80%' heigh = '80%'>

找到最佳的匹配数据的直线，也就是PC1

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p102.jpg' width = '80%' heigh = '80%'>

不过这个PC1有3个成分，分别是0.62个Gene 1，0.15个Gene 2和0.77个Gene 3

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p103.jpg' width = '80%' heigh = '80%'>

其中，Gene3 是最主要的组成部分

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p104.jpg' width = '80%' heigh = '80%'>

然后寻找PC2，PC2通过原点，并与PC1垂直，

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p105.jpg' width = '80%' heigh = '80%'>

最终，PC2的成分为0.77个Gene 1，0.62个Gene 2和0.15个Gene 3

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p106.jpg' width = '80%' heigh = '80%'>

在PC2中，Gene 1是最主要的组成部分

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p107.jpg' width = '80%' heigh = '80%'>

最后，我们寻找PC3，PC3通过原点，同时与PC1和PC2垂直

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p108.jpg' width = '80%' heigh = '80%'>

如果我们有更多的基因，那么我们就会不断地寻找更多的主成分，也就是说不断地往这个坐标中加入与前一个主成分垂直的直线，从理论上讲，一个基因就有一个主成分（例如这个案例中，3个基因，最终找到了3个主成分），因此，主成分的数目要么跟变量相等，要么跟样本的数目相等

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p109.png' width = '80%' heigh = '80%'>

一旦计算出来了所有的主成分，那么我们就可以使用特征值来计算每个主成分的变异占总变异的比例

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p110.jpg' width = '80%' heigh = '80%'>

在这个案例中，PC1的变异占总变异的79%，PC2占15%，PC3占6%，把这些主成分绘制成碎石图，就是下图左图的样子

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p111.jpg' width = '80%' heigh = '80%'>

从碎石图中我们可以看到，PC1和PC2占了变异的绝大比例

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p112.jpg' width = '80%' heigh = '80%'>

这就表明了，在二维图中，我们基本上只使用PC1和PC2就能解释三维图中的数据，因为二维图中的PC1和PC2占据了整体的变异的94%

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p113.jpg' width = '80%' heigh = '80%'>

我们将三维图转换为二维图，画出PC1和PC2，以及原始数据点

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p114.jpg' width = '80%' heigh = '80%'>

将数据投射到这个新坐标的PC1上

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p115.jpg' width = '80%' heigh = '80%'>

再投射到PC2上

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p116.jpg' width = '80%' heigh = '80%'>

然后，我们把这个坐标旋转一下，方便查看

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p117.jpg' width = '80%' heigh = '80%'>

下面的这个点就是Sample 4对应的点

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p118.jpg' width = '80%' heigh = '80%'>

我们再来回顾一下这个案例，首先我们把所有的数据点绘制到三维坐标中

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p119.jpg' width = '80%' heigh = '80%'>

再计算主成分

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p120.jpg' width = '80%' heigh = '80%'>

再计算主成分的特征值与变异比例

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p121.jpg' width = '80%' heigh = '80%'>

最后，我们使用PC1和PC2这个二维坐标来描述原始数据

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p122.jpg' width = '80%' heigh = '80%'>

如果我们检测了6个样本的4个基因，那么我们并不需要绘制出4维坐标图

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p123.jpg' width = '80%' heigh = '80%'>

我们可以计算出它们的主成分，画出碎石图，其中我们可以发现，PC1和PC2占据了变异的90%，那么我们仅使用PC1和PC2构成的二维PCA图就可以表示原始数据

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p124.jpg' width = '80%' heigh = '80%'>

将原始数据分别投射到PC1和PC2上

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p125.jpg' width = '80%' heigh = '80%'>

下面的这两个点就对应了Sample 2

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p126.jpg' width = '80%' heigh = '80%'>
<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p129.jpg' width = '80%' heigh = '80%'>

那么绘制出所有的点，如下：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p130.jpg' width = '80%' heigh = '80%'>

如果我们遇到下面的这种碎石图，在这种碎石图中，PC3和PC4也占据了相当比例的总变异，那么我们仅使用2个主成分（也就是PC1和PC2）就无法精确地代表原始数据

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p131.jpg' width = '80%' heigh = '80%'>

不过，即使是有瑕玼的PCA二维图，也可以用于数据的分类

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p132.jpg' width = '80%' heigh = '80%'>

在这个图中，黑色的点仍然可以被视为比较接近的类似

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p133.jpg' width = '80%' heigh = '80%'>

## R语言计算PCA

计算PCA的工具很多，这里我们介绍一下用R来计算PCA，在R中，计算PCA的函数是 `prcomp()`，绘图包是 `ggplot2`，使用R，我们可以知道每个主成分的变异占总变异的比值，以及计算载荷得分（loading scores），发现哪个变量对图形的影响最大

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p134.jpg' width = '80%' heigh = '80%'>

### 数据集

```R
data.matrix <- matrix(nrow=100, ncol=10)
colnames(data.matrix) <- c(
  paste("wt",1:5,sep=""),
  paste("ko",1:5,sep=""))
rownames(data.matrix) <- paste("gene", 1:100, sep="")
for (i in 1:100){
  wt.values <- rpois(5, lambda = sample(x=10:1000, size=1))
  ko.values <- rpois(5, lambda = sample(x=10:1000, size=1))
  data.matrix[i,] <- c(wt.values, ko.values)
}
head(data.matrix)
```

查看生成的实例数据

```R
> head(data.matrix)
      wt1 wt2 wt3  wt4 wt5 ko1 ko2 ko3 ko4 ko5
gene1 423 423 419  423 372 582 589 563 534 566
gene2 905 971 967 1031 910  95  96  96  89  96
gene3 139 177 147  176 152 408 422 419 426 456
gene4 702 712 717  691 678 534 546 594 562 515
gene5 582 572 616  569 631 315 321 314 345 330
gene6 765 853 781  792 768 316 339 317 317 294
```

### 计算PCA

使用 `prcomp`函数来对数据集进行PCA运算，如下所示：

```
pca <- prcomp(t(data.matrix),scale=TRUE)
```

代码解释：由于 `prcomp`函数在默认情况下，会把行识别为样本，列识别为基因，而我们生成的数据集 `data.matrix`正好相反，因此用到了 `t`函数，它表示把原来的数据集转置一下，将其行与列交换。

`prcomp`计算的结果含有主成分分析的各种信息，通过 `str()`函数我们可以看一下有哪些东西，如下所示：

```R
> str(pca)
List of 5
 $ sdev    : num [1:10] 9.34 1.85 1.6 1.45 1.24 ...
 $ rotation: num [1:100, 1:10] 0.104 -0.107 0.106 -0.103 -0.106 ...
  ..- attr(*, "dimnames")=List of 2
  .. ..$ : chr [1:100] "gene1" "gene2" "gene3" "gene4" ...
  .. ..$ : chr [1:10] "PC1" "PC2" "PC3" "PC4" ...
 $ center  : Named num [1:100] 489 526 292 625 460 ...
  ..- attr(*, "names")= chr [1:100] "gene1" "gene2" "gene3" "gene4" ...
 $ scale   : Named num [1:100] 84.2 455.8 142.2 82.1 143.2 ...
  ..- attr(*, "names")= chr [1:100] "gene1" "gene2" "gene3" "gene4" ...
 $ x       : num [1:10, 1:10] -8.92 -9.15 -8.71 -8.75 -8.77 ...
  ..- attr(*, "dimnames")=List of 2
  .. ..$ : chr [1:10] "wt1" "wt2" "wt3" "wt4" ...
  .. ..$ : chr [1:10] "PC1" "PC2" "PC3" "PC4" ...
 - attr(*, "class")= chr "prcomp"
```

从这个结果中我们可以发现，有 `sdev`，`rotation`，`center`，`scale`，`x`。其中 `x`含有主成分的信息

我们可以发现，`x`中有10个主成分（这是因为有10个样本），第1主成分占据了原始数据总变异的大部分，第2主成分占据了总变异的次大部分等。为了绘制二维图，我们通常使用PC1和PC2（个别情况下也会使用PC2和PC3），如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p155.jpg' width = '80%' heigh = '80%'>

我们此时，使用 `x`的前2列（即PC1和PC2）绘制一个二维图，如下所示：

```R
plot(pca)
```

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p135.png' width = '80%' heigh = '80%'>

其中，`pca$x[,1]`是PC1，`pca$x[,2]`是PC2

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p137.jpg' width = '80%' heigh = '80%'>

10个样本，分别位于图形的左右两侧，此时我们再看一下PC1占的总变异的比例，如下所示：

```R
pca.var <- pca$sdev^2
pca.var.per <- round(pca.var/sum(pca.var)*100,1)
```

结果如下:

```R
> pca.var
 [1] 8.727308e+01 3.432450e+00 2.546956e+00 2.094293e+00 1.534746e+00 1.230408e+00
 [7] 8.259253e-01 6.435822e-01 4.185583e-01 1.134741e-29
> pca.var.per
 [1] 87.3  3.4  2.5  2.1  1.5  1.2  0.8  0.6  0.4  0.0
```

绘制出各个主成分对应的碎石图

```R
barplot(pca.var.per, 
        main="Scree Plot", 
        xlab="Principal Component", 
        ylab="Percent Variation")
```

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p138.png' width = '80%' heigh = '80%'>

上图中最长的那个就是PC1所占的总变异的比例，也就是说PC1就能在很大程度上解释两组数据的区分，我们在PC1和PC2的二维图上也能看出来，这两组数据之间主要是由PC1分开的

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p139.jpg' width = '80%' heigh = '80%'>

### ggplot2绘图

ggplot2也能绘制类似的图形

```R
pca.data <- data.frame(Sample=rownames(pca$x),
                       X=pca$x[,1],
                       Y=pca$x[,2])
# one column with the sample ids
# Tow columns for the X and Y coordinates for each sample
pca.data
ggplot(data=pca.data, aes(x=X, y=Y, label=Sample))+
  geom_text() +
  xlab(paste("PC1 - ", pca.var.per[1], "%", sep=""))+
  ylab(paste("PC2 - ", pca.var.per[2], "%", sep=""))+
  theme_bw()+
  ggtitle("My PCA Graph")
```

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p140.png' width = '80%' heigh = '80%'>

从这个图形中，我们可以发现，X轴上标记了PC1所占总变异的比例为87.3%。Y轴上标记了PC2所占总变异的比例为3.4%。

### loading scores计算

`prcomp()`函数计算的结果中含有loading score，即 `rotation`这一部分的内容

由于PC1占所了总变异的大部分，因此我们只需要看PC1中的loading score

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p141.jpg' width = '80%' heigh = '80%'>

由于loading_score有负数，因此我们需要用 `abs()`函数取它们的绝对值

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p142.jpg' width = '80%' heigh = '80%'>

取完绝对值，对所有的loading score进行排序，从高到低；这样我们就能筛选出排名前十的基因了

```R
loading_score <- pca$rotation[,1]
gene_scores <- abs(loading_score)
gene_score_ranked <- sort(gene_scores, decreasing = TRUE)

top_10_genes <- names(gene_score_ranked[1:10])
top_10_genes
```

```R
 [1] "gene10" "gene98" "gene34" "gene94" "gene95" "gene99" "gene76" "gene47" "gene49"
[10] "gene72"
```

```R
> pca$rotation[top_10_genes,1]
    gene10     gene98     gene34     gene94     gene95     gene99     gene76     gene47 
 0.1069736  0.1069637  0.1069535  0.1069478 -0.1069281  0.1069267  0.1069243 -0.1069209 
    gene49     gene72 
 0.1069161  0.1069037
```

## python计算PCA

利用Python也可以进行PCA分析，在这一部分中，我们会介绍如何这些内容：

1. 利用Python生成一些我们要分析的数据；
2. 利用 `sklearn`模块中的 `PCA()`来进行分析，其中 `sklearn`是 `scikit-learn`的简称，它是一个机器学习的模块，支持包括分类、回归、降维和聚类四大机器学习算法。还包含了特征提取、数据处理和模型评估三大模块；
3. 计算每个主成分占总变异的比例；
4. 利用 `matplotlib`来绘制PCA图；
5. 计算变量的loading scores。
   如下所示：

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p143.jpg' width = '80%' heigh = '80%'>

### 导入python模块

```Python
import pandas as pd 
# import pandas package,
# pandas is short for "panel data"
import numpy as np
# numpy package will generate random numbers 
import random as rd
# random package will generate an example dataset
from sklearn.decomposition import PCA
from sklearn import preprocessing
# usr a function PCA() from sklearn.decomposition 
# PCA() use to perform PCA
# preprocessing uses to scaling the data before perform PCA
import matplotlib.pyplot as plt
# draw PCA graphd
```

### 生成模拟数据

```Python
genes = ['gene' + str(i) for i in range(1, 101)]
wt = ['wt' + str(i) for i in range(1,6)]
ko = ['ko' + str(i) for i in range(1,6)]
data = pd.DataFrame(columns=[*wt, *ko], index=genes)
for gene in data.index:
    data.loc[gene, 'wt1':'wt5'] = np.random.poisson(lam=rd.randrange(10,1000),size=5)
    data.loc[gene, 'ko1':'ko5'] = np.random.poisson(lam=rd.randrange(10,1000),size=5)
print(data.head())
print(data.shape)
# generate simulation dataset
```

### PCA分析

在进行PCA分析之前，先对数据进行中心化（我的理解是Z转换，转换后的数据均值是0，标准差是1）

```Python
scaled_data = preprocessing.scale(data.T)
```

需要注意的是，上述代码中使用了 `data.T`，这是对表达的数据集进行转换，将其行与列交换，跟R中类似。此处我们使用的 `sklearn`包中的 `preprocessing`函数，我们还可以使用另外的函数进行转换，就是 `StandardScaler().fit_transform(data.T)`。第二个函数在机器学习使用更加广泛

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p144.jpg' width = '80%' heigh = '80%'>

需要注意的是，`sklearn`中利用 `preprocessing.scale`计算变异与R中利用 `scale()`和 `prcomp()`计算变异的公式略有不同，区别就在于前者的分子是测量值的数目，后者是这个数目减1

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p145.jpg' width = '80%' heigh = '80%'>

后者的分子是测量值的数目减1，这会导致最终的结果变大，这是一种无偏估计

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p146.jpg' width = '80%' heigh = '80%'>

不过这两种方法都不影响PCA的分析

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p147.jpg' width = '80%' heigh = '80%'>

它的局限就在于，它们会对最终的绘图造成一定的影响，因为最终绘图中的坐标来源于scaled后的数值乘以loading scores

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p148.jpg' width = '80%' heigh = '80%'>

继续Python计算PCA，此时我们要创建一个 `PCA`对象, 这里要提一下，`PCA`不仅是一个函数，能够返回计算结果，在 `sklearn`中，它还用于创建一个对象，使用一个数据集进行训练，进行对另外一个数据集进行计算。不过在这里，我们只用 `PCA`计算这个数据集（我们不涉及机器学习的环境设置），其他的步骤非常繁琐，但也由此，我们也能了解一下机器学习的内容

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p149.jpg' width = '80%' heigh = '80%'>

计算PCA的所有结果，例如loading scores和每个主成分的变量占总变异的比例

```Python
>>> pca = PCA()
>>> pca.fit(scaled_data)
PCA(copy=True, iterated_power='auto', n_components=None, random_state=None,
  svd_solver='auto', tol=0.0, whiten=False)
```

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p150.jpg' width = '80%' heigh = '80%'>

接着，基于loading scores和scaled data来生成PCA图形中的坐标系

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p151.jpg' width = '80%' heigh = '80%'>

### 绘制图像

绘制碎石图

```Python
per_var = np.round(pca.explained_variance_ratio_*100,decimals=1)
# 计算每个主成分所占总变量的比例
labels = ['PC' + str(x) for x in range(1, len(per_var)+1)]
# 使用matplotlib来绘制相应的图形
plt.bar(x=range(1,len(per_var)+1), height=per_var, tick_label=labels)
plt.ylabel('Percentage of Explained Variance')
plt.xlabel('Principal Component')
plt.title('Scree Plot')
plt.show()
```

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p152.png' width = '80%' heigh = '80%'>

绘制PCA图

```Python
pca_df = pd.DataFrame(pca_data, index=[*wt, *ko], columns=labels)

plt.scatter(pca_df.PC1,pca_df.PC2)
plt.title('My PCA Graph')
plt.xlabel('PC1 - {0}%'.format(per_var[0]))
plt.ylabel('PC2 - {0}%'.format(per_var[1]))

for sample in pca_df.index:
    plt.annotate(sample, (pca_df.PC1.loc[sample],pca_df.PC2.loc[sample]))
plt.show()
```

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p153.jpg' width = '80%' heigh = '80%'>

从这张图中我们可以看到，wt组的数据集中在右侧，ko组都集中在了左侧

### 计算loading score

我们还可以计算一下PC1的loading scores，从而找出哪些基因在不同的分类方面发挥了主要作用

<img src = 'https://cdn.jsdelivr.net/gh/Moonerss/CDN/paper/pca/p154.jpg' width = '80%' heigh = '80%'>

我们要创建一个 `pandas`的 `series`对象，这个对象用于计算loading scores

```Python
loading_scores = pd.Series(pca.components_[0], index=genes)
# in python, first  number in index is 0, not 1
sorted_loading_scores = loading_scores.abs().sort_values(ascending=False)
# achieve absolute value using abs() function and sort them
top_10_genes = sorted_loading_scores[0:10].index.values
print(loading_scores[top_10_genes])
```

```Python
gene31   -0.107133
gene32    0.107131
gene34    0.107070
gene10   -0.107065
gene45    0.107065
gene28   -0.107064
gene30   -0.107062
gene39    0.107053
gene93   -0.107041
gene79    0.107036
dtype: float64s
```


