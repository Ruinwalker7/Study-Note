# Kotlin概念

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



#### 传入可变数量的实参

使用`*`传入可变参数的数组

使用`vararg`在函数申明中申明可变参数



#### 比较数组

判断两个数组是否具有相同的个元素and相同的排血，使用`.contentEquals()`or`.contentDeepEquals()`

请不要使用`==`or`!=`，因为这会比较两个数组是不是一个对象



#### 原生类型数组

如果使用`Array`来申明原始变量的数组，值会被装箱，这会显著影响机器的性能，所以我们可以使用原生类型数组

| Primitive-type array                                         | Equivalent in Java |
| ------------------------------------------------------------ | ------------------ |
| [`BooleanArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-boolean-array/) | `boolean[]`        |
| [`ByteArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-byte-array/) | `byte[]`           |
| [`CharArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-char-array/) | `char[]`           |
| [`DoubleArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-double-array/) | `double[]`         |
| [`FloatArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-float-array/) | `float[]`          |
| [`IntArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-int-array/) | `int[]`            |
| [`LongArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-long-array/) | `long[]`           |
| [`ShortArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-short-array/) | `short[]`          |

这些原生类型数组与`Array`没有继承关系，但是他们的方法一样



### 智能转换

Kotlin 的编译器足够聪明，使其可以自动转换类型。

```kotlin
fun demo(x: Any) {
    if (x is String) {
        print(x.length) // x 自动转换为字符串
    }
}
```

**当编译器能保证变量在检测及其使用之间不可改变时，智能转换才有效。**



## 控制流程

### 条件循环

`if`是一个表达式：它会返回一个值，如果使用`if`作为表达式，那么`else`是必须存在的



`when`类似于switch，同样的`when`也可以作为表达式，如果作为表达式的时候，`else` 是必须存在的，除非编译器认为已经覆盖了所有的情况

```kotlin
when (x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> {
        print("x is neither 1 nor 2")
    }
}
```



`if-not-null` 缩写`?.` 

```kotlin
println(files?.size) // 如果 files 不是 null，那么输出其大小（size）
```

执行代码

```kotlin
value?.let {
    …… // 如果非空会执行这个代码块
}
```



`if-not-null-else`

```kotlin
println(files?.size ?: "empty") 
```

`if-null`=`?:`





### 返回与跳转

Kotlin 有三种结构化跳转表达式：

- `return` 默认从最直接包围它的函数或者[匿名函数](https://book.kotlincn.net/text/lambdas.html#匿名函数)返回。
- `break` 终止最直接包围它的循环。
- `continue` 继续下一次最直接包围它的循环。



```kotlin
loop@ for (i in 1..100) {
    for (j in 1..100) {
        if (……) break@loop
    }
}
```

标签限定的 `break` 跳转到刚好位于该标签指定的循环后面的执行点。 `continue` 继续标签指定的循环的下一次迭代。



#### 返回到标签

如需从 lambda 表达式中返回，可给它加标签并用以限定 `return`。

```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return@forEach // 局部返回到该 lambda 表达式的调用者——forEach 循环
        print(it)
    }
    print(" done with implicit label")
}
fun main() {
    foo()
}
```

类似于 `continue` 的形式



### 异常

所有异常类继承自 `Throwable` 类。 每个异常都有消息、堆栈回溯信息以及可选的原因。

Kotlin 没有受检异常



### `Nothing`类型

**函数的返回类型：** 当函数永远不会正常返回（例如抛出异常或者无限循环）时，可以将函数的返回类型声明为 `Nothing`。这告诉编译器该函数不会返回任何结果。

**泛型类型参数的边界：** 在某些情况下，可以使用 `Nothing` 作为泛型类型参数的边界，因为它是所有类型的子类型，这在一些泛型场景中可能很有用。



### 区间迭代

```kotlin
for (i in 1..100) { …… }  // 闭区间：包含 100
for (i in 1..< 100) { …… } // 左开右闭区间：不包含 100
for (x in 2..10 step 2) { …… } //步长
for (x in 10 downTo 1) { …… } //反向
(1..10).forEach { …… }
```



## 类与对象

### 类

Kotlin 中使用 `class` 申明类

如果一个类没有类体，可以省略花括号。

#### 构造函数

一个类有一个*主构造函数*并可能有一个或多个*次构造函数*

**主构造函数：**

```kotlin
class Person constructor(firstName: String) { /*……*/ }
```

主构造函数初始化了一个类的实例和它的属性在类的头中，这个头不能包含任何可以执行的代码，使用`init{}`块来运行可执行程序

如果主构造函数中没有任何注解或者可见性修饰，可以省略 `constructor`

```kotlin
class Person(firstName: String) { /*……*/ }
```

可以设置默认值和变量可变性：

```kotlin
class Person(val firstName: String, val lastName: String, var isEmployed: Boolean = true)
```



**次构造函数：**

类也可以声明前缀有 `constructor`的*次构造函数*

如果类中有主构造函数，每个次构造函数需要委托给主构造函数，委托到同一个类的另一个构造函数用 `this` 关键字即可：

```kotlin
class Person(val name: String) {
    val children: MutableList<Person> = mutableListOf()
    constructor(name: String, parent: Person) : this(name) {
        parent.children.add(this)
    }
}
```

`init`块属于主构造函数的一部分，所以所有初始化块会先于次构造函数执行



### 继承

在 Kotlin 中所有类都有一个共同的超类 `Any`

默认情况下，Kotlin 类是最终（final）的——它们不能被继承。 要使一个类可继承，请用 `open` 关键字标记它：

```kotlin
open class Base // 该类开放继承
```

如需声明一个显式的超类型，请在类头中把超类型放到冒号之后：

```kotlin
class Derived(p: Int) : Base(p)
```

如果派生类有一个主构造函数，其基类可以（并且必须）根据其参数在该主构造函数中初始化。

如果派生类没有主构造函数，那么每个次构造函数必须使用`super` 关键字初始化其基类型，或委托给另一个做到这点的构造函数。 请注意，在这种情况下，不同的次构造函数可以调用基类型的不同的构造函数：

```kotlin
class MyView : View {
    constructor(ctx: Context) : super(ctx)

    constructor(ctx: Context, attrs: AttributeSet) : super(ctx, attrs)
}
```



#### 覆盖方法

对于基类需要重写的函数，必须要使用 `open` 修饰，子类必须要使用 `override` 修饰函数，注意想要继承一个类，必须要显示的对类 `open`



#### 覆盖属性

属性与方法的覆盖机制相同。

你也可以用一个 `var` 属性覆盖一个 `val` 属性，但反之则不行。 这是允许的，因为一个 `val` 属性本质上声明了一个 `get` 方法， 而将其覆盖为 `var` 只是在子类中额外声明一个 `set` 方法。

请注意，你可以在主构造函数中使用 `override` 关键字作为属性声明的一部分





### 属性

`var` 申明可变属性，`val` 申明只读属性

声明一个属性的完整语法如下：其初始器（initializer）、getter 和 setter 都是可选的。

```kotlin
var <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
    [<setter>]
```

如果自定义了一个 `getter` 函数，那么每次访问该属性的时候都会调用它：

```kotlin
class Rectangle(val width: Int, val height: Int) {
    val area: Int // property type is optional since it can be inferred from the getter's return type
        get() = this.width * this.height
}
```





#### 幕后字段

目的是将对数据的操作和数据分离开，这个幕后字段可以使用 `field` 标识符在访问器中引用

`field` 标识符只能用在属性的访问器内。



#### 编译器常量

如果只读属性的值在编译期是已知的，那么可以使用 `const` 修饰符将其标记为*编译期常量*。 这种属性需要满足以下要求：

- 必须位于顶层或者是 [`object` 声明](https://book.kotlincn.net/text/object-declarations.html#对象声明) 或*[伴生对象](https://book.kotlincn.net/text/object-declarations.html#伴生对象)*的一个成员
- 必须以 `String` 或原生类型值初始化
- 不能有自定义 getter

在注解中也可以使用常量



#### 延迟初始化属性与变量

如果一个属性是非空的，一定要在构造函数里面初始化，这往往不方便，用 `lateinit` 修饰符标记该属性，可以避免空检测

- 该修饰符只能用于在类体中的属性（不是在主构造函数中声明的 `var` 属性， 并且仅当该属性没有自定义 getter 或 setter 时）
- 也用于顶层属性与局部变量。 该属性或变量必须为非空类型，并且不能是原生类型。

在初始化前访问一个 `lateinit` 属性会抛出一个特定异常，该异常明确标识该属性被访问及它没有初始化的事实。

请在该属性的引用上使用 `.isInitialized`可以检测一个 `lateinit var` 是否被初始化



#### 延迟属性

```kotlin
val p: String by lazy { // 该值仅在首次访问时计算
    // 计算该字符串
}
```



### 接口

- 包含抽象方法的声明和实现
- 无法保存状态
- 可以又属性但必须为抽象或提供访问器实现
- 在接口中声明的属性不能有幕后字段（backing field），因此接口中声明的访问器不能引用它们



接口可以继承接口，可以提供实现也能声明新的函数和属性

如果继承多个接口，产生了函数冲突，需要重写



### 函数式（SAM）接口

只有一个抽象方法的接口称为*函数式接口*或 *单一抽象方法（SAM）*接口。函数式接口可以有多个非抽象成员，但只能有一个抽象成员。

可以用 `fun` 修饰符在 Kotlin 中声明一个函数式接口。

```kotlin
fun interface KRunnable {
   fun invoke()
}
```

#### SAM 转换

```kotlin
fun interface IntPredicate {
   fun accept(i: Int): Boolean
}
// 通过 lambda 表达式创建一个实例
val isEven = IntPredicate { it % 2 == 0 }
fun main() {
   println("Is 7 even? - ${isEven.accept(7)}")
}
```



### 可见性修饰

类、对象、接口、构造函数、方法与属性及其 setter 都可以有*可见性修饰符*。 getter 总是与属性有着相同的可见性。

在 Kotlin 中有这四个可见性修饰符：`private`、 `protected`、 `internal` 和 `public`。 默认可见性是 `public`。

#### 成员

对于类内部声明的成员：

- `private` 意味着只该成员在这个类内部（包含其所有成员）可见；
- `protected` 意味着该成员具有与 `private` 一样的可见性，但也在子类中可见。
- `internal` 意味着能见到类声明的*本模块内*的任何客户端都可见其 `internal` 成员。
- `public` 意味着能见到类声明的任何客户端都可见其 `public` 成员





### 扩展

Kotlin 能够对一个类或接口扩展新功能而无需继承该类或者使用像*装饰者*这样的设计模式。 这通过叫做*扩展*的特殊声明完成。

存在 扩展函数 和 扩展属性



#### 扩展函数

声明一个扩展函数需用一个*接收者类型*也就是**被扩展的类型**来作为他的前缀

```kotlin
fun MutableList<Int>.swap(index1: Int, index2: Int) {}
```

为了在接收者类型表达式中使用泛型，需要在函数名前声明泛型参数：

```kotlin
fun <T> MutableList<T>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // “this”对应该列表
    this[index1] = this[index2]
    this[index2] = tmp
}
```



#### 扩展是*静态*解析的

扩展不能够改变类，只是通过点表达式去调用新函数

如果扩展函数名与成员函数同名，会优先调用成员函数

如果变量不同，就能够分别调用





#### 扩展属性

```kotlin
val <T> List<T>.lastIndex: Int
    get() = size - 1
```

扩展属性并没有将成员实际插入到类中，因此对于扩展属性来说幕后字段是无效的，这就是为什么*扩展属性不能有初始化器*。他们的行为只能由显式提供的 getter/setter 定义。







### 数据类

主要用于存储数据：

```kotlin
data class User(val name: String, val age: Int)
```

编译器自动从主构造函数中声明的所有属性导出以下成员：

- `.equals()`/`.hashCode()` 对
- `.toString()` 格式是 `"User(name=John, age=42)"`
- [`.componentN()` 函数](https://book.kotlincn.net/text/destructuring-declarations.html) 按声明顺序对应于所有属性。
- `.copy()` 函数（见下文）



为了确保生成的代码的一致性以及有意义的行为，数据类必须满足以下要求：

- 主构造函数需要至少有一个参数。
- 主构造函数的所有参数需要标记为 `val` 或 `var`。
- 数据类不能是抽象、开放、密封或者内部的。



在类体中也可以声明属性，但是这个属性不会在上述的函数中出现。



### 密封类与密封接口

#### 密封类（Sealed Classes）：

- **限制继承结构**：密封类用来表示受限制的继承结构，即该类的子类是有限的且在同一个文件中声明。
- **使用关键字**：使用 `sealed` 关键字声明密封类。

```kotlin
sealed class Result
data class Success(val data: String) : Result()
data class Error(val message: String) : Result()
object Loading : Result()
```

使用密封类可以很好地结合 `when` 表达式，因为 `when` 表达式可以智能地识别出所有密封类的子类型。



### 泛型：in、out、where





### 枚举类





### 内联类







### 对象表达式与对象表达式





### 委托





### 属性别名





### 类型别名

```kotlin
typealias NodeSet = Set<Network.Node>
typealias FileTable<K> = MutableMap<K, MutableList<File>>
// 函数别名
typealias MyHandler = (Int, String, Any) -> Unit
```



## 函数

### 函数

`fun` 关键字



### 返回 Unit 的函数

如果一个函数并不返回有用的值，其返回类型是 `Unit`。`Unit` 是一种只有一个值——`Unit` 的类型。 这个值不需要显式返回



`Unit` 返回类型声明也是可选的，也可以不写明返回 `Unit` 。



### 显式返回类型

如果一个函数包含了 `{}`，则必须显示的指定返回类型，因为有时候对于编译器都是不明显的



### 交换两个变量

```kotlin
var a = 1
var b = 2
a = b.also { b = a }
```



## 类型安全的构建性





## 空安全





## 相等性

Kotlin 中有两类相等性：

- *结构*相等（`==`——用 `equals()` 检测）；
- *引用*相等（`===`——两个引用指向同一对象）。



## this表达式



## 异步程序设计技术



## 协程



## 注解





## 解构声明

把一个对象结构成很多变量，例如：

```kotlin
val (name, age) = person
```



### 解构声明与映射

可能遍历一个映射（map）最好的方式就是这样：

```kotlin
for ((key, value) in map) {
   // 使用该 key、value 做些事情
}
```

为使其能用，我们应该

- 通过提供一个 `iterator()` 函数将映射表示为一个值的序列。
- 通过提供函数 `component1()` 和 `component2()` 来将每个元素呈现为一对。

当然事实上，标准库提供了这样的扩展：

```kotlin
operator fun <K, V> Map<K, V>.iterator(): Iterator<Map.Entry<K, V>> = entrySet().iterator()
operator fun <K, V> Map.Entry<K, V>.component1() = getKey()
operator fun <K, V> Map.Entry<K, V>.component2() = getValue()
```

**下划线用于未使用的变量**





## 将代码标记为不完整（TODO）

Kotlin 的标准库有一个 `TODO()` 函数，该函数总是抛出一个 `NotImplementedError`。 其返回类型为 `Nothing`，因此无论预期类型是什么都可以使用它。 还有一个接受原因参数的重载：

```kotlin
fun calcTaxes(): BigDecimal = TODO("Waiting for feedback from accounting")
```

IntelliJ IDEA 的 kotlin 插件理解 `TODO()` 的语言，并且会自动在 TODO 工具窗口中添加代码指示。





## 对一个对象实例调用多个方法 （with）

```kotlin
class Turtle {
    fun penDown()
    fun penUp()
    fun turn(degrees: Double)
    fun forward(pixels: Double)
}

val myTurtle = Turtle()
with(myTurtle) { // 画一个 100 像素的正方形
    penDown()
    for (i in 1..4) {
        forward(100.0)
        turn(90.0)
    }
    penUp()
}
```



## 反射

Kotlin 中的函数和属性是一等公民，而对其自省（即在运行时获悉一个属性或函数的名称或类型）能力是使用函数式或反应式风格时所必需的。



### 类引用

*类字面值* 语法：

```kotlin
val c = MyClass::class
```



### 构造函数的引用

假设有一个简单的 `Person` 类：

```kotlin
class Person(val name: String, val age: Int)
```

获取对构造函数的引用可以这样做：

```kotlin
val constructorRef = ::Person // 获取构造函数的引用
```



# 多平台



# 平台



# 标准库





# 官方库





# 工具

## Gradle



## Dokka



