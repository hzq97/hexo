---
title: Vue插件
tags: 'vue'
categories: 'vue'
---

#####  介绍
```
插件就是给Vue添加全局功能
```
#####  install
- vue插件应该暴露一个install方法。此方法调用时，将Vue构造函数作为第一个参数传入，以及将一个可选的选项作为第二个参数传入
```
Plugin.install = function(Vue, options) {
    // 1. 添加全局方法或属性
  Vue.myGlobalMethod = function () {
    // 一些逻辑……
  }

  // 2. 添加一个全局资源(asset)
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 一些逻辑……
    }
    ...
  })

  // 3. 注入一些组件选项
  Vue.mixin({
    created: function () {
      // 一些逻辑……
    }
    ...
  })

  // 4. 添加一个实例方法
  Vue.prototype.$myMethod = function (methodOptions) {
    // 一些逻辑……
  }
}
```
#####  Vue.use()
- Vue.use() 使用插件
> Vue.use(MyPlugin);  
> 可以根据情况，传入一些可选的选项：  
> Vue.use(MyPlugin, { someOption: true })

通俗解释  就是自己写一些方法，属性，修饰符，自定义指令，混入等 在Vue中