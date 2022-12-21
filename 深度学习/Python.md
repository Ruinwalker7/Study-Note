# Python

### 基础

```python
print('12','12312')
name = input('请输入名字：') #输入元素
```

**采用缩进来确定代码块**，约定是四个空格为缩进，**不要混合使用tab和空格**

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-//通常在代码前面加上这两行
```

#### 集合

```python
classmate = ['Tim','Nick','Alex']
s = ['python', 'java', ['asp', 'php'], 'scheme']#多维数组
classmates = ('Michael', 'Bob', 'Tracy')#tuple,无法改变的数组
s = (1,)#消除歧义，是一个tuple
#tuple里面可以内嵌list，list的内容是可以改变的
```

##### `dit`

```python
#类似于map，key和value对应
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
#检查是否存在
'Bob' in d
d.get('Thomas')
d.get('Thomas', -1)  #如果不存在可以返回none，或者自己指定的value
d.pop('Bob') #删除对应的value
```



#### 条件判断

布尔值 `and`,`or`,`not`运算，`:`区分代码块

```python

if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4>
```



#### 循环

```python
for i in range(5)#i为0-4
```





#### 函数

- 定义函数要用`def`

- 导入函数 `from abstract import my_abs`

- 数据类型检查：

```python
if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
```

- 返回多个值

```python
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny

x, y = move(100, 100, 60, math.pi / 6)
```

返回的值是`tuple`，但是可以用多个变量按位置接受

- 设置默认参数：

与c++类似，但是要把默认的值放后面，需要赋值的值放前面，不然python会给出编译错误

- **默认参数要牢记一点：默认参数必须指向不变对象**

为什么要设计`str`、`None`这样的不变对象呢？因为不变对象一旦创建，对象内部的数据就不能修改，这样就减少了由于修改数据导致的错误。此外，由于对象不变，多任务环境下同时读取对象不需要加锁，同时读一点问题都没有。我们在编写程序时，如果可以设计一个不变对象，那就尽量设计成不变对象。

- 把函数参数设计成可变参数

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

- 关键字参数

而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个`dict`：

```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
    
person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}

>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了：

```python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
```

命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错



**在Python中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用。但是请注意，参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。**



- 递归

解决递归调用栈溢出的方法是通过**尾递归**优化，事实上尾递归和循环的效果是一样的

尾递归是指，**在函数返回的时候，调用自身本身，并且，return语句不能包含表达式。**这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。



### 高级特性

#### 切片

```python
L[0:3] #取前三个元素
L[:3] 	#如果是0可以省略
L[-2:] #倒数两个 注意倒过来是第一个元素是-1
```



#### 迭代

```python
for ch in 'abc'
#判断方法
>>> from collections.abc import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```



#### 列表生成式

```python
[x*x for x in range(1,4)] #[1,4,9,16]
#筛选偶数的平方
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
```



#### 生成器

使用next算出下一个值

编写函数时使用yield返回，调用next时会从yield开始



### 函数式编程

#### 高级函数

##### map

```python
def f(x):
	return x*x

>>>list[map(f,[1,2,3,4,5])]
[1,4,9,16,25]
```

map传入两个值，第一个是函数，一个是`Iterable`，最终返回一个`Iterator`



##### reduce

```python
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> reduce(fn, [1, 3, 5, 7, 9])
13579
```

操作两个数，返回值与下一个数继续操作



##### filter

筛选函数，可以对任何序列进行操作

可以理解为将一个筛选方法加入到对象中，如果是一个`Iterator`则调用next寻找下一个



##### sorted

排序函数，可以加入一个方法决定排序的规则



#### 返回函数

```python
>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function lazy_sum.<locals>.sum at 0x101c6ed90>
>>> f()
25
```



### 问题与解决

1.vscode无法使用python

ubuntu默认使用的不是python3版本，需要建立链接

```shell
sudo ln -s /usr/bin/python3.8 /usr/bin/python
```

