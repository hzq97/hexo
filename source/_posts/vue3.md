---
title: vue3
tags: 
    - vue
categories: 
    - vue
---
##  全局配置
### errorHandler
> 处理和渲染函数和侦听器执行期间抛出未捕获的错误

```javascript
app.config.errorHandler = (err, vm, info) => {}
```

### warnHandler
> 为Vue运行时提供一个警告函数。只有**开发环境**使用

```javascript
app.config.warnHandler = (err, vm, info) => {}
```

### globalProperties
> 为全局所有组件添加一个可访问的property
- 2.0添加方式是在prototype上直接添加

```javascript
// 2.0
Vue.prototype.$http = () => {}
// 3.0
app.config.globalProperties.$http = () => {}
```

### optionMergeStrategies
> 自定义合并策略

```javascript
app.config.optionMergeStrategies.hello = (parent, child) => {
    return 'Hello,' + ${child}
}
app.mixin({
    hello: 'Vue'
})
```

### performance
> 设置为true，开启Vue在浏览器的性能追踪，只适用于**开发模式**

### compilerOptions
> 配置编译器的选项

####    compilerOptions.isCustomElement
> 忽略任何未进行注册的组件

```javascript
// 任何以 'ion-' 开头的元素都会被识别为自定义元素
app.config.compilerOptions.isCustomElement = tag => tag.startsWith('ion-')
```

####    compilerOptions.whitespace
> Vue压缩空格设置

- 值：'condense' | 'preserve'  默认值：'condense'

```
1.元素内的多个开头/结尾空格会被压缩成一个空格
2.元素之间的包括折行在内的多个空格会被移除
3.文本结点之间可被压缩的空格都会被压缩成为一个空格
将值设置为 'preserve' 可以禁用 2 和 3。
```

####    compilerOptions.delimiters
> 配置模板插值分隔符

```javascript
// 将分隔符设置为 ES6 模板字符串风格
app.config.compilerOptions.delimiters = ['${','}']
```

####    compilerOptions.comments
> 设置为true，可保留生产环境的注释

### 与2.0不同之处
- 取消silent配置，silent：取消 Vue 所有的日志与警告
- 取消devtools配置，devtools：是否允许 vue-devtools 检查代码
- ignoredElements 3.0更换为 compilerOptions.isCustomElement
- 取消keyCodes配置，keyCodes：方法修饰器
- 取消productionTip配置 阻止生产环境下的提示

##  应用api
```
const app = createApp({})
app.xxx // 应用api
```

### 与2.0全局api 相同方法
- component: 注册或检索全局组件。
- directive: 注册或检索全局指令。
- use: 安装 Vue.js 插件。
- version:  Vue 的版本号。
- mixin: 全局混入

###    不同
####    mount
> 渲染根组件。

####    provide ==Why?==
> 设置一个可以注册到所有组件的值，组件内使用inject接受使用

```javascript
import { createApp } from 'vue'

const app = createApp({
  inject: ['user'],
  template: `
    <div>
      {{ user }}
    </div>
  `
})

app.provide('user', 'administrator')
```

####    unmount
> 卸载应用实例的根组件。

##  全局api

###    与2.0全局api 相同方法
- nextTick
- version

###    不同
####    createApp
> 返回一个上下文实例，创建一个Vue应用实例

```javascript
const app = createApp({
  data() {
    return {
      ...
    }
  },
  methods: {...},
  computed: {...}
  ...
})
```

####    h
> 返回一个虚拟节点

```
render() {
  return h('h1', {}, 'Some title')
}
```

####    defineComponent
####    defineAsyncComponent
####    resolveComponent
####    resolveDynamicComponent
####    resolveDiective
####    withDirectives
####    createReenderer
####    defineCustomElement（原生里面有自定义元素）
> 创建一个自定义元素，可用于任意框架或非框架

```javascript
import { defineCustomElement } from 'vue'
const MyVueElement = defineCustomElement({
  // 这里是普通的 Vue 组件选项
  props: {},
  emits: {},
  template: `...`,
  // 只用于 defineCustomElement：注入到 shadow root 中的 CSS
  styles: [`/* inlined css */`]
})
// 注册该自定义元素。
// 注册过后，页面上所有的 `<my-vue-element>` 标记会被升级。
customElements.define('my-vue-element', MyVueElement)
// 你也可以用编程的方式初始化这个元素：
// (在注册之后才可以这样做)
document.body.appendChild(
  new MyVueElement({
    // 初始化的 prop (可选)
  })
)
```

```
// 原生写法
class WordCount extends HTMLParagraphElement {
  constructor() {
    // 必须首先调用 super 方法
    super();

    // 元素的功能代码写在这里

    ...
  }
}

customElements.define('word-count', WordCount, { extends: 'p' });
```
####    mergeProps
> 用于对象合并，生成一个新的对象，**不会改变传入的对象**

####    useCssModule
> 允许在setup单文件组件中访问css模块

```html
<script>
import { h, useCssModule } from 'vue'
export default {
  setup() {
    const style = useCssModule()
    return () =>
      h(
        'div',
        {
          class: style.success
        },
        'Task complete!'
      )
  }
}
</script>
<style module>
.success {
  color: #090;
}
</style>
```
### 与2.0差别
- 去除set,delete,observable（使一个对象变为响应式）,compile（将一个模板字符串编译成render函数）等方法
- 去除过滤器功能filter
- 去除extend（创建Vue子类）方法

##  实例选项
### 数据
- data,props,computed,methods,watch 与2.0相同
- 没有propsData属性

####    emits
> 注册自定义事件

```
const app = createApp({})

// 数组语法
app.component('todo-item', {
  emits: ['check'],
  created() {
    this.$emit('check')
  }
})

// 对象语法
app.component('reply-form', {
  emits: {
    // 没有验证函数
    click: null,

    // 带有验证函数
    submit: payload => {
      if (payload.email && payload.password) {
        return true
      } else {
        console.warn(`Invalid submit event payload!`)
        return false
      }
    }
  }
})
```

####    expose
> 暴露在公共实例上的propperty列表

```javascript
export default {
  // increment 将被暴露，
  // 但 count 只能被内部访问
  expose: ['increment'],

  data() {
    return {
      count: 0
    }
  },

  methods: {
    increment() {
      this.count++
    }
  }
}
```

### Dom
-   template,render
-   去掉el,3.0使用mount绑定
-   去掉renderError

### 生命周期 
- 基本与2.0相同

####    errorCaptured
> 在捕获一个来自后代组件的错误时被调用。

### 资源
- directives和components
- 去掉过滤器

### 组合
- 保留混入minxins
- 保留extends
- 保留provide / inject
- 去除parent

####    provide / inject
> 向下注入依赖，需要配合使用

- 无论多深层次都可以使用
- 绑定非响应式 但是如果传入响应式对象，子孙也会是响应式

####    ==++step++==
> 组件内组合api起点

- 调用时间：在创建组件实例时，在初始 prop 解析之后立即调用 setup。在生命周期方面，它是在 beforeCreate 钩子之前调用的。
- 一切都要return出来
- 两个参数 props,context
- context 对象包括 attrs,alots,emit,expose
```html
<template>
  <div>{{ count }} {{ object.foo }}</div>
</template>

<script>
  import { ref, reactive } from 'vue'

  export default {
    setup() {
      const count = ref(0)
      const object = reactive({ foo: 'bar' })

      // 暴露到template中
      return {
        count,
        object
      }
    }
  }
</script>
```

### 其他
- name，inheritAttrs，delimiters（3.1废弃）保留
- functional（使组件无状态 (没有 data) 和无实例 (没有 this 上下文)。）去掉
- model去掉
- comments（当设为 true 时，将会保留且渲染模板中的 HTML 注释。）去掉

####    compilerOptions
> 与全局compilerOptions配置相同

##  实例
### 实例属性
- $children，$scopedSlots，$listeners，$isServer 去除，其他保留

### 实例方法
- $set,$delete,$on,$off,$once,$mount,$destroy 去除 其他保留

##  指令
-   全部保留

### v-memo
> 该指令接收一个固定长度的数组作为依赖值进行记忆比对。如果数组中的每个值都和上次渲染的时候相同，则整个该子树的更新会被跳过。

##  属性值
- 保留key,ref,is
- 去掉slot,slot-scope,scope

##  内置组件
- 保留2.0所有

### teleport组件
> 可将内容渲染到指定标签下

- to:指定标签
- disabled:禁用 <teleport> 的功能

```html
<!-- 渲染到body标签下 -->
<teleport to="body">
  <div v-if="modalOpen" class="modal">
    <div>
      I'm a teleported modal! 
      (My parent is "body")
      <button @click="modalOpen = false">
        Close
      </button>
    </div>
  </div>
</teleport>
```

##  响应式api
### 响应式基础API
#### reactive
> 返回响应式对象

```javascript
const obj = reactive({count: 0})
```

#### readonly
> 只读

```javascript
const obj = readinly({count: 0})

obj.count ++ // 改变失败 并警告
```

#### isProxy
> 检查对象是否是由 reactive 或 readonly 创建的 proxy。

#### isReactive
> 检查对象是否是由 reactive 创建的 proxy。

#### isReadonly
> 检查对象是否是由 readonly 创建的 proxy。

#### toRaw
> 返回 reactive 或 readonly 代理的原始对象。

#### markRaw
> 标记一个对象，使其永远不会转换为 proxy。

```javascript
const foo = markRaw({})
console.log(isReactive(reactive(foo))) // false

// 嵌套在其他响应式对象中时也可以使用
const bar = reactive({ foo })
console.log(isReactive(bar.foo)) // false
```

#### shallowReactive
> 创建一个响应式代理，它跟踪其自身 property 的响应性，但不执行嵌套对象的深层响应式转换 (暴露原始值)。

```javascript
const state = shallowReactive({
  foo: 1,
  nested: {
    bar: 2
  }
})

// 改变 state 本身的性质是响应式的
state.foo++
// ...但是不转换嵌套对象
isReactive(state.nested) // false
state.nested.bar++ // 非响应式
```

#### shallowReadonly
> 创建一个 proxy，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换 (暴露原始值)。

```javascript
const state = shallowReadonly({
  foo: 1,
  nested: {
    bar: 2
  }
})

// 改变 state 本身的 property 将失败
state.foo++
// ...但适用于嵌套对象
isReadonly(state.nested) // false
state.nested.bar++ // 适用
```
### Refs
#### ref
> 返回一个响应式且可变的 ref 对象。ref 对象仅有一个 .value property，指向该内部值。

- ref一般用于基础数据类型，reactive用于复杂数据类型

```javascript
const count = ref(0)
console.log(count.value) // 0

count.value++
console.log(count.value) // 1
```

#### unRef
> 如果参数是一个 ref，则返回内部值，否则返回参数本身。

#### toRef
> 可以用来为源响应式对象上的某个 property 新创建一个 ref。然后，ref 可以被传递，它会保持对其源 property 的响应式连接。

```javascript
const state = reactive({
  foo: 1,
  bar: 2
})

const fooRef = toRef(state, 'foo')

fooRef.value++
console.log(state.foo) // 2

state.foo++
console.log(fooRef.value) // 3
```

#### toRefs
> 将响应式对象转换为普通对象，其中结果对象的每个 property 都是指向原始对象相应 property 的 ref。**不丢失响应性**

- 如果要为特定的 property 创建 ref，则应当使用 toRef

```javascript
function useFeatureX() {
  const state = reactive({
    foo: 1,
    bar: 2
  })

  // 操作 state 的逻辑

  // 返回时转换为ref
  return toRefs(state)
}

export default {
  setup() {
    // 可以在不失去响应性的情况下解构
    const { foo, bar } = useFeatureX()

    return {
      foo,
      bar
    }
  }
}
```

#### isRef
> 检查值是否为一个 ref 对象。

#### customRef
> 创建一个自定义ref,并对其依赖项跟踪和更新触发进行显式控制。

- 它需要一个工厂函数
- 函数接受track(用于追踪)和trigger（用于更新）

```javascript
function useDebouncedRef(value, delay = 200) {
  let timeout
  return customRef((track, trigger) => {
    return {
      get() {
        track() // 用于追踪
        return value
      },
      set(newValue) {
        clearTimeout(timeout)
        timeout = setTimeout(() => {
          value = newValue
          trigger() // 用于更新，实时更新
        }, delay)
      }
    }
  })
}

export default {
  setup() {
    return {
      text: useDebouncedRef('hello')
    }
  }
}
```
#### shallowRef==
> 创建一个跟踪自身 .value 变化的 ref，但不会使其值也变成响应式的。

#### triggerRef==
> 手动执行与 shallowRef 关联的任何作用 (effect)。

### watch和computed

#### computed
> 适合在step()中使用的计算属性

- 接受一个 getter 函数，并根据 getter 的返回值返回一个不可变的响应式 ref 对象。

```javascript
const count = ref(1)
const plusOne = computed(() => count.value + 1)

console.log(plusOne.value) // 2

plusOne.value++ // 错误
```

- 接受一个具有 get 和 set 函数的对象，用来创建可写的 ref 对象。

```javascript
const count = ref(1)
const plusOne = computed({
  get: () => count.value + 1,
  set: val => {
    count.value = val - 1
  }
})

plusOne.value = 1
console.log(count.value) // 0
```

#### watchEffect
> 立即执行传入的函数，当依赖项发生变化时重新运行
#### watchPostEffect
#### watchSyncEffect

#### watch
> 适合在step()中使用的监听属性

- 监听ref（单个）

```javascript
const a = ref(0)  
watch(a, (newVal, oldVal) => {
  console.log(newVal, oldVal)
})
```

- 监听reactive（单个）

```javascript
const data = reactive({
  count: 0
})

watch(data.count, (newVal, oldVal) => { // 错误写法
  console.log(newVal, oldVal)
})

watch(() => data.count, (newVal, oldVal) => { // 正确写法
  console.log(newVal, oldVal)
})
```

- 监听多个，watch 第一个参数可以使用数组的形式

```javascript
watch([a, () => data.count], (newVal, oldVal) => {
  console.log(newVal, oldVal)
})
```

- 与watchEffect区别 
  - watch首次监听不会触发函数，watchEffect首次加载会触发传入的函数
  - watch可以拿到新值和旧值，watchEffect只能得到新值
  - watch可以监听多个值，需要指定监听的值
  - watchEffect不用指定监听数据

### Effect作用域api（Effect 作用域是一个高阶的 API，主要服务于库作者。）
####    effectScope
####    getCurrentScope
####    onScopeDispose

##  组合式API
### step
- 所用数据需暴露出来给模板

```javascript
setup() {
    const readersNumber = ref(0)
    const book = reactive({ title: 'Vue 3 Guide' })

    // 暴露给模板
    return {
        readersNumber,
        book
    }
}
```

- 如果在step()中使用渲染函数(ref需显式使用)

```javascript
setup() {
    const readersNumber = ref(0)
    const book = reactive({ title: 'Vue 3 Guide' })
    // 请注意，我们需要在这里显式地使用 ref 值
    return () => h('div', [readersNumber.value, book.title])
}
```

- 如果返回渲染函数 ，则不能返回其他属性 只能通过expose暴露出来

```javascript
// MyBook.vue

import { h } from 'vue'

export default {
  setup(props, { expose }) {
    const reset = () => {
      // 某些重置逻辑
    }

    // expose 只能被调用一次。
    // 如果需要暴露多个 property，则它们
    // 必须全部包含在传递给 expose 的对象中。
    expose({
      reset
    })

    return () => h('div')
  }
}
```

### 生命周期在step()中的使用
- beforeCreate -> 使用 setup()
- created -> 使用 setup()

```javascript
import { onMounted, onUpdated, onUnmounted } from 'vue'

const MyComponent = {
  setup() {
    onMounted(() => {
      console.log('mounted!')
    })
    onUpdated(() => {
      console.log('updated!')
    })
    onUnmounted(() => {
      console.log('unmounted!')
    })
  }
}
```

### ==getCurrentInstance==

##  组件规范
- 由<template>、<script>、<style> 以及可选的附加自定义块组成

```html
<template>
  <div class="example">{{ msg }}</div>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>

<style>
.example {
  color: red;
}
</style>

<custom1>
  这里可以是，例如：组件的文档
</custom1>
```

### <script setup>
> 该脚本会被预处理并作为组件的 setup() 函数使用

- 顶层的绑定 (包括变量，函数声明，以及 import 引入的内容) 都能在模板中直接使用

```
<script setup>
// 变量
const msg = 'Hello!'

// 函数
function log() {
  console.log(msg)
}
</script>

<template>
  <div @click="log">{{ msg }}</div>
</template>
```
####    await
> <script setup> 中可以使用顶层 await。结果代码会被编译成 async setup()

```
<script setup>
const post = await fetch(`/api/post/1`).then(r => r.json())
</script>
```

####    defineProps 和 defineEmits
> 在 <script setup> 中必须使用 defineProps 和 defineEmits API 来声明 props 和 emits 

```
<script setup>
const props = defineProps({
  foo: String
})

const emit = defineEmits(['change', 'delete'])
// setup code
</script>
```

####    defineExpose

```
<script setup>
import { ref } from 'vue'

const a = 1
const b = ref(2)

defineExpose({
  a,
  b
})
</script>
```

####    useSlots 和 useAttrs
> 在 <script setup> 使用 slots 和 attrs 的情况应该是很罕见的，因为可以在模板中通过 $slots 和 $attrs 来访问它们。

```
<script setup>
import { useSlots, useAttrs } from 'vue'

const slots = useSlots()
const attrs = useAttrs()
</script>
```

### 自定义块

### lang预处理
```
<script lang="ts">
  // 使用 TypeScript
</script>
<template lang="pug">
p {{ msg }}
</template>

<style lang="scss">
  $primary-color: #333;
  body {
    color: $primary-color;
  }
</style>
```

### 可以使用src拆分
```
<template src="./template.html"></template>
<style src="./style.css"></style>
<script src="./script.js"></script>
```

### <style>

####    全局选择器
> 如果想让一个样式应用到全局 使用:global

```
<style scoped>
:global(.red) {
  color: red;
}
</style>
```

####    <style module>
- 可以将css类作为$style对象的键暴露给组件

```
<template>
  <p :class="$style.red">
    This should be red
  </p>
</template>

<style module>
.red {
  color: red;
}
</style>
```
- module attribute 一个值来自定义注入的类对象的 property 键

```
<template>
  <p :class="classes.red">red</p>
</template>

<style module="classes">
.red {
  color: red;
}
</style>
```
- 可以与组合api一起使用
```
// 默认, 返回 <style module> 中的类
useCssModule()

// 命名, 返回 <style module="classes"> 中的类
useCssModule('classes')
```

####    状态驱动的动态 CSS
> 单文件组件的 <style> 标签可以通过 v-bind 这一 CSS 函数将 CSS 的值关联到动态的组件状态上

```html
<template>
  <div class="text">hello</div>
</template>

<script>
export default {
  data() {
    return {
      color: 'red'
    }
  }
}
</script>

<style>
.text {
  color: v-bind(color);
}
</style>
```