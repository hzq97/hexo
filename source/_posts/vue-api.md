---
title: Vue api
tags: 'vue'
categories: 'vue'
---

##### 全局配置

    Vue.config  是一个对象，包含 Vue 的全局配置。可以在启动应用之前修改下列属性：
    1.keyCodes    自定义按键
    2.optionMergeStrategies
    3.silent    禁止所有的日志与警告
    4.devtools  是否允许vue-devtools审查应用程序
    5.errorHandler  设置函数  用于在组件 render 函数调用和 watcher 期间捕获错误（具体捕获错误查看详情）
    6.warnHandler  为运行时(runtime)下的 Vue 警告设置一个自定义处理函数

##### 全局api

    vue.component 注册全局组件
    vue.use  安装vue插件  该方法需要在调用 new Vue() 之前被调用。
    Vue.set  向响应式对象中添加一个属性，并确保这个新属性同样是响应式的，且触发视图更新。
    Vue.delete   删除对象的属性。如果对象是响应式的，确保删除能触发更新视图。
    Vue.directive   注册或获取全局指令。v-自定义
                    bind    // 只调用一次，指令第一次绑定到元素时调用。
                    inserted   // 当被绑定的元素插入到 DOM 中时……
                    update      //
                    componentUpdated
                    unbind
    Vue.filter   注册或获取全局过滤器。
    Vue.mixin( mixin )   注册或获取混入。影响注册之后所有创建的每个 Vue 实例。插件作者可以使用混入，向组件注入自定义的行为。不推荐在应用代码中使用。
    Vue.compile( template )   ？？？在 render 函数中编译模板字符串。只在独立构建时有效  var res = Vue.compile('<div><span>{{ msg }}</span></div>')
    Vue.version Vue的版本

##### 选项/数据

    data  对象或函数  ，组件只接受函数
    props   父组件传来的数据
    propsData   只用于 new 创建的实例中，创建实例时传递 props，主要作用是方便测试
    computed    计算属性    计算属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。
    methods     各种方法
    watch   一个对象，键是需要观察的表达式，值是对应回调函数。遍历 watch 对象的每一个属性

##### 选项/DOM

    el  提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。
    template    一个字符串模板作为 Vue 实例的标识使用。模板将会 替换 挂载的元素。挂载元素的内容都将被忽略，除非模板的内容有分发插槽。

##### 生命周期

    beforeCreate    在实例初始化之后
    created     在实例创建完成后被立即调用。
    beforeMount     在挂载开始之前被调用
    mounted     el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子
    beforeDestroy   实例销毁之前调用。
    destroyed   Vue 实例销毁后调用。
    activated   keep-alive 组件激活时调用。
    deactivated     keep-alive 组件停用时调用。
    beforeUpdate    数据更新时调用
    updated     由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
    errCaptured     当捕获一个来自子孙组件的错误时被调用。

##### 选项/组合

    parent      ???
    mixins      mixins 选项接受一个混入对象的数组。
    extends     允许声明扩展另一个组件(可以是一个简单的选项对象或构造函数)，而无需使用 Vue.extend。
    provide/inject    ???

##### 选项/其他

    name    允许组件模板递归地调用自身。注意，组件在全局用 Vue.component() 注册时，全局 ID 自动作为组件的 name。
    delimiters   改变纯文本插入分隔符。默认{{内容}}  delimiters:['${','}']
    functional      ???使组件无状态 (没有 data ) 和无实例 (没有 this 上下文)。
    model       ???允许一个自定义组件在使用 v-model 时定制 prop 和 event。
    inheritAttrs    false默认行为将去掉
    comments    当设为 true 时，将会保留且渲染模板中的 HTML 注释。默认行为是舍弃它们。

##### 实例属性

    .$data  Vue 实例观察的数据对象。
    .$props     当前组件接收到的 props 对象。
    .$el    Vue 实例使用的根 DOM 元素。
    .$options   用于当前 Vue 实例的初始化选项。在实例中的自定义属性
    .$parent    ？？？父实例，如果当前实例有的话。
    .$root      当前组件树的根 Vue 实例。
    .$children      当前实例的直接子组件。
    .$slots     用来访问被插槽分发的内容。
    .$scopedSlots   用来访问作用域插槽。对于包括 默认 slot 在内的每一个插槽，该对象都包含一个返回相应 VNode 的函数。
    .$refs  一个对象，持有注册过 ref 特性 的所有 DOM 元素和组件实例。
    .$isServer  当前 Vue 实例是否运行于服务器。
    .$attrs     包含了父作用域中不作为 prop 被识别 (且获取) 的特性绑定 (class 和 style 除外)。
    .$listeners     包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。

##### 实例方法/数据

    .$watch     deep: true 深度检测  immediate: true 将立即以表达式的当前值触发回调：
    .$set   这是全局 Vue.set 的别名。
    .$delete    这是全局 Vue.delete 的别名。

##### 实例方法/事件

    .$on    监听自定义事件
    .$emit  触发实例上事件
    .$once  监听一个自定义事件，只触发一次
    .$off   移除自定义事件

##### 实例方法/生命周期

    .$mount     使用 vm.$mount() 手动地挂载一个未挂载的实例。
    .$forceUpdate   迫使 Vue 实例重新渲染。
    .$nextTick    将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。
    .$destroy   完全销毁一个实例。

