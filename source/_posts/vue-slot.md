---
title: Vue 插槽
tags: 'vue'
categories: 'vue'
---

##### 例子
```
// 定义组件my-component
<p class="myComponent">
  <slot name="mySlot"></slot>
</p>

<!-- 使用组件 -->
<my-component> 插槽内容 </<my-component>>
```
##### 后备内容
- 在slot组件内设置后备内容,当没有提供任何插槽内容时 渲染后备内容

```
<!-- 在slot组件内设置后备内容 -->
<button type="submit">
  <slot>Submit</slot>
</button>

<!-- 当没有提供任何插槽内容时 渲染后备内容 -->
<submit-button></submit-button>

<!-- 渲染结果 -->
<button type="submit">
  Submit
</button>
```
> 父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

##### 具名插槽
- slot组件提供一个name的属性，可以定义额外插槽，使用v-slot指令携带对应参数设置内容
- 一个不带 name 的 <slot> 出口会带有隐含的名字“default”。
- 具名插槽可以缩写 v-slot 缩写为 # ，v-slot:header 可以被重写为 #header
```
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```
```
<base-layout>
<!-- 对应渲染到<slot name="header"></slot> -->
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

<!-- 对应渲染到<slot name="footer"></slot> -->
  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```
> 注意:v-slot 只能添加在 <template> 上

##### 作用域插槽
-  为了让 user 在父级的插槽内容中可用
```
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>

<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```

##### 解构插槽Prop
- 解构赋值 可以重命名或者设置默认值等
```
<!-- 将user重命名为person -->
<current-user v-slot="{ user: person }">
  {{ person.firstName }}
</current-user>

<!-- 设置一个默认的firstname -->
<current-user v-slot="{ user = { firstName: 'Guest' } }">
  {{ user.firstName }}
</current-user>
```
##### 动态插槽名