---
title: vue Router
tags: 
    - vue
    - vueRouter
categories: 
    - vue
    - vueRouter
---

#####   关于组件

```
全局组件
	Vue.component('my-component',{
		// 反引号可以让内容回车
  		template:`<div>全局组件</div>`
	})
关于子组件
	Vue.component('my-component',{
  		template:`<div>全局组件<my-component></my-component><child></child></div>`，
  		// 注册子组件(子组件只能运用于所注册的组件里)
  		components:{
  			'child':'<h3>这是子组件</h3>'
		}
	})
```

#####   Js操作

```
1.创建组件（组件中data必须是函数）
	const me={
  		template:'<div>组件</div>'
	}
2.约定路由规则，指定组件
	const routes=[
      {
          // 路由路径规则
          path:'/me',
          // 指定组件
          component:me
      },
      {
      	//默认路由
  		  path:'/*'	,
  		  component:me
	  }
	]
3.创建路由实例
	const router=new VueRouter({
  		routes:routes;
  		//名字相同routes:routes=routes
	})
4.挂载路由
	new Vue({
  		el:'#app',
  		router:router 
	})
```

#####   html操作

```
1.设置路由链接
	router-link(使用to属性)
		<router-link to='/index'></router-link>
2.设置路由显示区域
	router-view
		<router-view></router-view>
```

#####   路由传参

```
约定路由规则时设置参数
	const routes=[
      {
          // 路由路径规则
          path:'/me:id',
          // 指定组件
          component:**
      }
	]
路由参数获取
	this.$route.params.id
html中
	<router-link to='/index/45'></router-link>
路由参数监听
	$route: function(){...}
```
####    Api
```
https://router.vuejs.org/zh/api/#to
```
##### router-link
```
<router-link >属性
to      表示目标路由的链接。
replace     设置 replace 属性的话，当点击时，会调用 router.replace() 而不是 router.push()，于是导航后不会留下 history 记录。(跳转方式)
append      设置 append 属性后，则在当前 (相对) 路径前添加基路径
tag     有时候想要 <router-link> 渲染成某种标签，例如 <li>。默认值: "a"
active-class    设置 链接激活时使用的 CSS 类名。
exact    "是否激活" 默认类名的依据是 inclusive match (全包含匹配)。如果当前的路径是 /a 开头的，那么 <router-link to="/a"> 也会被设置 CSS 类名。
event       声明可以用来触发导航的事件。默认值: 'click'
exact-active-class      ??配置当链接被精确匹配的时候应该激活的 class。
```
##### router-view
```
<router-view>属性
name    如果 <router-view>设置了名称，则会渲染对应的路由配置中 components 下的相应组件
```
##### router构建选项
```
router构建选项
routes      path,component,name,components,redirect,props,alias,children,beforeEnter,meta,caseSensitive,pathToRegexpOptions
mode    配置路由模式 可选值: "hash" | "history" | "abstract"
base    应用的基路径。例如，如果整个单页应用服务在 /app/ 下，然后 base 就应该设为 "/app/"。
linkActiveClass     全局配置 <router-link> 的默认“激活 class 类名”。
linkExactActiveClass    全局配置 <router-link> 精确激活的默认的 class。
scrollBehavior      方法接收  to 和 from 路由对象。第三个参数 savedPosition当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。
fallback    当浏览器不支持 history.pushState 控制路由是否应该回退到 hash 模式。
```
##### Router 实例
- Router 实例属性
```
router.app  配置了 router 的 Vue 根实例
router.mode     路由使用的模式。
router.currentRoute     当前路由对应的路由信息对象。
```
- Router 实例方法
```
Router 实例方法
(to,from)
    to: Route: 即将要进入的目标 路由对象
    from: Route: 当前导航正要离开的路由
    next: Function: 一定要调用该方法来 resolve 这个钩子。
router.beforeEach(to,from.next)     注册一个全局前置守卫
router.beforeResolve(to,from.next)   注册一个全局守卫
router.afterEach(to,from)    注册全局后置钩子
增加全局的导航守卫
    区别：当一个导航触发时，全局前置守卫按照创建顺序调用。
         在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用。
         然而和守卫不同的是，这些钩子不会接受 next 函数也不会改变导航本身
router.push
router.replace
router.go
router.back
router.forward
动态的导航到一个新 URL。
router.getMatchedComponents
？？？解析目标位置 
router.addRoutes
动态添加更多的路由规则。参数必须是一个符合 routes 选项要求的数组。
？？？router.onReady
该方法把一个回调排队，在路由完成初始导航时调用，这意味着它可以解析所有的异步进入钩子和路由初始化相关联的异步组件。
router.onError
注册一个回调，该回调会在路由导航过程中出错时被调用。
```
##### $route路由对象
- $route路由对象属性
```
路由对象属性
.path       字符串，对应当前路由的路径，总是解析为绝对路径，如 "/foo/bar"。
.params     一个 key/value 对象，包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象。(id)
.query      一个 key/value 对象，表示 URL 查询参数。
.hash       当前路由的 hash 值 (带 #) ，如果没有 hash 值，则为空字符串。
.fullPath       完成解析后的 URL，包含查询参数和 hash 的完整路径。
.matched        一个数组，包含当前路由的所有嵌套路径片段的路由记录 。    
.name   当前路由的名称，如果有的话。
.redirectedFrom     如果存在重定向，即为重定向来源的路由的名字。
```
- 路由监听
```
watch属性
$route:  监听路由
```
