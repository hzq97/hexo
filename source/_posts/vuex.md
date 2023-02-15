---
title: vuex
tags: 
    - vue
    - vuex
categories: 
    - vue
    - vuex
---

- Vuex 通过 store 选项，提供了一种机制将状态从根组件“注入”到每一个子组件中（需调用 Vue.use(Vuex)）

####    State

> 主状态树

```
const state = {
    count:10,
    todoList: [
        {id: 1;done: true},
        {id: 2;done: false}
    ]
}

<!-- 获取 -->
store.state.count
<!-- 实例获取 -->
this.$store.state.count
```
####    Getter
> 通过 State派生出来的状态

```
const getter = {
    // 获取state里todoList长度
    gettoListLength: state=> {
        return state.todoList.length
    },
    // 根据id  得到done值
    getBool: (state) => (id) => {
        return state.todos.find(todo => todo.id === id)
    }
}

<!-- 获取 -->
store.getters.gettoListLength
<!-- 实例获取 -->
this.$store.getters.getBool(2)
```
####    Mutation
> 改变store 中状态唯一的方法  提交Mutation

- 必须进行同步操作

```
const mutation = {
    increment (state){
        state.count++
    },
    increments (state, n) {
        state.count += n
    }
}

<!-- 获取 -->
store.commit('increment')
store.commit('increments',10)
<!-- 实例获取 -->
$store.commit('increment')
$store.commit('increments',10)
```
####    Action
> 类似于 Mutation 用于提交Mutation,不能直接更改

- Action 可以包含任意异步操作

```
一般用于异步操作时
actions: {
    increment (context) {
      context.commit('increment')
    }
}

<!-- 调用 -->
store.dispatch('increment')
```
####    Module
> 可以将store 分割成多个模块，每个模块都拥有自己的 state,getter,mutation,action

- 可以根据状态的作用写成多个文件
```
const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

```