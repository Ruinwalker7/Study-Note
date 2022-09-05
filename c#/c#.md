# C#

### 基本语法

第一句都是：using 

输出

```c#
	Console.WriteLine("A:{0},a:{1}",65,96);//输出第一个元素和第二个元素
	///XML注释，会被编译
```

#### 按应用传递

```c#
public void swap(ref int x, ref int y){}   //ref修饰
```

#### 按输出传递参数

```c#
public void getValue(out int x );//可以直接传递x参数
```



#### C# 可空类型（Nullable）

C# 提供了一个特殊的数据类型，**nullable** 类型（可空类型），可空类型可以表示其基础值类型正常范围内的值，再加上一个 null 值。

```c#
Nullable<int> i = new Nullable<int>(3);
int i; //默认值0
int? ii; //默认值null ?等于是将一个不能为空的数据类型赋值为空
```

??是Null合并运算符

```c#
int num3 = num1 ?? 5.34;      // num1 如果为空值则返回 5.34
```



DllImport引入dll，Kernel32.dll是系统常用api