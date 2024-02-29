---
title: "Cox模型的R实现"
date: 2018-05-08T15:57:22+08:00
lastmod: 2024-02-29T15:57:22+08:00
author: "Jeason"
categories: 
  - R
  - Stats
tags: 
  - Stats
description: "本文介绍了在R语言如何实现Cox比例风险回归模型以及结果的解释"
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

### 载入需要的R包  

- **survival**用于计算生存分析  

- **survminer**用于可视化生存分析结果  

```R
library("survival")
library("survminer")
```  


### 计算COX model : coxph()  

`coxph` 可以用于构建计算 Cox proportional hazards regression model，常见用法如下：    

```R
coxph(formula, data, method)
```  


> formula : 是通过	`Surv`构建的生存对象 ，如：*Surv(time, event)*  
>
>  data：包含变量的data.frame  
>
> method：计算各项指标所用的检验方法   



### Univariate Cox regression  

以`survival`包提供的`lung`数据为例，进行单变量的cox回归分析   

```R
res.cox <- coxph(Surv(time, status) ~ sex, data = lung)
res.cox
```  


```R
Call:
coxph(formula = Surv(time, status) ~ sex, data = lung)
      coef exp(coef) se(coef)     z      p
sex -0.531     0.588    0.167 -3.18 0.0015
Likelihood ratio test=10.6  on 1 df, p=0.00111
n= 228, number of events= 165 
```  

使用`summary`函数显示更加详细的结果：    

```R
summary(res.cox)
```  


```R
Call:
coxph(formula = Surv(time, status) ~ sex, data = lung)
  n= 228, number of events= 165 
       coef exp(coef) se(coef)      z Pr(>|z|)   
sex -0.5310    0.5880   0.1672 -3.176  0.00149 **  
---
Signif. codes:  0 '*' 0.001 '' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    exp(coef) exp(-coef) lower .95 upper .95
sex     0.588      1.701    0.4237     0.816
Concordance= 0.579  (se = 0.022 )
Rsquare= 0.046   (max possible= 0.999 )
Likelihood ratio test= 10.63  on 1 df,   p=0.001111
Wald test            = 10.09  on 1 df,   p=0.001491
Score (logrank) test = 10.33  on 1 df,   p=0.001312
```  


结果的各项指标解释如下：  

1. ***Statistical significance***. z值给出了Wald统计值。它对应于每个回归系数与其标准误差的比率（z = coef / se（coef））。 wald统计量评估给定变量的系数（β）是否在统计学上显著，由上可知，变量性别具有高度统计上显著的系数。  
2. ***The regression coefficients***. Cox模型结果中要注意的第二个特征是回归系数（coef）的符号。如果回归系数为正值，说明死亡风险高，预后差；为负值，则风险低，预后好。 R给出的是第二组相对于第一组的风险比（HR），即女性对男性。在这些数据中，性别的β系数= -0.53表明女性的死亡风险（低存活率）低于男性。 
3. ***Hazard ratios***. 指数系数（exp（coef）= exp（-0.53）= 0.59）也称为风险比，给出协变量的效应大小。 例如，女性（性别= 2）的风险降低了0.59倍，即41％。 女性与预后良好相关。  
4. ***Confidence intervals of the hazard ratios***. 给出了风险比（exp（coef））的95％置信区间的上限和下限。  
5. ***Global statistical significance of the model***.  三个检验模型的P值：The likelihood-ratio test, Wald test, and score logrank statistics.这三种方法是类似的，如果样本量足够大，则三个方法会得到相同的结果，当样本量较小时，The likelihood-ratio test能得到较好的结果。   



### Multivariate Cox regression analysis   



当我们纳入多个变量时，我们可以这样操作：   

```R
res.cox <- coxph(Surv(time, status) ~ age + sex + ph.ecog, data =  lung)
summary(res.cox)
```  

```R
Call:
coxph(formula = Surv(time, status) ~ age + sex + ph.ecog, data = lung)
  n= 227, number of events= 164 
   (1 observation deleted due to missingness)
             coef exp(coef)  se(coef)      z Pr(>|z|)    
age      0.011067  1.011128  0.009267  1.194 0.232416    
sex     -0.552612  0.575445  0.167739 -3.294 0.000986 ***
ph.ecog  0.463728  1.589991  0.113577  4.083 4.45e-05 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
        exp(coef) exp(-coef) lower .95 upper .95
age        1.0111     0.9890    0.9929    1.0297
sex        0.5754     1.7378    0.4142    0.7994
ph.ecog    1.5900     0.6289    1.2727    1.9864
Concordance= 0.637  (se = 0.026 )
Rsquare= 0.126   (max possible= 0.999 )
Likelihood ratio test= 30.5  on 3 df,   p=1.083e-06
Wald test            = 29.93  on 3 df,   p=1.428e-06
Score (logrank) test = 30.5  on 3 df,   p=1.083e-06
```  



由上可知：在三种检验方法中，p值都是显著的。这表明了该模型的重要性。  



在多变量Cox分析中，协变量`sex`和`ph.ecog`仍然显着（p <0.05）。然而，协变量`age`并不显着（p = 0.23）。  
> 
> 
> 
> `sex`的p值为0.000986，危险比HR = exp（coef）= 0.58，表明患者性别与死亡风险降低之间有很强的关系。协变量的风险比可以解释为对风险的倍增效应。例如，将其他协变量保持不变，即女性（`sex`= 2）可将风险降低0.58倍，即降低42％。我们的结论是，女性与良好的预后相关。  
> 
> 
> 
> 类似地，`ph.ecog`的p值为4.45e-05，危险比HR = 1.59，表明`ph.ecog`值与死亡风险增加之间的强关系。保持其他协变量不变，`ph.ecog`的较高值与生存率较差相关。  
> 
> 
> 
> 相比之下，`age`的p = 0.23。风险比HR = exp（coef）= 1.01，95％置信区间为0.99至1.03。由于HR的置信区间包括1，因此这些结果表明年龄在调整`ph.ecog`和`sex`后对HR差异的贡献较小，并且仅趋向于显着性。例如，将其他协变量保持不变，额外的一年会导致每日死亡风险的因数为exp（beta）= 1.01或1％，这不是一个重大贡献。  
> 


### References:

Cox Proportional Hazards Model[http://www.sthda.com/english/wiki/cox-proportional-hazards-model]( http://www.sthda.com/english/wiki/cox-proportional-hazards-model)
