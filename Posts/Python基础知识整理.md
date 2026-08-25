---
layout: post
title: Python 基础知识整理
description: 从数据类型到函数参数，梳理 Python 学习中最常用的核心概念。
category: Python
permalink: /python-basics/
---

## 数据类型

### 整数

Python表达整数的方式和其他语言基本一样，十六进制用 `0x` 作为前缀；对于很大的整数，Python允许使用 `_` 分隔，例如`10_000_000_000`。

### 浮点数

Python中可以使用科学计数法表示，把10用e替代，1.23x10⁹就是`1.23e9`。

### Bytes

Python使用b前缀加上引号来表示bytes类型。x = b'ABC'，每个字符占1字节。

### String

字符串是以`'`或`"`括起来的文本，如果字符串内部既包含`'`又包含`"`，可以使用转义字符`\`来标识。
转义字符可以转义很多字符，`\n`表示换行，`\t`表示制表符，还可以把它本身转义输出。
Python还规定使用`r''`的字符串默认不转义。

```Python
print('I'm \"OK\"!')
print('I \'m Bangor.')
print('I \'m learning \nPython.\\')
print('\\\t\\')
print(r'\\\t\\')
```

### 编码

对于单个字符的编码，Python提供了`ord()`函数来获取字符的整数表示，而`chr()`函数则把其转换成对应字符。`encode()`函数可以把字符串转换成指定编码的bytes类型。`len()`可以计算字符串的字符数，如果是bytes类型，则是计算字节数。

```Python
print(ord('A')) # 结果是65
print(chr(65)) # 结果是A
print('ABC'.encode('ascii')) # 结果是b'ABC'
print('中文'.encode('utf-8')) # 结果是b'\xe4\xb8\xad\xe6\x96\x87'
print(len('ABCDE')) # 结果是5
print(len('中文')) # 结果是2
print(len('中文'.encode('utf-8'))) # 结果是6
```

### 格式化

- Python中可以使用`%`来代表占位符并格式化字符串，如果只有一个占位符则可以省略。

    |占位符|替换内容|
    |---|---|
    |%s|字符串|
    |%d|整数|
    |%f|浮点数|
    |%x|十六进制整数|

    其中，格式化整数和浮点数还可以指定是否补0和整数与小数的位数：

    ```Python
    print('Hello, %s' % 'world')
    print('Hi, %s, you have $%d.' % ('Michael', 1000000))
    print('%2d-%02d' % (3, 1))
    print('%.2f' % 3.1415926)
    ```

    如果不知道要用什么占位符，`%s`永远生效。

- 第二种格式化字符串的方式是使用字符串的`format()`函数，它会把传入的参数依次替换字符串内的占位符`{0}` `{1}`等等。

    ```Python
    print('Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', (85-72)/72))
    ```

- 第三种格式化字符串的方法是在字符串开头添加`f`，称之为`f-string`。它可以替换掉参数。

    ```Python
    r = 2.5
    s = 3.14 * r ** 2
    print(f'半径是{r}的圆面积为{s:.2f}')
    ```

### 空值

Python里空值使用`None`表示。

### 无限大

无限大可以使用`math.inf`表示。也可以在前面加一个负号表示负无穷`-math.inf`。

### 除法计算

Python中`/`表示正常除，无论有没有整除，结果都是浮点数。
而使用`//`进行除法计算，结果是整数，小数部分会被舍弃。
还有一种是进行取余的除法计算，使用`%`来进行计算。

```Python
print(10/3) # 结果是3.3333333333333335
print(9/3) # 结果是3.0
print(10//3) # 结果是3
print(10%3) # 结果是1
```

### 列表 list

list是一种有序的集合，可以随时添加和删除其中的元素，用`[]`表示。元素可以是不同的数据类型，包括list。
python可以使用负数来获取元素，-1则是最后一个元素。
`append()`函数表示在list末尾添加元素，`insert()`函数表示在list指定位置插入元素，`pop()`函数表示删除list指定位置的元素，若为空则删除最后一个。

```Python
l = ['A', 'B', 'C']
print(l[-2])
l.append('D')
l.insert(1, 'E')
l.pop()
```

### 元组 tuple

tuple和list一样是有序的集合，用`()`表示，但不同是tuple一旦初始化就不能更改，也就没有`append()` `insert()`这些方法。
如果定义一个元素的tuple，需要加一个逗号来消除歧义：

```Python
t = (1,)
```

tuple还可以添加list作为元素，而list可修改，这便是‘可变的’tuple

```Python
t = ('a', 'b', ['A', 'B'])
```

### 条件判断及模式匹配

```Python
score = 'B'
if score == 'A':
    print('score is A.')
elif score == 'B':
    print('score is B.')
elif score == 'C':
    print('score is C.')
else:
    print('invalid score.')

match score:
    case 'A':
        print('score is A.')
    case 'B':
        print('score is B.')
    case 'C':
        print('score is C.')
    case _: # _表示匹配到其他任何情况
        print('score is ???.')

args = ['gcc', 'hello.c', 'world.c']
# args = ['clean']
# args = ['gcc']
match args: # match还可以匹配列表
    # 如果仅出现gcc，报错:
    case ['gcc']:
        print('gcc: missing source file(s).')
    # 出现gcc，且至少指定了一个文件:
    case ['gcc', file1, *files]:
        print('gcc compile: ' + file1 + ', ' + ', '.join(files))
    # 仅出现clean:
    case ['clean']:
        print('clean')
    case _:
        print('invalid command.')
```

Python提供`range()`函数来生成一个整数序列，`list()`函数把序列转换成集合

```Python
print(list(range(5)))
```

### 字典 dict和set

Python的字典使用`{}`表示，dict的key必须是不可变对象

```Python
d = {'A': 1, 'B': 2, 'C': 3}
```

要避免key不存在的错误，一是通过`in`判断，二是通过dict的`get()`方法

```Python
print('D' in d)
print(d.get('D')) # 不存在则返回None
print(d.get('D'), -1) # 不存在则返回-1
d.pop('A') # 通过pop()方法删除dict的数据
```

Python还有一种集合 set，它是一组key的集合，不存储value，由于key不能重复，所以set没有重复数据

```Python
print(s) # 重复数据会被过滤
s.add(4)
s.remove(4)
```

## 函数

### 函数参数

在定义函数的默认参数时，默认参数必须指向不变对象

```Python
# 如果把L定义为[]，重复执行则会出现多个'END'
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L
```

定义函数的可变参数

```Python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

定义函数的关键字参数，这些关键字参数在函数内部自动组装为一个tuple

```Python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)

extra = {'city': 'Beijing', 'job': 'Engineer'}
# **extra表示把extra的所有key-value用关键字参数传入函数
person('Jack', 24, **extra)
```

如果要限制关键字参数的名字，就可以用命名关键字参数，*后面的参数被视为命名关键字参数

```Python
def person(name, age, *, city, job):
    print(name, age, city, job)
```

如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了

```Python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
```

命名关键字参数可以有默认值，调用时可以不传入city参数

```Python
def person(name, age, *, city='Beijing', job):
    print(name, age, city, job)
```

如果五种参数都要使用，顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。

```Python
def f1(a, b=0, *c,  d, **e):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'e =', e)
```
