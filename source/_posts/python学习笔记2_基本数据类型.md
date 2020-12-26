---
title: python学习笔记2_数据类型
date: 2020-10-15 12:19:42
tags:
---

#### 标准数据类型

Python3 中有六个标准的数据类型：

- Number（数字）
- String（字符串）
- List（列表）
- Tuple（元组）
- Set（集合）
- Dictionary（字典）

Python3 的六个标准数据类型中：

- **不可变数据（3 个）：**Number（数字）、String（字符串）、Tuple（元组）；
- **可变数据（3 个）：**List（列表）、Dictionary（字典）、Set（集合）。

```
>>> a = 111
>>> isinstance(a, int)
True
```

**isinstance 和 type 的区别在于：**

- type()不会认为子类是一种父类类型。
- isinstance()会认为子类是一种父类类型。

#### 数值类型实例

| int    | float      | complex    |
| :----- | :--------- | :--------- |
| 10     | 0.0        | 3.14j      |
| 100    | 15.20      | 45.j       |
| -786   | -21.9      | 9.322e-36j |
| 080    | 32.3e+18   | .876j      |
| -0490  | -90.       | -.6545+0J  |
| -0x260 | -32.54e100 | 3e+26J     |
| 0x69   | 70.2E-12   | 4.53e-7j   |

Python还支持复数，复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都是浮点型

- 1、Python可以同时为多个变量赋值，如a, b = 1, 2。
- 2、一个变量可以通过赋值指向不同类型的对象。
- 3、数值的除法包含两个运算符：**/** 返回一个浮点数，**//** 返回一个整数。
- 4、在混合计算时，Python会把整型转换成为浮点数。

**与 C 字符串不同的是，Python 字符串不能被改变。向一个索引位置赋值，比如word[0] = 'm'会导致错误。**

|                             函数                             |                        描述                         |
| :----------------------------------------------------------: | :-------------------------------------------------: |
| [int(x [,base\])](https://www.runoob.com/python3/python-func-int.html) |                  将x转换为一个整数                  |
| [float(x)](https://www.runoob.com/python3/python-func-float.html) |                 将x转换到一个浮点数                 |
| [complex(real [,imag\])](https://www.runoob.com/python3/python-func-complex.html) |                    创建一个复数                     |
| [str(x)](https://www.runoob.com/python3/python-func-str.html) |                将对象 x 转换为字符串                |
| [repr(x)](https://www.runoob.com/python3/python-func-repr.html) |             将对象 x 转换为表达式字符串             |
| [eval(str)](https://www.runoob.com/python3/python-func-eval.html) | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
| [tuple(s)](https://www.runoob.com/python3/python3-func-tuple.html) |               将序列 s 转换为一个元组               |
| [list(s)](https://www.runoob.com/python3/python3-att-list-list.html) |               将序列 s 转换为一个列表               |
| [set(s)](https://www.runoob.com/python3/python-func-set.html) |                   转换为可变集合                    |
| [dict(d)](https://www.runoob.com/python3/python-func-dict.html) |  创建一个字典。d 必须是一个 (key, value)元组序列。  |
| [frozenset(s)](https://www.runoob.com/python3/python-func-frozenset.html) |                  转换为不可变集合                   |
| [chr(x)](https://www.runoob.com/python3/python-func-chr.html) |              将一个整数转换为一个字符               |
| [ord(x)](https://www.runoob.com/python3/python-func-ord.html) |             将一个字符转换为它的整数值              |
| [hex(x)](https://www.runoob.com/python3/python-func-hex.html) |         将一个整数转换为一个十六进制字符串          |
| [oct(x)](https://www.runoob.com/python3/python-func-oct.html) |          将一个整数转换为一个八进制字符串           |