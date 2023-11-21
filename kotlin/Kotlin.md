# Kotlin

## 基本类型

在 Kotlin 中，所有东西都是对象，在这个意义上讲可以在任何变量上调用成员函数与属性。



### 数字

整数类型：`Int`,`Long`,`Byte`,`Short`

浮点数：`Float` 与 `Double` 类型

**Kotlin 中的数字没有隐式拓宽转换**



数值常量字面值有以下几种:

- 十进制: 123
  - Long 类型用大写 `L` 标记: `123L`
- 十六进制: `0x0F`
- 二进制: `0b00001011`



对于小数，Kotlin 默认是 `Double` 类型



#### JVM 平台的数字表示

在 JVM 平台数字存储为原生类型 `int`、 `double` 等。 

当创建可空数字引用如 `Int?` 或者使用泛型时。 在这些场景中，数字会装箱为 Java 类 `Integer`、 `Double` 等。

两个可空数字引用可能会引用不同的对象，也可能会引用相同的对象





#### 显示转换

**较小的类型*不能 隐式转换*为较大的类型**

所有数字类型都支持转换为其他类型：

- `toByte(): Byte`
- `toShort(): Short`
- `toInt(): Int`
- `toLong(): Long`
- `toFloat(): Float`
- `toDouble(): Double`



#### 整数除法

整数间的除法总是返回整数。会丢弃任何小数部分，除非显示将一个数字转换为浮点型



#### 浮点数比较

对于静态类型不是浮点数的情况，认为：

- 认为 `NaN` 与其自身相等
- 认为 `NaN` 比包括正无穷大（`POSITIVE_INFINITY`）在内的任何其他元素都大
- 认为 `-0.0` 小于 `0.0`



### 字符串

访问：`s[1]`

遍历：

```kotlin
for (c in str) {
    println(c)
}
```

字符串是不可变的，一旦初始化了一个字符串，就不能改变或者重新赋值，所有转化操作都是以返回一个新的字符串对象为结果



#### 多行字符串

*多行字符串*可以包含换行以及任意文本。 它使用三个引号（`"""`）分界符括起来，内部没有转义并且可以包含换行以及任何其他字符：

```kotlin
val text = """
    for (c in "foo")
        print(c)
"""
```

如需删掉多行字符串中的前导空格，请使用 [`trimMargin()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/trim-margin.html) 函数：

```kotlin
val text = """
|Tell me and I forget.
|Teach me and I remember.
|Involve me and I learn.
|(Benjamin Franklin)
    """.trimMargin()
```

可以传入参数作为边界前缀





#### 字符串模版

```kotlin
 println("$s.length is ${s.length}") // 输出 "abc.length is 3"
```

使用`$`或者`${}`作为模版



### 数组

Kotlin 中最常见的数组类型是对象类型数组，由 [`Array`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-array/) 类表示。



#### 创建数组

创建数组的方法：

- 创建数组的函数：`arrayOf()`, `arrayOfNulls()`, `emptyArray()`
- `Array` 构造函数

```kotlin
val simpleArray = arrayOf(1, 2, 3)

val nullArray: Array<Int?> = arrayOfNulls(3)
 
var exampleArray = emptyArray<String>()
```



#### 嵌套数组

```kotlin
    // Creates a two-dimensional array
    val twoDArray = Array(2) { Array<Int>(2) { 0 } }
    println(twoDArray.contentDeepToString())
    // [[0, 0], [0, 0]]
```





