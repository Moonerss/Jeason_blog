---
title: "Shell编程规范"
date: 2019-10-14T20:11:36+08:00
lastmod: 2024-02-28T20:11:36+08:00
author: "Jeason"
categories: 
  - shell
tags: 
  - shell
description: "这里介绍了编写shell脚本的代码规范"
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

规范的编程格式既可以使代码更加整洁，又能帮助别人理解使用代码，下面是在使用shell脚本的过程中涉及到的一些代码编程规范  

## 命名  

1. 命名只能使用字母,数字和下划线,首个字符不能以数字开头。  
2. 中间不能有空格,不能使用标点符号,不能使用汉字,可以使用下划线 _ ,所以我们往往使用 _ 作为分词的标识 例如 user_name、city_id 等  
3. 不能使用bash里的关键字(可用help命令查看保留关键字)  
4. 脚本中的所有变量风格统一使用下划线命名风格  

> 目前关于命名使用较多的两种方式是`驼峰式` 和 `下划线式`，两种命名规则见仁见智  

```sh
local userName="Jeason"
local user_name="Jeason"
function log_info(){
  # todo
}
function logInfo(){
  # todo
}
function get_user_info(){
  # todo
}
function getUserInfo(){
  # todo
}
```

## 首行  

推荐使用`#!/usr/bin/env bash`，自动检测shell环境  

## 注释  

1. 除脚本首行外,所有以 `#` 开头的语句都将成为注释。  
2. 函数必须有注释标识该函数的用途、入参变量、函数的返回值类型,且必须简单在一行内写完。  
3. 函数的注释 `#` 顶格写, 井号后面紧跟一个空格 ,对于该格式的要求是为了最后生成函数的帮助文档是用的(markdown语法),然后是注释的内容,注释尽量简短且在一行,最后跟的是函数的类型。  
4. 函数内注释 `#` 与缩进格式对整齐。  
5. 变量的注释紧跟在变量的后面  

```sh
# 主函数 []<-()                  <-------函数注释这样写
function main(){
  local var="Hello World!!!"
  echo ${var}
}
# info级别的日志 []<-(msg:String)  <-------带入参的函数注释
log_info(){
  echo "[$(date +'%Y-%m-%dT%H:%M:%S%z')][$$]: [info] $*" >&2
}
# error级别的日志 []<-(msg:String) <-------带入参的函数注释
log_error(){
  # todo [error]用红色显示         <------函数内注释
  local msg=$1 # 将要输出的日志内容  <------变量的注释紧跟在变量的后面
  if [[ x"${msg}" != x"" ]];then
    # 注释                        <-------函数内注释 `#` 与缩进格式对整齐
    echo "[$(date +'%Y-%m-%dT%H:%M:%S%z')][$$]:[error] $*" >&2
  fi
}
```

## 缩进  

1. 使用两个空格进行缩进,不适用tab缩进  
2. 不在一行的时候使用 `\ `进行换行,使用 `\` 换行的原则是整齐美观  

```sh
#!/usr/bin/env bash
# 脚本使用帮助文档 []<-()
manual(){
  cat "$0"|grep -v "less \"\$0\"" \
          |grep -B1 "function "   \
          |grep -v "\\--"         \
          |sed "s/function //g"   \
          |sed "s/(){//g"         \
          |sed "s/#//g"           \
          |sed 'N;s/\n/ /'        \
          |column -t              \
          |awk '{print $1,$3,$2}' \
          |column -t
}
function search_user_info(){
  local result=$(httpclient_get --cookie "${cookie}" \
                                         "${url}/userName=${user_name}")
}
```

## 变量  

1. 变量赋值使用 `=` 等号,左右不能留有空格  
2. 使用变量的值用 `$` 取值符号  
3. 使用变量的时候,变量名一定要用 `{}` 包裹  
4. 使用变量的时候一定要用 双引号 `"${}"` 包裹  

```sh
var1="Hello World"   #正确,推荐使用双引号
var2='Hello World'   #正确,不推荐使用单引号
var3="${var1}"       #应用前面定义的变量的时候也要使用双引号包裹
var4=6            
var5=6.70            #小数
var3=${var1}         #正确,不推荐
```

5. 常量一定要定义成`readonly`,这种变量不能使用`source`跨shell使用  
6. 函数中的变量要用`local`修饰,定义成局部变量,这样在外部遇到重名的变量也不会影响  
7. 变量一经定义,不允许删除  

## 变量类型  

1. 定义在函数中的我们称之为函数局部变量;定义在函数外部，shell脚本中变量我们称之为脚本全局变量  
2. 环境变量 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。  

## 函数  

函数定义形式：  
```sh
function main(){
  #函数执行的操作
  #函数的返回结果
}
```
或  
```sh
main(){
  #函数执行的操作
  #函数的返回结果
}
```

1. 使用关键字 `function` 显示定义的函数为 `public` 的函数,可以供 外部脚本以 `sh 脚本 函数 函数入参` 的形式调用  
2. 未使用关键字 `function` 显示定义的函数为 `privat` 的函数, 仅供本脚本内部调用,注意这种`privat`是人为规定的,并不是shell的语法,不推荐以 `sh 脚本 函数 函数入参` 的形式调用,注意是不推荐而不是不能。  
3. 在函数内部首先使用有意义的变量名接受参数,然后在使用这些变量进行操作,禁止直接操作`$1`,`$2` 等，除非这些变量只用一次  
4. 函数的注释  

函数类型的概念是从函数编程语言中的概念偷过来的,shell函数的函数类型指的是函数的输入到函数的输入的映射关系  
```sh
# 主函数 []<-()                  <-------函数注释这样写
function main(){
  local var="Hello World!!!"
  echo ${var}
}
# info级别的日志 []<-(msg:String)  <-------带入参的函数注释
log_info(){
  echo "[$(date +'%Y-%m-%dT%H:%M:%S%z')][$$]: [info] $*" >&2
}
```
`main`函数的函数类型是 `[]<-()` , `<-` 左侧表的是函数的返回值类型 用`[]`包裹, 右侧是函数的参数类型 用`()`包裹,多个参数用 `,` 分隔,参数的描述是从 Scala 语言中偷过来, 先是参数名称 然后是参数类型 中间用`:`分隔  

对于`main`函数的注释来说, `#` 顶格写,后面紧跟一个空格,其实这样写是遵循的markdown的语法, 后面再跟一个空格,然后是 `[]<-()`,代表这个函数没有入参也没有返回值,这个函数的目的就是执行这个这个函数中的命令,但我不关心这个函数的返回值。也就是利用函数的副作用来完成我们想要的操作  

对于`log_info` 也是一样 不过 最后的函数类型是 `[]<-(msg:String)` 代表入参是一个`string`类型的信息,然后也没有返回值。关于函数的返回值,我理解的函数的返回值有两种形式,一种是显示的`return`一种是隐式的`echo`  

## 返回值  

### 显式return  

1. 明确返回结果是在`[0-255]`之间的数值类型的时候使用显示 `reuturn` 返回结果  
2. 返回结果类型是`Boolean`类型,也就是说函数的功能是起判断作用,返回结果是真或者假的时候使用显示 `return` 返回结果  

### 隐式echo  

其他形式的输出使用`echo`  

## 循环分支  

### if  

```sh
if [[ condition ]]; then
  # statements
fi

if [[ condition ]]; then
  # statements
else
  # statements
fi

if [[ condition ]]; then
  # statements
elif [[ condition ]]; then
  # statements
else
  # statements
fi
```

+ `if` 后面的判断 使用 双中括号`[[]]`  
+ `if [[ condition ]]; then` 写在一行  

### while  

```sh
while [[ condition ]]; do
  # statements
done

while read -r item ;do
  # statements
done < 'file_name'
```

### until  

```sh
until [[ condition ]]; do
  # statements
done
```

### for  

```sh
for (( i = 0; i < 10; i++ )); do
  # statements
done

for item in ${array}; do
  # statements
done
```

### case  

```sh
case word in
  pattern )
    #statements
    ;;
    *)
    #statements
    ;;
esac
```

-----------------------------------
### Reference  
[Shell编程规范--简书](https://www.jianshu.com/p/161618366866)

