---
title: Shell脚本
date: 2021-11-29 00:07:00
author: scholar
# img: ../images/哈希.jpg
hide: false
summary: 大数据
---
# Shell脚本学习笔记
## 脚本入门
    ps:本文出现脚本均在hadoop中运行
### 脚本格式
- 脚本以$#!/bin/bash$ 开头, 表示选用**bash**作为脚本的解释器。
### 第一个shell脚本
#### 代码
创建一个helloworld.sh文件
内容为：
```shell
#!/bin/bash
echo "HelloWorld Cao sang"
```
运行helloworld.sh脚本

    sh helloworld.sh
    或
    bash helloworld.sh
    或
    bash 绝对路径/helloworld.sh
    或
    sh 绝对路径/helloworld.sh
若直接调用 helloworld.sh则权限不够，主要原因是我们用bash,sh解析器运行脚本时，是**解析器去执行helloworld.sh文件**，所以脚本自身**不需要执行权限**。

若是直接输出helloworld.sh文件，就是helloworld.sh这个文件去执行它本身，**自己调用自己需要执行权限**。

我们可以执行 $\color{red}{chmod 777 helloworld.sh}$来给予脚本权限

然后就可以用**脚本文件自己来执行自己**

相对路径
    
    ./helloworld.sh
绝对路径

    绝对路径/helloworld.sh
### 第二个shell脚本
多命令处理

1.需求

在指定目录里建立.txt文件，并在.txt文件里输入"I love cls"

2.实例实操
```shell
touch batch.sh
vim batch.sh
#!/bin/bash
cd /home/atguigu
touch cls.txt
echo "I love cls" >> cls.txt
```
## 脚本中的变量
### 系统变量
#### 1.常用系统变量
$HOME, $PWD, $SHELL, $USER等
#### 2.案例实操
##### (1) 查看系统变量的值
    [atguigu@hadoop101 datas]$ echo $HOME
    /home/atguigu
##### (2) 显示当前shell中所有变量 ： set
    
    [atguigu@hadoop101 datas]$ set
    BASH=/bin/bash
    BASH_ALIASES=()
    BASH_ARGC=()
    BASH_ARGV=()

### 自定义变量
#### 1.基本语法
(1)定义变量：
变量=值

(2)撤销变量:unset变量

(3)声明静态变量:readonly变量,注意：不能unset

#### 2.变量定义规则
(1) 有字母，数字，下划线构成，不能以数字开头,另外：$\color{red}{环境变量名建议大写}$

(2) $\color{red}{等号两侧不能有空格}$

(3) 在bash中,变量默认类型都是**字符串类型**,无法直接进行数值运算

(4) 变量值如果有空格,**需要用双引号或单引号括起来**

#### 3.案例实操
(1)定义变量A

    [atguigu@hadoop101 datas]$ A=5
    [atguigu@hadoop101 datas]$ echo $A
    5
(2)给变量A重新赋值
    
    [atguigu@hadoop101 datas]$ A=8
    [atguigu@hadoop101 datas]$ echo $A
    8
(3)撤销变量A

    [atguigu@hadoop101 datas]$ unset A
    [atguigu@hadoop101 datas]$ echo $A
(4)声明静态变量B=2,不能unset

    [atguigu@hadoop101 datas]$ readonly B=2
    [atguigu@hadoop101 datas]$ echo $B
    2
    [atguigu@hadoop101 datas]$ B=9
    -bash:B:readonly variable
(5)在bash中,变量类型均为字符串,无法直接进行数值运算

    [atguigu@hadoop101 datas]$ C=1+1
    [atguigu@hadoop101 datas]$ echo $C
    1+1
(6)变量的值如果用空格,应用双引号或单引号括起来

    [atguigu@hadoop101 datas]$ D=I love xiaohua
    -bash: world:command not found
    [atguigu@hadoop101 datas]$ D="I love xiaohua"
    [atguigu@hadoop101 datas]$ echo $D
(7)可把变量提升为全局变量，供其他Shell程序使用
**export** 变量名

    [atguigu@hadoop101 datas]$ vim helloworld.sh
    #!/bin/bash
    echo "helloworld"
    echo $B
    [atguigu@hadoop101 datas]$ ./helloworld.sh
    helloworld
发现没有打印变量B的值

    [atguigu@hadoop101 datas]$ export B
    [atguigu@hadoop101 datas]$ bash helloworld.sh
    helloworld
    2
### 特殊变量：$n
#### 1.基本语法
$n (n为数字, $ 0代表该脚本的名字,$ 1~ $ 9代表输入的第一到第九个参数,十以上的参数需要用大括号包含,如${10})

#### 2.实例操作

    [atguigu@hadoop101 datas]$ touch parameter.sh 
    [atguigu@hadoop101 datas]$ vim parameter.sh

    #!/bin/bash
    echo "$0  $1   $2"

    [atguigu@hadoop101 datas]$ chmod 777 parameter.sh

    [atguigu@hadoop101 datas]$ ./parameter.sh cls  xz
    ./parameter.sh  cls   xz
### 特殊变量：$#
#### 1.基本语法
$# : 获取输入参数的个数，**常用于循环**,**不包含脚本名**

#### 2.实例操作

    [atguigu@hadoop101 datas]$ vim parameter.sh

    #!/bin/bash
    echo "$0  $1   $2"
    echo $#

    [atguigu@hadoop101 datas]$ chmod 777 parameter.sh

    [atguigu@hadoop101 datas]$ ./parameter.sh cls  xz
    parameter.sh cls xz 
    2
### 特殊变量 ：$ *, $ @
#### 1.基本语法
$* 代表命令行中的所有参数，把所有参数当成一个整体

$# 代表命令行中的所有参数，但把每个参数区别对待

基本$* **代表把参数凑成一个字符串**

$# **把参数存储在一个数组里，我类比其他语言，感觉是这样**
#### 2.实例操作

    [atguigu@hadoop101 datas]$ vim parameter.sh

    #!/bin/bash
    echo "$0  $1   $2"
    echo $#
    echo $*
    echo $@

    [atguigu@hadoop101 datas]$ bash parameter.sh 1 2 3
    parameter.sh  1   2
    3
    1 2 3
    1 2 3
### 特殊变量:$?
#### 1.基本语法
$? 判断最后一次命令执行的状态,若为**0**，则表示命令正常执行；

如果返货一个**非0**的数(具体是什么数，由命令决定)，证明上一个命令执行不正确。

#### 2.实例操作

    [atguigu@hadoop101 datas]$ ./helloworld.sh
    helloworld
    [atguigu@hadoop101 datas]$ echo $?
    0
## 运算符
### 1.基本语法
(1) $((运算式)) 或 $[运算式]
(2) expr +,-,*,/,% 加减乘除取余

$\color{red}{注意exper运算符之间要有空格}$
### 2.实例操作
(1) 计算 1 + 2的值

    [atguigu@hadoop101 datas]$ exper 1 + 2
    3
(2)计算 3 - 2的值
    
    [atguigu@hadoop101 datas]$ exper 3 - 2
    1

(3) 计算$(2+3)X4$
    
exper一步完成运算

    exper `exper 2 + 3 `*4

采用$[运算符]解决

    [atguigu@hadoop101 datas]$ S=$[(2+3)*4] 
    [atguigu@hadoop101 datas]$ echo S
    20   
    