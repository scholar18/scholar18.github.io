---
title: Python第二章笔记
author: 曹毅
# img: /source/images/python.jpg
---
# 变量和简单数据类型

## 变量的命名

变量名只能包含数字、字母、下划线，且不能以**数字做开头**.

变量名不能包含**空格**，且不能使用Python关键字

变量名应具体，使人一眼就明白该变量的含义

变量名不应使用**0，1** .易与**o，l**搞混

## 字符串

字符串即一系列字符，在Python中用**双撇号""或单撇号' '括**起来的都是字符串

- 在使用字符串是应注意
- 外围使用""时，内部不应再次使用"",易发生混乱。
- 外围使用''时，内部也不应使用''.

下面是一些有关字符串的函数

以下字符串同一用 name 表示
- name.title() #使每个单词的首字母大写
- name.upper() #将所有字母改为大写字母
- name.lower() #将所有字母改为小写字母

- name = f"{name1} {name}"
将class1与class2连接起来，每个字符串用{}括起来，在每个{}之间或者说在""内的个个地方都可以插入一些字符串。
(f字符串是在Python3.6版本引入，在3.5及更早版本无法使用，可以
- 使用format())
例：full_name = "{} {}".format(first_name,last_name)
    用法与f字符串类似，这里不做过多解释

- name.rstrip() #可暂时删除字符串末尾的空白若要永久删除，应将删除操作的结果关联到变量：
- name = name.rstrip() #就可以永久删除空白
- name.lstrip() #可暂时删除字符串开头的空白
- name.strip()  #可暂时删除字符串两侧的空白


## 整数与浮点数
Python中的整数包括正整数，0，负整数。
奇妙的是，Python中整数的值是可以无限大的，当取值超过计算机自身的计算能力时，Python 会自动转用高精度计算（大数计算）。这使得在计算高精度时，Python有着天然的优势。


当数值过大时，我们可以在其中添加**下划线**来使数更加易读，例：14_000_000_000与14000000000没有任何区别。


关于运算

- +，-，*与其他语言类似，不做阐述
- **代表乘方运算
- /代表除法，任意两个数相除，结果一定是浮点数.
- //也是除法，但结果是整数，且为向下取整。
- % 为取余运算

记：python支持同时给多个变量同时赋值,但要保证前后个数相等

### x,y,z = 1,2,3 ###

## Python之禅

The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!


下面为中文翻译：
Python之禅 by Tim Peters
 
优美胜于丑陋（Python 以编写优美的代码为目标）
    明了胜于晦涩（优美的代码应当是明了的，命名规范，风格相似）
    简洁胜于复杂（优美的代码应当是简洁的，不要有复杂的内部实现）
    复杂胜于凌乱（如果复杂不可避免，那代码间也不能有难懂的关系，要保持接口简洁）
    扁平胜于嵌套（优美的代码应当是扁平的，不能有太多的嵌套）
    间隔胜于紧凑（优美的代码有适当的间隔，不要奢望一行代码解决问题）
    可读性很重要（优美的代码是可读的）
    即便假借特例的实用性之名，也不可违背这些规则（这些规则至高无上）
    
不要包容所有错误，除非你确定需要这样做（精准地捕获异常，不写 except:pass 风格的代码）
    
当存在多种可能，不要尝试去猜测
而是尽量找一种，最好是唯一一种明显的解决方案（如果不确定，就用穷举法）
虽然这并不容易，因为你不是 Python 之父（这里的 Dutch 是指 Guido ）
    
做也许好过不做，但不假思索就动手还不如不做（动手之前要细思量）
    
如果你无法向人描述你的方案，那肯定不是一个好方案；反之亦然（方案测评标准）
    
命名空间是一种绝妙的理念，我们应当多加利用（倡导与号召）