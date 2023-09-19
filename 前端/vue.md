# Vue

vite：快速构建网站

## 加载Vue.js

### 通过CDN使用VUE

```javascript
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```



### 使用ES模块构建

```vue
<script type="importmap">
  {
    "imports": {
      "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js"
    }
  }
</script>

<div id="app">{{ message }}</div>

<script type="module">
  import { createApp, ref } from 'vue'

  createApp({
    setup() {
      const message = ref('Hello Vue!')
      return {
        message
      }
    }
  }).mount('#app')
</script>
```



## 基础

### 创建一个Vue应用

每个 Vue 应用都是通过 [`createApp`](https://cn.vuejs.org/api/application.html#createapp) 函数创建一个新的 **应用实例**：

```javascript
import { createApp } from 'vue'

const app = createApp({
  /* 根组件选项 */
})
```

#### 挂载应用

应用实例必须在调用了 `.mount()` 方法后才会渲染出来。

```javascript
app.mount('#app')
```



### 响应式基础

#### 声明响应式状态

##### ref()

`ref()` 接收参数，并将其包裹在一个带有 `.value` 属性的 ref 对象中返回

要在组件模板中访问 ref，请从组件的 `setup()` 函数中声明并返回它们：

ps: 在模板中使用 ref 时，我们**不**需要附加 `.value`。



##### \<script setup\>

Vue 单文件组件 (Single-File Component)，简称SFC。

SFC 是一种可复用的代码组织形式，它将从属于同一个组件的 HTML、CSS 和 JavaScript 封装在使用 `.vue` 后缀的文件中。

我们可以使用 `<script setup>` 来大幅度地简化代码：



在标准的 JavaScript 中，检测普通变量的访问或修改是行不通的。然而，我们可以通过 getter 和 setter 方法来拦截对象属性的 get 和 set 操作。

 `.value` 属性给予了 Vue 一个机会来检测 ref 何时被访问或修改。在其内部，Vue 在它的 getter 中执行追踪，在它的 setter 中执行触发。



##### 深层响应性

`Ref` 可以持有任何类型的值，包括深层嵌套的对象、数组或者 JavaScript 内置的数据结构，比如 `Map`。

Ref 会使它的值具有**深层响应性**。这意味着即使改变嵌套对象或数组时，变化也会被检测到



##### DOM 更新时机

当你修改了响应式状态时，DOM 会被自动更新。但是，DOM 更新不是同步的。Vue 会在“next tick”更新周期中缓冲所有状态的修改，以确保不管你进行了多少次状态修改，每个组件都只会被更新一次。

要等待 DOM 更新完成后再执行额外的代码，可以使用 [nextTick()](https://cn.vuejs.org/api/general.html#nexttick) 全局 API：

```javascript
import { nextTick } from 'vue'

async function increment() {
  count.value++
  await nextTick()
  // 现在 DOM 已经更新了
}
```



#### reactivate()

`reactivate`的目的是使普通对象拥有响应性

`reactive()` 返回的是一个原始对象的代理（Proxy），它和原始对象是不相等的

只有代理对象是响应式的，更改原始对象不会触发更新。因此，使用 Vue 的响应式系统的最佳实践是 **仅使用你声明对象的代理版本**。



##### 局限性

1. **有限的值类型**：它只能用于对象类型 (对象、数组和如 `Map`、`Set` 这样的[集合类型](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects#keyed_collections))。它不能持有如 `string`、`number` 或 `boolean` 这样的[原始类型](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)。

2. **不能替换整个对象**：由于 Vue 的响应式跟踪是通过属性访问实现的，因此我们必须始终保持对响应式对象的相同引用。这意味着我们不能轻易地“替换”响应式对象，因为这样的话与第一个引用的响应性连接将丢失：

3. **对解构操作不友好**：当我们将响应式对象的原始类型属性解构为本地变量时，或者将该属性传递给函数时，我们将丢失响应性连接：

   ```javascript
   const state = reactive({ count: 0 })
   
   // 当解构时，count 已经与 state.count 断开连接
   let { count } = state
   // 不会影响原始的 state
   count++
   
   // 该函数接收到的是一个普通的数字
   // 并且无法追踪 state.count 的变化
   // 我们必须传入整个对象以保持响应性
   callSomeFunction(state.count)
   ```

由于这些限制，我们建议使用 `ref()` 作为声明响应式状态的主要 API。





### 计算属性

推荐使用**计算属性**来描述依赖响应式状态的复杂逻辑

`computed()` 方法期望接收一个 getter 函数，返回值为一个**计算属性 ref**。

```html
<script setup>
import { reactive, computed } from 'vue'

const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})

// 一个计算属性 ref
const publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No'
})
</script>

<template>
  <p>Has published books:</p>
  <span>{{ publishedBooksMessage }}</span>
</template>
```

Vue 的计算属性会自动追踪响应式依赖。它会检测到 `publishedBooksMessage` 依赖于 `author.books`，所以当 `author.books` 改变时，任何依赖于 `publishedBooksMessage` 的绑定都会同时更新。



不难发现，这里的publishedBooksMessage换成一个函数也一样可以执行，但是不一样的是，使用计算属性依赖于缓存，如果books不变，那么计算属性会直接返回缓存，而不会重新计算，这样可以有效避免大量的计算。

但值得注意的是，使用计算属性的时候不能使用`Date.now()`，这样的函数，因为这不是一个响应式依赖，所以其永远不会更新



### 数据绑定

#### Attribute 绑定

为了给Attribute绑定一个动态值，需要使用`v-bind`指令：

```vue
<div v-bind:id="dynamicId"></div>
```

或者可以使用一种简写的形式：

```vue
<div :id="dynamicId"></div>
```



#### Class和Style绑定

Vue 专门为 `class` 和 `style` 的 `v-bind` 用法提供了特殊的功能增强，表达式的值可以是对象或数组。

##### 绑定 HTML class

```html
<div
  class="static"
  :class="{ active: isActive, 'text-danger': hasError }"
></div>
```

isActive和hasError是布尔值，假设`isActive`和`hasError`分别是`true`和`false`

那么结果是`class="static active"`

当 `isActive` 或者 `hasError` 改变时，class 列表会随之更新。举例来说，如果 `hasError` 变为 `true`，class 列表也会变成 `"static active text-danger"`。



绑定的对象并不一定需要写成内联字面量的形式，也可以直接绑定一个对象：

```javascript
const classObject = reactive({
  active: true,
  'text-danger': false
})
```

```html
<div :class="classObject"></div>
```

这段描述与前面是一样的



### 条件渲染

`v-if`和`v-else-if`和`v-else`共同控制DOM



#### `<template>` 上的 `v-if`

因为 `v-if` 是一个指令，他必须依附于某个元素。但如果我们想要切换不止一个元素呢？在这种情况下我们可以在一个 `<template>` 元素上使用 `v-if`，这只是一个不可见的包装器元素，最后渲染的结果并不会包含这个 `<template>` 元素。

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

`v-else` 和 `v-else-if` 也可以在 `<template>` 上使用。





#### v-show

`v-show`会在DOM渲染中保留该元素；`v-show`仅切换了`display`属性



不建议同时使用`v-if`和`v-show`



### 列表渲染

#### `v-for`

`v-for` 指令的值需要使用 `item in items` 形式的特殊语法，其中 `items` 是源数据的数组，而 `item` 是迭代项的**别名**：

在 `v-for` 块中可以完整地访问父作用域内的属性和变量。`v-for` 也支持使用可选的第二个参数表示当前项的位置索引。

```html
<li v-for="(item, index) in items">
  {{ parentMessage }} - {{ index }} - {{ item.message }}
</li>
```

- Parent - 0 - Foo
- Parent - 1 - Bar





推荐在任何可行的时候为 `v-for` 提供一个 `key attribute`，除非所迭代的 DOM 内容非常简单 (例如：不包含组件或有状态的 DOM 元素)，或者你想有意采用默认行为来提高性能。

key 绑定的值期望是一个基础类型的值，例如字符串或 number 类型。不要用对象作为 v-for 的 key。关于 key attribute 的更多用途细节



#### 数组变化侦测

变更方法：

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`



`filter()`，`concat()` 和 `slice()`会返回一个新的数组，而不会更改原数组



### 事件处理

#### 事件监听

我们可以使用 `v-on` 指令监听 DOM 事件：

template

```vue
<button v-on:click="increment">{{ count }}</button>
```

因为其经常使用，`v-on` 也有一个简写语法：

```vue
<button @click="increment">{{ count }}</button>
```



事件处理器 (handler) 的值可以是：

1. **内联事件处理器**：事件被触发时执行的内联 JavaScript 语句 (与 `onclick` 类似)。
2. **方法事件处理器**：一个指向组件上定义的方法的属性名或是路径。



#### 事件修饰符

为了希望调用的方法更加专注于数据逻辑而不用处理DOM事件的细节会更好，包含以下修饰符

- `.stop`
- `.prevent`
- `.self`
- `.capture`
- `.once`
- `.passive`

```html
<!-- 单击事件将停止传递，该事件不会传递给父类 -->
<a @click.stop="doThis"></a>

<!-- 提交事件将不再重新加载页面，阻止默认事件 -->
<form @submit.prevent="onSubmit"></form>

<!-- 修饰语可以使用链式书写 -->
<a @click.stop.prevent="doThat"></a>

<!-- 也可以只有修饰符 -->
<form @submit.prevent></form>

<!-- 仅当 event.target 是元素本身时才会触发事件处理器 -->
<!-- 例如：事件处理器不来自子元素 -->
<div @click.self="doThat">...</div>
```



`.capture`实现捕获触发事件的机制。即是给元素添加一个监听器，当元素发生冒泡时，先触发带有该修饰符的元素。（若有多个该修饰符，则由外而内触发）。等于是修改触发的顺序。

`.once` 表示只触发一次事件处理函数。先来看看没有.once的情况。

`.self` 实现只有点击当前元素时候，才会触发事件处理函数。



### 表单绑定

可以同时使用`v-on`和`v-bind`实现表单输入

但是vue里面更推荐使用`v-model`

```vue
<input v-model="text">
```



### 生命周期钩子

> 让开发者有机会在特定阶段运行自己的代码



### 侦听器

#### 基本实例

使用`watch函数`在每次响应式状态发生变化时触发回调函数

```javascript
// 可以直接侦听一个 ref
watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.indexOf('?') > -1) {
    answer.value = 'Thinking...'
    try {
      const res = await fetch('https://yesno.wtf/api')
      answer.value = (await res.json()).answer
    } catch (error) {
      answer.value = 'Error! Could not reach the API. ' + error
    }
  }
})
```



#### 侦听的数据类型

`watch`的第一个参数可以是不同形式的数据源：ref(包括计算属性)，getter函数，响应式对象，或者一个数组

ps: 你不能够直接监听一个响应式对象的属性值，你需要返回一个该属性的getter函数

```javascript
// 提供一个 getter 函数
watch(
  () => obj.count,
  (count) => {
    console.log(`count is: ${count}`)
  }
)
```



#### 深层监听器

直接监听一个响应式对象时，会创建深层监听器，会在所有嵌套发生变化时触发

相比之下，一个返回响应式对象的 getter 函数，只有在返回不同的对象时，才会触发回调

你也可以给上面这个例子显式地加上 `deep` 选项，强制转成深层侦听器：

```javascript
watch(
  () => state.someObject,
  (newValue, oldValue) => {
    // 注意：`newValue` 此处和 `oldValue` 是相等的
    // *除非* state.someObject 被整个替换了
  },
  { deep: true }
)
```

> 深度侦听需要遍历被侦听对象中的所有嵌套的属性，当用于大型数据结构时，开销很大。因此请只在必要时才使用它，并且要留意性能。



## 组件

### 组件基础

组件允许我们将 UI 划分为**独立的、可重用的**部分，并且可以对每个部分进行单独的思考。在实际应用中，组件常常被组织成层层嵌套的树状结构：

![组件树](assets/components.7fbb3771.png)

组件最重要的目的：可复用

#### 定义组件

使用单独的`.vue`文件定义一个组件，被称为SFC

#### 使用组件

需要在父组件中导入它。假设我们把计数器组件放在了一个叫做 `ButtonCounter.vue` 的文件中，这个组件将会以默认导出的形式被暴露给外部。

```html
<script setup>
import ButtonCounter from './ButtonCounter.vue'
</script>

<template>
  <h1>Here is a child component!</h1>
  <ButtonCounter />
</template>
```



#### 传递props

使用场景：在博客首页有一个表示博客文章的组件，我们希望每个组件拥有相同的样式，但是使用不同的名字

Props 是一种特别的 attributes，你可以在组件上声明注册。要传递给博客文章组件一个标题，我们必须在组件的 props 列表上声明它。这里要用到 [`defineProps`](https://cn.vuejs.org/api/sfc-script-setup.html#defineprops-defineemits) 宏：

```html
<!-- BlogPost.vue -->
<script setup>
defineProps(['title'])
</script>

<template>
  <h4>{{ title }}</h4>
</template>
```

props接收任何值，
