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

### Attribute 绑定

为了给Attribute绑定一个动态值，需要使用`v-bind`指令：

```vue
<div v-bind:id="dynamicId"></div>
```

或者可以使用一种简写的形式：

```vue
<div :id="dynamicId"></div>
```



### 事件监听

我们可以使用 `v-on` 指令监听 DOM 事件：

template

```vue
<button v-on:click="increment">{{ count }}</button>
```

因为其经常使用，`v-on` 也有一个简写语法：

```vue
<button @click="increment">{{ count }}</button>
```





### 表单绑定

可以同时使用`v-on`和`v-bind`实现表单输入

但是vue里面更推荐使用`v-model`

```vue
<input v-model="text">
```





### 条件控制

`v-if`和`v-else-if`和`v-else`共同控制DOM



