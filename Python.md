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



#### 

### 问题与解决

1.vscode无法使用python

ubuntu默认使用的不是python3版本，需要建立链接

```shell
sudo ln -s /usr/bin/python3.8 /usr/bin/python
```

