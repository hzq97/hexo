---
title: Vue mixin 混入
tags: 'vue'
categories: 'vue'
---

### 解释
```
官方解释：混入 (mixins) 是一种分发 Vue 组件中可复用功能的非常灵活的方式。
混入对象可以包含任意组件选项。
当组件使用混入对象时，所有混入对象的选项将被混入该组件本身的选项。
```
### 例子
```
// 定义一个混入对象
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}


// 定义一个使用混入对象的组件 
var Component = Vue.extend({
  mixins: [myMixin]
})

var component = new Component() // => "hello from mixin!"
```
```
var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})
```
###   全局混入 Vue.mixin
```
Vue.mixin({
  created: function () {
    console.log('123465')
  }
})
```
###    选项合并策略
```
当具有相同data数据或者方法时 进行数据合并 
```
##### 当数据冲突时
>  以组件data优先

```
var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})
```
##### 当钩子函数冲突时
> 两者将合并为一个数组，将都被调用。

```
var mixin = {
  created: function () {
    console.log('混入对象的钩子被调用')
  }
}

new Vue({
  mixins: [mixin],
  created: function () {
    console.log('组件钩子被调用')
  }
})

// => "混入对象的钩子被调用"
// => "组件钩子被调用"
```
#####   当methods、components 和 directives 下键值对（方法）冲突时
> 取组件对象的键值对 

```
var mixin = {
  methods: {
    foo: function () {
      console.log('foo')
    },
    conflicting: function () {
      console.log('from mixin')
    }
  }
}

var vm = new Vue({
  mixins: [mixin],
  methods: {
    bar: function () {
      console.log('bar')
    },
    conflicting: function () {
      console.log('from self')
    }
  }
})

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => "from self"
```
### 自定义合并策略
- Vue.config.optionMergeStrategies
```
Vue.config.optionMergeStrategies.myOption = function (toVal, fromVal) {
  // 返回合并后的值
}
```