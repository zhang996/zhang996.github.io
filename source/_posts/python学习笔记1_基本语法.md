---
title: python学习笔记1_基本语法
date: 2020-10-12 12:19:42
tags:
---

#### 编码：

默认情况下，Python 3 源码文件以 **UTF-8** 编码，所有字符串都是 unicode 字符串。

```
# -*- coding: cp-1252 -*-
```

定义允许在源文件中使用 Windows-1252 字符集中的字符编码，对应适合语言为保加利亚语、白罗斯语、马其顿语、俄语、塞尔维亚语。

#### python保留字

Python 的标准库提供了一个 keyword 模块，可以输出当前版本的所有关键字：

```
>>> import keyword
>>> keyword.kwlist
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

#### 注释

Python中单行注释以 **#** 开头，实例如下：

```
# 第一个注释
```

多行注释可以用多个 **#** 号，还有 **'''** 和 **"""**：

```
#!/usr/bin/python3  
# 第一个注释 
# 第二个注释  
''' 
第三注释 
第四注释 
'''  
""" 
第五注释 
第六注释 
""" 
print ("Hello, Python!")
```

#### 行与缩进

python最具特色的就是使用缩进来表示代码块，不需要使用大括号 **{}** 。

缩进的空格数是可变的，但是同一个代码块的语句必须包含相同的缩进空格数。

#### 多行语句

Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠(\)来实现多行语句

在 [], {}, 或 () 中的多行语句，不需要使用反斜杠(\)

#### 数字(Number)类型

python中数字有四种类型：整数、布尔型、浮点数和复数。

- **int** (整数), 如 1, 只有一种整数类型 int，表示为长整型，没有 python2 中的 Long。
- **bool** (布尔), 如 True。
- **float** (浮点数), 如 1.23、3E-2
- **complex** (复数), 如 1 + 2j、 1.1 + 2.2j

#### 字符串(String)

- python中单引号和双引号使用完全相同。
- 使用三引号('''或""")可以指定一个多行字符串。
- 转义符 '\'
- 反斜杠可以用来转义，使用r可以让反斜杠不发生转义。。 如 r"this is a line with \n" 则\n会显示，并不是换行。
- 按字面意义级联字符串，如"this " "is " "string"会被自动转换为this is string。
- 字符串可以用 + 运算符连接在一起，用 * 运算符重复。
- Python 中的字符串有两种索引方式，从左往右以 0 开始，从右往左以 -1 开始。
- Python中的字符串不能改变。
- Python 没有单独的字符类型，一个字符就是长度为 1 的字符串。
- 字符串的截取的语法格式如下：**变量[头下标:尾下标:步长]**

#### 等待用户输入

执行下面的程序在按回车键后就会等待用户输入：

```
input("\n\n按下 enter 键后退出。")
```

以上代码中 ，"\n\n"在结果输出前会输出两个新的空行

#### 同一行显示多条语句

语句之间使用分号(;)分割

#### 多个语句构成代码组

缩进相同的一组语句构成一个代码块，我们称之代码组。

像if、while、def和class这样的复合语句，首行以关键字开始，以冒号( : )结束，该行之后的一行或多行代码构成代码组。

将首行及后面的代码组称为一个子句(clause)。

```
if expression : 
   suite
elif expression : 
   suite 
else : 
   suite
```

#### Print 输出

print 默认输出是换行的，如果要实现不换行需要在变量末尾加上 **end=""**

```
#!/usr/bin/python3  
x="a" 
y="b" 
# 换行输出 
print( x ) 
print( y )  
print('---------') 
# 不换行输出 
print( x, end=" " ) 
print( y, end=" " ) 
print()
```

执行结果为

```
a
b
---------
a b
```

#### import 与 from...import

在 python 用 **import** 或者 **from...import** 来导入相应的模块。

将整个模块(somemodule)导入，格式为： **import somemodule**

从某个模块中导入某个函数,格式为： **from somemodule import somefunction**

从某个模块中导入多个函数,格式为： **from somemodule import firstfunc, secondfunc, thirdfunc**

将某个模块中的全部函数导入，格式为： **from somemodule import \***

```
from sys import argv,path  #  导入特定的成员  
print('================python from import===================================') 
print('path:',path) # 因为已经导入path成员，所以此处引用时不需要加sys.path
```

```
import sys 
print('================Python import mode==========================') 
print ('命令行参数为:') 
for i in sys.argv:
	print (i) 
print ('\n python 路径为',sys.path)
```



参考《[Python3 基础语法](https://www.runoob.com/python3/python3-basic-syntax.html)》