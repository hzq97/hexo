---
title: vue过滤器 filter
tags: 'vue'
categories: 'vue'
---
### 过滤器
#####   使用
```
<!-- 在双花括号中 -->
{{ message | capitalize }}

<!-- 带参过滤器 -->
{{ message | filterA('arg1', arg2) }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
```
#####   自定义过滤器
- 实例注册
```
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```
- 全局注册
```
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})

new Vue({
  // ...
})
```
#####   内置过滤器（2.0版本已移除）
```
capitalize  首字母大写
uppercase  全部大写
currency  输出金钱以及小数点  参数1：货币符号（默认$）  参数2： 小数位数(默认2)
pluralize  如果只有一个参数，复数形式末尾加s  如果多个  参数被当作一个字符串数组
...
```