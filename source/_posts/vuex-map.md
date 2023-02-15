---
title: vuex辅助函数
tags: 
    - vue
    - vuex
categories: 
    - vue
    - vuex
---

####    mapState
> 当一个组件需要获取多个状态时候，将这些状态都声明为计算属性会有些重复和冗余。

```
原本
computed: {
    count () {
      return this.$store.state.count
    },
    count1 () {
      return this.$store.state.count
    },
    count2 () {
      return this.$store.state.count
    },
    count3 () {
      return this.$store.state.count
    }
}
现在
computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,
    count1: state => state.count1,
    count2: state => state.count2,
    count3: state => state.count3,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',
    countAlias1: 'count1',
    countAlias2: 'count2',
    countAlias3: 'count3'

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
})

或者
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count','count1','count2','count3'
])
```
####    mapGetters
```
mapGetters({
  // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
})
```
####    mapMutations
```
...mapMutations([
  'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

  // `mapMutations` 也支持载荷：
  'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
]),
...mapMutations({
  add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
})
```
####    mapActions
```
...mapActions([
  'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

  // `mapActions` 也支持载荷：
  'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
]),
...mapActions({
  add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
})
```