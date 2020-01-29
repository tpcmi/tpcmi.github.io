---
layout:     post
title:      “Python 专项查缺补漏"
date:       2019-12-19 22:00:00
author:     "Tp"
header-img: "img/starry_night.jpg"
tags:

- NowCoder
---

[toc]

## 闭包(Closure)

在内部函数中，对外部作用域变量引用，则内部函数被认为是闭包。（函数可以作为函数的返回值返回）

```python
def print_msg():
  msg = "Hello,world!"
  def inner_print():
    print(msg)
  return inner_print

message = print_msg()
# 输出“Hello，world！”
message()

```

- 闭包无法修改外部函数的局部变量

```python
def out_value():
  x = 0
  def inner_value():
    x = 1
    print('inner x: ',x)
  print('out_value x before call func:',x)
  inner_value()
  print('out_value x after call func:',x)

out_value()

# 输出：
# out_value x before call func: 0
# inner x:  1
# out_value x after call func: 0
```



- 循环中不包含域的概念

```python
l = []
for i in range(3):
	def func(x):
		return x*i
	l.append(func)
  
for f in l:
	print(f(2)，end=' ')

# 输出
# 4 4 4
```

想要实现输出0，2，4只需要修改`func`为闭包即可

```python
l = []
for i in range(3):
  def out_func(i):
    def func(x):
      return x*i
    return func
	l.append(out_func(i))
  
for f in l:
	print(f(2),end=' ')

# 输出
# 0 2 4
```

- 应用

用于保存当前运行环境

## 复数

1、虚数不能单独存在，它们总是和一个值为 0.0 的实数部分一起构成一个复数

2、复数由实数部分和虚数部分构成

3、表示虚数的语法：<u>**real+imagej**</u>

4、实数部分和虚数部分都是<u>**浮点数**</u>

5、虚数部分必须有后缀<u>**j或J**</u>

## 装饰器

装饰器的作用是为已存在的函数添加额外的功能

[后续有时间补充完善](https://www.zhihu.com/question/26930016/answer/99243411)

## Tuple

- 大小比较

```python
(2,3)<(3,-1)
# True
```

Tuple是从第一个开始比较，相同则比较下一位，且数量多的大

- Tuple的值不可以改变

## 无穷大
```python
float('inf')
```

