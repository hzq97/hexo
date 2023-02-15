---
title: vue Router 钩子函数
tags: 
    - vue
    - vueRouter
categories: 
    - vue
    - vueRouter
---

##  全局路由钩子

  ```
    const router = new VueRouter({ ... })

    router.beforeEach((to, from, next) => {
      // ...
    })
    router.afterEach((to, from) => {
      // ...
    })
  ```
  
#### 1.beforeEach
> 全局前置守卫

@ agrement
- to[Route]: 即将要进入的路由
- from[Route]: 当前正要离开的路由
- next[function]: 调用该方法来resolve这个钩子。
    1. next(): 执行下一个钩子
    2. next(false): 中断跳转
    3. next('/') && next({path: '/'}): 跳转其他路由
    4. next(error): 如果传入error实例，则终止然后router.onError()

#### 2.beforeResolve
> 全局解析守卫
- 在导航被确认之前，在所有组件内守卫和异步路由组件被解析之后

#### 3.afterEach
> 全局后置钩子
- 这些钩子不会接受 next 函数也不会改变导航本身

##  路由独享守卫
```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```
##  组件内的守卫
```
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

##  导航路由解析流程
```
1.导航被触发。
2.在失活的组件里调用离开守卫。
3.调用全局的 beforeEach 守卫。
4.在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
5.在路由配置里调用 beforeEnter。
6.解析异步路由组件。
7.在被激活的组件里调用 beforeRouteEnter。
8.调用全局的 beforeResolve 守卫 (2.5+)。
9.导航被确认。
10.调用全局的 afterEach 钩子。
11.触发 DOM 更新。
12.用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数。
```