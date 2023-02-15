---
title: vue 面试题
tags: 
    - vue
    - 面试题
categories: 
    - vue
    - 面试题
---

### vue-cli 问题

    1.构建vue-cli工程都用到哪些技术，它们作用分别是什么？   *
        vue.js  vue-cli工程的核心，主要特点是 双向数据绑定 和 组件系统。
        vue-router  vue官方推荐使用的路由框架。
        vuex    专为 Vue.js 应用项目开发的状态管理器
        axios   用于发起 GET 、或 POST 等 http请求，基于 Promise 设计。
        vux     一个专为vue设计的移动端UI组件库。
        emit.js     创建一个emit.js文件，用于vue事件机制的管理。
        webpack     模块加载和vue-cli工程打包器。

<!---->

    2.请说出vue-cli工程中每个文件夹和文件的用处？   **
        build   用于存放 webpack 相关配置和脚本。
        config  主要存放配置文件，用于区分开发环境、线上环境的不同。 
        dist 文件夹：默认 npm run build 命令打包生成的静态资源文件，用于生产部署。
        node_modules：存放npm命令下载的开发环境和生产环境的依赖包。
        src: 存放项目源码及需要引用的资源文件。
        index.html：设置项目的一些meta头信息和提供<div id="app"></div>用于挂载 vue 节点。
        package.json：用于 node_modules资源部 和 启动、打包项目的 npm 命令管理。
        
        src下assets：存放项目中需要用到的资源文件，css、js、images等。
        src下componets：存放vue开发中一些公共组件：header.vue、footer.vue等。
        src下emit：自己配置的vue集中式事件管理机制。
        src下router：vue-router vue路由的配置文件。
        src下service：自己配置的vue请求后台接口方法。
        src下page：存在vue页面组件的文件夹。
        src下util：存放vue开发过程中一些公共的.js方法。
        src下vuex：存放 vuex 为vue专门开发的状态管理器。
        src下app.vue：使用标签<route-view></router-view>渲染整个工程的.vue组件。
        src下main.js：vue-cli工程的入口文件。

<!---->

    3.config文件夹 下 index.js 的对于工程 开发环境 和 生产环境 的配置？   ***
        //build生产环境
        index：配置打包后入口.html文件的名称以及文件夹名称
        assetsRoot：配置打包后生成的文件名称和路径
        assetsPublicPath：配置 打包后 .html 引用静态资源的路径，一般要设置成 "./"
        productionGzip：是否开发 gzip 压缩，以提升加载速度
        //dev开发环境
        port：设置端口号
        autoOpenBrowser：启动工程时，自动打开浏览器
        proxyTable：vue设置的代理，用以解决 跨域 问题

<!---->

    4.请你详细介绍一些 package.json 里面的配置  **
        scripts：npm run xxx 命令调用node执行的 .js 文件
        dependencies: 生产环境依赖包的名称和版本号，即这些 依赖包 都会打包进 生产环境的JS文件里面
        devDependencies:开发环境依赖包的名称和版本号，即这些 依赖包 只用于 代码开发 的时候，不会打包进 生产环境js文件 里面。

### vue

    1.对于Vue是一套渐进式框架的理解
    没有过多要求，可以当jquery用，做一些页面，也可以用全家桶做整个系统

<!---->

    2.请说出vue几种常用的指令
    v-if
    v-on
    v-show
    v-model
    v-text
    v-html
    v-bind

<!---->

    3.请问 v-if 和 v-show 有什么区别?
    v-if    是否渲染
    v-show  是否隐藏

<!---->

    4.vue常用的修饰符
    v-model的 
    .lazy   不会立即改变，失焦后同步
    .number
    .trim
    v-on的
    .stop 阻止冒泡
    .prevent 阻止默认
    .self
    .once

<!---->

    5.v-on可以监听多个方法吗？
        可以
        v-on="{
            keyup:xxx,
            keydown:xxx
        }"

<!---->

    6.vue中 key 值的作用
    当数据改变，Vue底层通过对比能够更快的获取到更新的内容并显示到页面上。
    总之就是一句话，key属性能够提升性能(主要作用于数据更新时)。

<!---->

    7. vue-cli工程升级vue版本
        修改package.json vue版本  然后npm install

<!---->

    8.vue事件中如何使用event对象？
    参数加$event

<!---->

    9. $nextTick的使用
    dom生成之后调用，可以进行dom操作

<!---->

    10.Vue 组件中 data 为什么必须是函数
    javascipt只有函数构成作用域，data是一个函数时，每个组件实例都有自己的作用域，每个实例相互独立,不会相互影响

<!---->

    11.v-for 与 v-if 的优先级
    v-for优先级高

<!---->

    12.vue中子组件调用父组件的方法
        第一种方法是直接在子组件中通过this.$parent.event来调用父组件的方法this.$parent.event
        第二种方法是在子组件里用$emit向父组件触发一个事件，父组件监听这个事件就行了。
        <!--父组件-->
        <template>
          <div>
            <child @fatherMethod="fatherMethod"></child>
          </div>
        </template>
        <script>
          import child from '~/components/dam/child';
          export default {
            components: {
              child
            },
            methods: {
              fatherMethod() {
                console.log('测试');
              }
            }
          };
        </script>
        
        <!--子组件-->
        <template>
          <div>
            <button @click="childMethod()">点击</button>
          </div>
        </template>
        <script>
          export default {
            methods: {
              childMethod() {
                this.$emit('fatherMethod');
              }
            }
          };
        </script>
        第三种是父组件把方法传入子组件中，在子组件里直接调用这个方法

<!---->

    13.vue中 keep-alive 组件的作用
        可以使被包含的组件保留状态，或避免重新渲染。
        include - 字符串或正则表达，只有匹配的组件会被缓存
        exclude - 字符串或正则表达式，任何匹配的组件都不会被缓存
        <keep-alive include="a">
          <component>
            <!-- name 为 a 的组件将被缓存！ -->
          </component>
        </keep-alive>
        
        路由配置中
        meta:{
            keepAlive:  
            <!-- 是否被缓存 -->
        }

<!---->

    14.vue中编写可复用的组件
    插槽
    prop传参

```
15.vue生命周期有关的试题

```

    16. vue如何监听键盘事件中的按键?
        <!--按键修饰符-->
        <input @keyup.enter="function">
        <!--组合写法-->
        @click.ctrl=”function”   Ctrl + Click
        @keyup.alt.67=”function”    Alt + C

<!---->

    17.vue更新数组时触发视图更新的方法
    vue.set
    vue.delete
    数组的操作方法
        push()
        pop()
        shift()
        unshift()
        splice()  
        sort()
        reverse()

<!---->

    18.vue中对于对象的更改检测

<!---->

    19.解决非工程化项目初始化页面闪动问题
    v-cloak指令和css规则如[v-cloak]{display:none}一起用时，这个指令可以隐藏未编译的Mustache标签直到实例准备完毕。
    v-cloak     这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。

<!---->

    20.vue等单页面应用及其优缺点
    优点：
    Vue 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件，核心是一个响应的数据绑定系统。MVVM、数据驱动、组件化、轻量、简洁、高效、快速、模块友好。
    缺点：
    不支持低版本的浏览器，最低只支持到IE9；不利于SEO的优化（如果要支持SEO，建议通过服务端来进行渲染组件）；第一次加载首页耗时相对长一些；不可以使用浏览器的导航按钮需要自行实现前进、后退。

<!---->

    21.什么是vue的计算属性？
    在一个计算属性里可以完成各种复杂的逻辑，包括运算、函数调用等，只要最终返回一个结果就可以。
    计算属性还可以依赖多个Vue 实例的数据，只要其中任一数据变化，计算属性就会重新执行，视图也会更新。

<!---->

    22.vue-cli提供的几种脚手架模板
    browserify和webpack 

### vue router

    1.vue-router如何响应 路由参数 的变化？
    watch 监听$router
    beforeRouteUpdate (to, from, next) {
    // react to route changes...
    // don't forget to call next()
    }

<!---->

    2.完整的 vue-router 导航解析流程
        1.导航被触发；
        2.在失活的组件里调用beforeRouteLeave守卫；
        3.调用全局beforeEach守卫；
        4.在复用组件里调用beforeRouteUpdate守卫；
        5.调用路由配置里的beforeEnter守卫；
        6.解析异步路由组件；
        7.在被激活的组件里调用beforeRouteEnter守卫；
        8.调用全局beforeResolve守卫；
        9.导航被确认；
        10..调用全局的afterEach钩子；
        11.DOM更新；
        12.用创建好的实例调用beforeRouteEnter守卫中传给next的回调函数。

