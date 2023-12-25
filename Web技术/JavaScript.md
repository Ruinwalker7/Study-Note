# JavaScript

## 基础

浏览器分为两个引擎：渲染引擎和JS引擎：

⚫ 渲染引擎：解析HTML和CSS，例如Chrome的Webkit和blink

⚫ JS引擎：JS解释器，用于读取和执行网页中Javascript代码，例如Chrome的V8

### JavaScript 用法

1. `<script>`标签
2. 外部`<script src="myScript.js"></script>`

### 输出

- 使用 `window.alert()` 写入警告框
- 使用 `document.write()` 写入 HTML 输出
- 使用 `innerHTML` 写入 HTML 元素
- 使用 `console.log()` 写入浏览器控制台
- 使用`prompt()`输入



模板字符串：`${name}`

### 变量

◼ JS当中，函数分为**声明式函数和赋值式函数**，声明式函数与赋值式函数的区别在于：

在JS的预编译期，声明式函数将会先被提取出来，然后才按顺序执行js代码

◼ JS的解析过程分为两个阶段**编译期与执行期**

◼ DOM—文档对象模型

- 文档对象模型（Document Object Model，简称DOM），是W3C组织推荐的处理可扩展标记语言的标准编程接口。通过 DOM 提供的接口可以对页面上的各种元素进行操作。

◼ BOM—浏览器对象模型

- BOM (Browser Object Model，简称BOM) 是指浏览器对象模型，它提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。通过BOM可以操作浏览器窗口，比如弹出框、控制浏览器跳转、获取分辨率等。



`strict`模式

- 不强制要求用var申明变量，一个变量没有申明就被使用，自动申明为全局变量

- strict模式下运行JS代码，强制通过var申明变量 `'use strict';`


在JavaScript中，`var`, `let`, 和 `const` 是用来声明变量的关键字，它们各自有不同的行为特性：

1. **`var`**:
   - **作用域**: `var` 声明的变量有函数作用域，如果在函数外部声明，它具有全局作用域。
   - **提升（Hoisting）**: `var` 声明的变量会被提升到其作用域的顶部，但只是声明被提升，初始化不会被提升。
   - **重声明**: 可以在同一作用域中多次声明同一个变量。
2. **`let`**:
   - **作用域**: `let` 声明的变量具有块级作用域（例如在一个if语句或循环中）。
   - **提升**: 不会被提升。在声明之前的访问会导致`ReferenceError`。
   - **重声明**: 在相同作用域中不能重复声明。
   - **更新**: 值可以被更新。
3. **`const`**:
   - **作用域**: 同`let`，`const` 也是块级作用域。
   - **提升**: 与`let`一样，`const`声明的变量也不会被提升。
   - **重声明**: 在相同作用域中不能重复声明。
   - **更新**: 声明后必须立即初始化，且之后不能被重新赋值。但如果变量是一个对象或数组，其属性或元素是可以被重新赋值的



#### Value = undefined

在计算机程序中，经常会声明无值的变量。未使用值来声明的变量，其值实际上是 undefined。

在执行过以下语句后，变量 carname 的值将是 undefined：

```js
var carname;
```

#### 重新声明 JavaScript 变量

如果重新声明 JavaScript 变量，该变量的值不会丢失：

在以下两条语句执行后，变量 carname 的值依然是 "Volvo"：

```js
var carname="Volvo";
var carname;
```

#### 作用域

局部变量：在函数中通过var声明的变量。

全局变量：在函数外通过var声明的变量。

没有声明就使用的变量，默认为全局变量，不论这个变量在哪被使用。

#### 变量类型

值类型(基本类型)：字符串（String）、数字(Number)、布尔(Boolean)、空（Null）、未定义（Undefined）、Symbol。

引用数据类型（对象类型）：对象(Object)、数组(Array)、函数(Function)，还有两个特殊的对象：正则（RegExp）和日期（Date）。

- Number：不区分整数和浮点数
- 字符串：以单引号'或双引号"括起来的任意文本
- 布尔值：true、false
- 数组：用[]表示，元素之间用,分隔
- 对象：一组由键-值组成的无序集合
- NaN：NaN表示Not a Number，当无法计算结果时用NaN表示
- Infinity：Infinity表示无限大，当数值超过了JavaScript的Number所能
  - 表示的最大值时，就表示为Infinity
- null：一个“空”的值，表示“没有对象”，可用于函数的参数
- Undefined：表示“未定义”，一般用于已定义但没有赋值

变量的数据类型可以使用 **`typeof`** 操作符来查看：

强制转换

```javascript
Number("string")
parseFloat("100px") //常用于过滤单位
parseInt() //保留整数
```

##### 对象

在JavaScript中，对象是一组无序的相关属性和方法的集合，所有的事物都是对象，例如字符 串、数值、数组、函数等。

对象是由**属性**和**方法**组成的：

- 属性：反映事物的特征
- 方法：反映事物的行为

创建一个对象：

```javascript
var person={
"name":"小明",
"age":"18",
"like":function(){
            return "喜欢打篮球,弹吉他";
      }
}

var person=new Object();
person.name='小明'；
person.sex='男'；
person.method=function(){
  return this.name+this.sex;
}
```

### 使用 innerHTML

如需访问 HTML 元素，JavaScript 可使用 `document.getElementById(id)` 方法。

### 函数

#### 局部 JavaScript 变量

在 JavaScript 函数内部声明的变量（使用 var）是*局部*变量，所以只能在函数内部访问它。（该变量的作用域是局部的）。

您可以在不同的函数中使用名称相同的局部变量，因为只有声明过该变量的函数才能识别出该变量。

只要函数运行完毕，本地变量就会被删除。

------

#### 全局 JavaScript 变量

在函数外声明的变量是*全局*变量，网页上的所有脚本和函数都能访问它。

------

#### JavaScript 变量的生存期

JavaScript 变量的生命期从它们被声明的时间开始。

局部变量会在函数运行以后被删除。

全局变量会在页面关闭后被删除。

------

#### 向未声明的 JavaScript 变量分配值

如果您把值赋给尚未声明的变量，该变量将被自动作为 window 的一个属性。

这条语句：

carname="Volvo";

将声明 window 的一个属性 carname。

非严格模式下给未声明变量赋值创建的全局变量，是全局对象的可配置属性，可以删除。

```javascript
var var1 = 1; // 不可配置全局属性
var2 = 2; // 没有使用 var 声明，可配置全局属性

console.log(this.var1); // 1
console.log(window.var1); // 1
console.log(window.var2); // 2

delete var1; // false 无法删除
console.log(var1); //1

delete var2; 
console.log(delete var2); // true
console.log(var2); // 已经删除 报错变量未定义
```

### 事件

1、在标签中填写 onclick 事件调用函数时，不是 **onclick=函数名**， 而是 **onclick=函数名+()**，代码如下：

```html
<script> 
function myfunction(){
    document.getElementById("demo").innerHTML="onclick事件触发";
}
</script>
</head>
<body>
    <h1 id="demo">一个段落</h1>
    <button onclick="myfunction()" type="button">点击这里</button>
</body>
```

### 比较

在JavaScript中判断两个对象是否相等可以有多种方法，但由于对象是引用类型，直接比较它们通常会比较它们的引用而不是它们的内容。以下是一些常用的方法来判断两个对象是否“相等”：

1. **浅比较**:
   这种方法只比较对象的第一层属性。这可以通过遍历对象的每个属性并比较它们的值来实现。但是，这种方法不会比较嵌套对象或数组内的元素。

   ```js
   1function shallowEqual(obj1, obj2) {
   2    if (obj1 === obj2) {
   3        return true;
   4    }
   5    if (typeof obj1 !== 'object' || obj1 === null || typeof obj2 !== 'object' || obj2 === null) {
   6        return false;
   7    }
   8    const keys1 = Object.keys(obj1);
   9    const keys2 = Object.keys(obj2);
   10    if (keys1.length !== keys2.length) {
   11        return false;
   12    }
   13    for (let key of keys1) {
   14        if (!obj2.hasOwnProperty(key) || obj1[key] !== obj2[key]) {
   15            return false;
   16        }
   17    }
   18    return true;
   19}
   ```

2. **深度比较**:
   深度比较会递归地比较所有属性和子属性。这种方法更加全面，但也更消耗资源。你可以自己编写深度比较函数，或使用像Lodash这样的库提供的`isEqual`函数。

   ```js
   1function deepEqual(obj1, obj2) {
   2    if (obj1 === obj2) {
   3        return true;
   4    }
   5    if (typeof obj1 !== 'object' || obj1 === null || typeof obj2 !== 'object' || obj2 === null) {
   6        return false;
   7    }
   8    const keys1 = Object.keys(obj1);
   9    const keys2 = Object.keys(obj2);
   10    if (keys1.length !== keys2.length) {
   11        return false;
   12    }
   13    for (let key of keys1) {
   14        if (!obj2.hasOwnProperty(key)) {
   15            return false;
   16        }
   17        if (typeof obj1[key] === 'object' && typeof obj2[key] === 'object') {
   18            if (!deepEqual(obj1[key], obj2[key])) {
   19                return false;
   20            }
   21        } else if (obj1[key] !== obj2[key]) {
   22            return false;
   23        }
   24    }
   25    return true;
   26}
   ```

## 函数

使用var申明的变量是有作用域的

局部作用域

- 变量作用域实际上是函数内部，我们在for循环等语句块中是无法定义具有局部作用域的变量
- 关键字let

变量提升：变量和函数的声明会在物理层面移动到代码的最前

JavaScript引擎自动提升了变量y的声明，但不会提升变量y的赋值

**高阶函数：**

- 一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数

- reduce：对数组中的每个元素按序执行一个由您提供的 reducer 函数，每一次运行 reducer 会
  将先前元素的计算结果作为参数传入

  ![image-20231101185132102](assets/image-20231101185132102.png)

- sort：数组的元素进行排序，并返回数组。默认排序顺序是在将元素转换为字符串

- 闭包定义：

  是一个函数以及其捆绑的周边环境状态（lexical environment，词法环境）的引用的组合

## 面向对象编程

不区分类和实例的概念，通过原型（prototype）来实现面向对象编程

- __proto__(或[[Prototype]]):指向该对象的构造函数的原型对象
  (已弃用: 不再推荐使用该特性)，推荐使用Object.getPrototypeOf()
- prototype：指向该方法的原型对象, 只有函数才有prototype属性
- constructor:返回对象的构造函数

<img src="assets/image-20230927170354788.png" alt="image-20230927170354788" style="zoom: 50%;" />

所谓继承关系不过是把一个对象的原型指向另一个对象而已



## 标准对象

1. `Object`

   - **`Object.assign()`**：用于复制一个或多个源对象的所有可枚举属性到目标对象。

   - **`Object.keys()`**：返回一个包含给定对象所有自身属性（非继承属性）的名称的数组。

2. `Array`

   - **`Array.map()`**：创建一个新数组，其结果是该数组中的每个元素调用一次提供的函数后的返回值。

   - **`Array.filter()`**：创建一个新数组，包含通过所提供函数实现的测试的所有元素。

   - **`Array.reduce()`**：对数组中的每个元素执行一个由您提供的reducer函数（升序执行），将其结果汇总为单个返回值。

3. `String`

   - **`String.includes()`**：判断一个字符串是否包含在另一个字符串中，根据情况返回true或false。

   - **`String.slice()`**：提取某个字符串的一部分，并返回一个新的字符串，不会修改原字符串。

4. `Number`

   - **`Number.isFinite()`**：检查一个数值是否为有限数。

   - **`Number.parseInt()`**：将一个字符串参数解析成整数。

5. `Math`

   - **`Math.random()`**：返回一个浮点, 伪随机数在范围从0到小于1，即[0, 1)。

   - **`Math.max()`, `Math.min()`**：返回给定数字中最大值和最小值。

6. `Date`

   - **`Date.now()`**：返回自1970年1月1日 00:00:00 UTC以来经过的毫秒数。

   - **`new Date()`**：创建一个新的Date对象，表示当前日期和时间。

7. `RegExp`

   - **`RegExp.test()`**：测试给定的字符串是否符合正则表达式。

   - **`RegExp.exec()`**：在一个指定字符串中执行一个搜索匹配。

8. `Function`

   - **`Function.call()`**：调用一个函数, 其具有一个指定的`this`值和分别地提供的参数（参数的列表）。

   - **`Function.apply()`**：调用一个函数, 其具有一个指定的`this`值，以及作为一个数组（或类数组对象）提供的参数。

9. `JSON`

   - **`JSON.stringify()`**：将一个JavaScript值（对象或者数组）转换为一个 JSON字符串。

   - **`JSON.parse()`**：解析一个JSON字符串，构造由字符串描述的JavaScript值或对象。

## DOM和浏览器对象

HTML 输出流中使用 document.write，相当于添加在原有html代码中添加一串html代码。而如果在文档加载后使用（如使用函数），会覆盖整个文档。

使用函数来执行document.write代码如下：

```html
<script>
function myfunction(){
    document.write("使用函数来执行document.write，即在文档加载后再执行这个操作，会实现文档覆盖");
}
document.write("<h1>这是一个标题</h1>");
document.write("<p>这是一个段落。</p>");
</script>
<p>
您只能在 HTML 输出流中使用 <strong>document.write</strong>。
如果您在文档已加载后使用它（比如在函数中），会覆盖整个文档。
</p>
<button type="button" onclick="myfunction()">点击这里</button>
```

### DOM元素查找

- 根据ID查找：`document.getElementById(id)`
  - Id名称大小写敏感
  - 返回值：拥有指定 ID 的第一个元素
- 根据**标签名**查找元素： `document.getElementsByTagName(name)`
  - name可以不区分大小写
- 根据name属性查找元素： `document.getElementsByName(name)`
- 根据**类名**查找元素： `document. getElementsByClassName(className)`
- 根据**选择器**查找匹配的第一个元素： `document.querySelector(selName)`
- 根据**选择器**查找所有元素： `document.querySelectorAll(selName)`
- body：doucumnet.body
- html：document.documentElement

### DOM元素操作

◼ 修改元素内容：element.innerText， element.innerHTML 

◼ innerText: 设置或者获取一个元素及其后代的“渲染”文本内容 

◼ innerHTML：设置或者获取一个元素后代的全部HTML文档

◼修改元素属性：src、href、alt 

◼ 表单元素属性：type、value、checked、selected、disabled

修改元素样式：element.style，element.className

> CSS允许font-size这样的名称，但它并非JavaScript有效的属性名，所以需要在 JavaScript中改写为驼峰式命名fontSize

**自定义属性：**

element.getAttribute("attr")/setAttribute("attr’,’value")/removeAttribute("attr")

◼HTML5自定义属性：data-开头做为属性名并且赋值

### DOM元素节点操作

- 所有内容都有可以表示为节点node
- DOM树中所有节点均可通过JavaScript进行访问
- 所有节点均可被创建删除和修改



父节点：`node.parentNode`

- 返回节点最近一个父节点，没有就返回null


子节点：`node.childNodes`

- 返回包含指定节点的子节点的集合，该集合为即时更新的集合


子节点：`parentNode.children`

- 与childNodes不同，只返回子元素节点，其余节点不返




◼ parentNode.firstChild：返回第一个子节点
◼ parentNode.lastChild：返回最后一个子节点
◼ parentNode.firstElementChild：返回第一个子元素节点
◼ parentNode.lastElementChild：返回最后一个子元素节点

◼ 兄弟节点： node.nextSibling、node.previousSibling 返回当前元素的下/前一个兄弟节点 

◼ node.nextElementSibling、node.previousElementSibling：返回当前元素的下/前 一个兄弟元素节点 

◼ 如何解决nextElementSibling、previousElementSibling兼容性问题

◼ 创建元素： document.createElement('tag') //创建由 tag 指定的 HTML 元素

◼ 添加节点： node.appendChild(childNode) //添加到子节点列表末尾 node.insertBefore(newNode, referenceNode) //添加元素到参考子节点之前 

◼ 删除节点 node.removeChild(childNode) //删除指定子节点

### 事件基础

三要素：事件源、事件类型、处理程序

![image-20230927173705805](assets/image-20230927173705805.png)

获取事件源、注册事件、添加事件处理程序

主要区别：<font color=red>注册事件的唯一性</font>

#### 注册删除事件

1. element.addEventListener(event, function, useCapture);
2. element.on+event()= function();

删除：

1. eventTarget.onclick = null
2. element.removeEventListener(type, listener, useCapture)  //useCapture需要和addEventListener时设置匹配 
3. element.detachEvent(eventNameWithOn, listener)

#### DOM浏览器事件流

事件发生会在元素节点之间**按特定的顺序传播**，这个传播过程称之为**事件流**

事件流三个阶段：1、捕获阶段 2、当前目标阶段 3、冒泡阶段

#### DOM事件对象

触发DOM上某个事件时，会产生一个**事件对象**

◼ e.target 和 this 的区别：

◼ e.target：触发事件的对象 (某个 DOM 元素) 的引用

◼ this：事件绑定元素和e.currentTarget一致

◼ 在事件的冒泡或捕获阶段被调用时两者的区别

#### DOM事件委托

事件委托是一种利用事件冒泡原理来优化事件监听的技术。通过在父元素上设置一个事件监听器来管理所有子元素的相同事件，而不是给每个子元素单独设置事件监听器。

例子：对于表格中任何一个元素都能点击？

可以将事件监听器设置在其父节点上，并让子节点上发生的事件冒泡到父节点上， 这种称为<font color=red>事件委托</font>

#### DOM事件冒泡

事件冒泡是指在一个元素上触发的事件不仅会被该元素处理，还会依次向上传递给其父级元素，一直到`document`对象。这意味着一个事件可以被多层嵌套的元素共同处理。

**作用**：

- **代码简洁**：允许你在一个共同的父级元素上设置事件监听器，而不是在每个子元素上单独设置。
- **动态元素**：对于动态添加到页面的元素（比如通过AJAX加载的内容），你不需要重新绑定事件监听器，只要它们位于已有事件监听器的元素内部。

##### 阻止事件冒泡

**什么时候需要**：

- **特定处理**：当你不希望事件继续向上传递，只想在特定的子元素上处理事件。
- **避免冲突**：在复杂的应用中，防止事件冒泡可能有助于避免不同组件间的潜在冲突。

**如何实现**：

在事件处理函数中使用`event.stopPropagation()`方法可以阻止事件进一步冒泡。

### BOM (浏览器对象) 

- 提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是 window 
- 不存在浏览器对象模型（BOM）的官方标准

<img src="assets/image-20230928152500475.png" alt="image-20230928152500475" style="zoom: 67%;" />

#### 1. `window`

- **`window.document`**：指向与窗口关联的`document`对象，是DOM的入口点。
- **`window.location`**：提供了有关当前URL的信息，并允许JavaScript重定向到新的URL。
- **`window.history`**：允许操作浏览器会话历史（即浏览器的后退、前进按钮）。
- **`window.screen`**：包含了有关用户屏幕的信息，如分辨率。
- **`window.alert()`, `window.confirm()`, `window.prompt()`**：用于显示对话框。
- **`window.setTimeout()`, `window.setInterval()`**：用于时间控制，分别用于设置延时函数和重复执行的函数。
- **`window.open()`, `window.close()`**：用于打开和关闭新窗口。

#### 2. `document`

虽然严格来说`document`属于DOM的一部分，但它通常与BOM一起讨论。

- **`document.getElementById()`, `document.getElementsByClassName()`, `document.getElementsByTagName()`**：用于选取DOM元素。
- **`document.querySelector()`, `document.querySelectorAll()`**：提供了更强大的选择器。
- **`document.cookie`**：允许访问和修改当前网页的cookie。
- **`document.createElement()`, `document.createTextNode()`**：用于动态创建DOM元素和文本节点。

#### 3. `navigator`

- **`navigator.userAgent`**：提供关于用户浏览器的信息。
- **`navigator.geolocation`**：用于获取用户的地理位置（需要用户许可）。

#### 4. `screen`

- **`screen.width`, `screen.height`**：显示屏幕的宽度和高度。
- **`screen.availWidth`, `screen.availHeight`**：显示可用的屏幕宽度和高度（不包括系统任务栏等）。

#### 5. `location`

- **`location.href`**：当前页面的URL。
- **`location.hostname`**, **`location.pathname`**, **`location.search`**：分别提供URL的域名部分、路径部分和查询字符串部分。
- **`location.reload()`, `location.assign()`, `location.replace()`**：用于页面重载和导航。

#### 6. `history`

- **`history.back()`, `history.forward()`**：模拟浏览器的后退和前进按钮。
- **`history.pushState()`, `history.replaceState()`**：允许在浏览历史中添加或修改记录。



## 同步和异步机制

<img src="assets/image-20230928152716430.png" alt="image-20230928152716430" style="zoom:80%;" />

JavaScript 是以单线程的方式运行的

◼ 同步任务：在主线程上顺序执行，形成一个执行栈

◼ 异步任务：JS异步依赖于回调函数

### 回调

回调是实现 javaScript 异步的主要方式

`setTimeout(exp, millisec)`: 在指定的毫秒数后调用函数或计算表达式

异步回调的嵌套会是一个灾难

### Promise

JavaScript 中异步编程的基础是 Promise

◼定义：用于表示一个异步操作的最终完成（或失败）及其结果值。

◼三大状态：pending（进行中）、fulfilled（成功）、rejected（失败）

◼特点：不受外界影响和不可逆

#### 使用

◼ 构造函数：接受一个函数为参数，该函数内置resolve和reject两个内置函数。

◼ resovle函数：将Promise状态从 "pending=> fulfilled "

◼ reject函数：将Promise状态从 "pending=> rejected "

◼Promise.prototype.then:为Promise实例添加状态改变时的回调函数。 

◼then方法传入两个函数：resolved状态的回调函数，rejected状态的回调函数(可选) 

◼then方法返回的是一个新的Promise实例 

◼catch方法：.then(null, rejection)的别名，用于指定发生错误时的回调函数

#### 常用函数

◼Promise.resolve(value):返回一个以给定值解析后的 Promise 对象 

◼Promise.reject(error):返回一个带有拒绝原因的 Promise 对象 

◼Promise.all(iterable of promise):等待所有的promise对象执行完成 

◼Promise.race(iterable of promise)：等待任一promise 解决或拒绝

### async 和 await

async 是“异步”的简写，而 await 可以认为是 async wait 的简写。所以应该很好理解 async 用于申明一个 function 是异步的，而 await 用于等待一个异步方法执行完成。

async 的返回值是一个 Promise 对象

如果在函数中 `return` 一个直接量，async 会把这个直接量通过 `Promise.resolve()` 封装成 Promise 对象。

如果没有 await ，async会马上返回一个Primise对象

async 函数返回一个 Promise 对象，所以 await 可以用于等待一个 async 函数的返回值——这也可以说是 await 在等 async 函数

### 微任务和宏任务

◼宏任务：由事件触发的任务，script(整体代码)、setTimeout、setInterval、 setImmediate(Node.js 环境)、UI事件、I/O（Node.js）… 

◼微任务：由JavaScript代码创建产生，通常是由promise创建的，对.then /catch /  finally处理程序的执行会成为微任务（其他如async/await, process.nextTick等）

## 错误处理

**try** 语句测试代码块的错误。

**catch** 语句处理错误。

**throw** 语句创建自定义错误。

**finally** 语句在 try 和 catch 语句之后，无论是否有触发异常，该语句都会执行。