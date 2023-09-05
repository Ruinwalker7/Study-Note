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

### 声明式渲染

Vue 单文件组件 (Single-File Component)，简称SFC。

SFC 是一种可复用的代码组织形式，它将从属于同一个组件的 HTML、CSS 和 JavaScript 封装在使用 `.vue` 后缀的文件中。



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
