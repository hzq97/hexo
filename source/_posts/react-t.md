---
title: react 面试题
tags: 
    - react
    - 面试题
categories: 
    - react
    - 面试题
---

### 基本
#####   react 工作原理
#####   区分realv dom(真实dom)和virtual dom(虚拟dom)

realv dom(真实dom) | virtual dom(虚拟dom)
---|---
更新缓慢 | 更新快
可以直接更新HTML | 无法直接更新HTML
如果元素更新，则创建新DOM | 如果元素更新，则更新JSX
DOM操作代价很高 | DOM操作简单
消耗内存较多 | 很少的内存消耗

#####   什么是react?
```
1.2011 facebook 前端javascript库
2.遵循基于组件的方法，有助于构建可重复的UI组件
3.2015年开源
4.用于开发复杂和交互式的Web合移动UI
```
#####   react有什么特点？
```
1.使用虚拟DOM而不是真正的DOM
2.可以进行服务器渲染
3.单向数据流绑定
```
#####   react一些主要优点？
```
1.提高性能
2.方便在客户端和服务端使用
3.代码可读性很好
4.很容易与Meteor,Angular等其他框架集成
5.编写UI测试变得非常容易
```
#####   react有哪些限制
```
1.React只是一个库，而不是一个完整的框架
2.他的库非常庞大，需要时间来理解
3.新手上手难  很难理解
4.编码变得复杂，因为它使用内联模块和 JSX
```
#####   什么是JSX
```
JSX是JavaScript XML 的简写。是React使用的一种文件，它利用JavaScript的表现力和类似HTML的模板语法。这使得HTML文件非常容易理解。此文件能使应用非常可靠，并能够提高其性能
```
#####   你了解Virtual DOM吗？解释一下它的工作原理。
```

Virtual DOM是一个轻量级的JavaScript 对象，它最初只是realDOM的副本。它是一个节点树。它将元素、它们的属性和内容作为对象及其属性。React的渲染函数从React组件中创建一个节点树。然后响应数据模型中的变化来更新该树，该变化是由用户或系统完成的各种动作引起的。

1.每当底层数据发生变化  整个UI将重新渲染 Virtual DOM
2.与real DOM进行对比 查找差异
3.渲染real DOM中的差异部分
```
#####   为什么浏览器无法读取JSX
```
浏览器只能读取JavaScript  只能把JSX编译成JavaScript对象
```
### React组件
#####   你怎么理解在React 中，一切都是组件
```
组件是React应用UI的构建模块。这些组件将整个UI分成晓得独立的并可重复的部分。每个组件彼此独立，不会影响UI的其余部分
```
#####   怎样解释 React 中render()的目的
```
每个React组件强制要求必须有一个render().它返回一个 React 元素，是原生 DOM 组件的表示。
必须每次调用时都返回相同的结果。
```
#####   展示组件和容器组件之间有何不同
```
展示组件通常只关心ui部分
容器组件一般存储数据  以供展示组件所使用  一般是其他组件的数据源
```
#####   类组件和函数式组件之间有何不同
```
一般类组件没有状态  一般用于无状态组件 只接受props
```
#####   区分有状态组件和无状态组件
```
1.无状态组件没有state,只是通过props然后渲染
2.无状态组价可复用性高
```
#####   React组件的生命周期
```
1.初始渲染阶段
2.更新阶段  只有在 prop 或状态发生变化时才可能更新和重新渲染
3.卸载阶段
```
#####   React 组件的生命周期方法
```
1.componentWillMount() 渲染之前执行
2.componentDidMount()  仅第一次渲染之后执行
3.componentWilUpdate()  DOM渲染之前调用
4.componentDidUpdate()  DOM渲染之后调用
5.componentWillUnmount()  组件卸载之后执行
6.componentWillRececiveProps()  当从父类接收到props并且在调用另一个渲染器之前调用
7.shouldComponentUpdate()  根据特定条件返回 true 或 false。如果你希望更新组件，请返回true 否则返回 false
(就是当props或state发生变化时候   调用shouldComponentUpdate 返回false不重新渲染)
```
#####   你对React的refs有什么了解
> Refs提供了一种允许我们访问DOM节点或在render方法中创建的React元素的方式。

```
Refs是React中引用的简写。它是一个有助于对特定的React元素或组件的引用的属性，它将由组件渲染配置函数返回。

给DOM元素添加ref  返回一个DOM节点
给类组件添加ref  返回当前组件实例（不能用在函数声明的组件上，因为函数声明的组件没有实例）

使用refs的一些情况
1.做一些动画的交互
2.媒体控件的播放
3.获取焦点、获取文本等
```
#####   如何模块化React中的代码
```
export和import
```
#####   受控组件和非受控组件
> 关于表单获取值得问题  受控组件是指受控于state  非受控是指利用refs获取当前值

受控组件 | 非受控组件
---|---
没有维持自己状态 | 保持自己状态
数组由父组件控制 | 数据由DOM控制
通过props获得当前值，然后通过回调通知其更改 | Refs用于获取当前值
#####   什么是高阶组件（HOC）
> 高阶组件是参数为组件，返回值为新组件的函数。

```
组件 可以接受组件
例如高阶函数
函数可以接受函数参数
```
```
const List = ({ data }) => (
  <ul>
    {data.map(item => <li key={item.name}>{item.name}</li>)}
  </ul>
)

const withLoading = BaseComponent => ({ isLoading, ...otherProps }) => (
  isLoading ?
    <div>我正在加载...</div> :
    <BaseComponent {...otherProps} />
)

const LoadingList = withLoading(List)
```
#####   什么是纯组件？
```
纯（Pure） 组件是可以编写的最简单、最快的组件。它们可以替换任何只有 render() 的组件。这些组件增强了代码的简单性和应用的性能。
```
#####   React中key的作用
```
key 用于识别唯一的 Virtual DOM 元素及其驱动 UI 的相应数据。它们通过回收 DOM 中当前所有的元素来帮助 React 优化渲染。这些 key 必须是唯一的数字或字符串，React 只是重新排序元素而不是重新渲染它们。这可以提高应用程序的性能。
```
#####   为什么建议传递给 setState 的参数是一个 callback 而不是一个对象
```
因为 this.props 和 this.state 的更新可能是异步的，不能依赖它们的值去计算下一个 state。
```
#####   (在构造函数中)调用 super(props) 的目的是什么
> 为了可以在子类中使用this.props

#####   state资料
```
1）通过this.setState来修改state中的数据；

2）this.setState是异步的；

3）其中有两个参数，第一个参数是一个对象或者是一个函数（必须返回一个对象），函数中的第一个值为（prevState），第二个参数是（prevProps）

4）为什么是异步的，一位state可以批量执行，也就是说当多个setState一起同时执行时会被合并，提高DOM的渲染效率；

5）this.setState什么时候是同步的？原生js绑定的事件，setTimeout/setInterval等，（就是不受react机制控制时）

6）this.setState本身其实是一个同步的，异步不是因为本身的运行机制或者代码，而是因为他所在的合成事件和钩子函数的调用顺序在更新之前，导致函数内没法立即拿到更新后的值，形成了所谓的异步，可以通过第二个参数中的callback拿到更新后的结果；
```
### Redux
#####   MVC框架主要问题是什么？
```
1.对DOM操作的代价特别高
2.程序运行缓慢且效率低下
3.内存浪费严重
4.由于循环依赖性，组件模型需要围绕models和views进行创建
```
#####   什么是redux
```
它是JavaScript程序的可预测状态容器，用于整个应用的状态管理。
使用Redux开发的应用易于测试，可以在不同环境中运行，并显示一致的行为
```
#####   Redux遵循的三个原则是什么？
```
1.单一事实来源：整个应用的状态存储在store中的对象/状态树里
2.状态是只读的：改变状态的唯一方法是触发一个动作 action
3.使用纯函数进行更改 reducer
```
#####   你对“单一事实来源”有什么理解
```
Redux 使用 “Store” 将程序的整个状态存储在同一个地方。因此所有组件的状态都存储在 Store 中，并且它们从 Store 本身接收更新。单一状态树可以更容易地跟踪随时间的变化，并调试或检查程序。
```
#####   Redux组成
```
1.action 一个用来描述发生什么事情的对象
2.Reducer 确定状态如何变化的地方
3.Store 整个程序的状态/对象树保存在Store
4.view 只显示Store提供的数据
```
#####   如何在Redux中定义Action？
```
Action必须有type属性
function addList(text){
    return {
        type: "ADD_TYPE",
        data: text
    }
}
```
#####   解释Reducer的作用
```
Reducer 是纯函数，它规定应用程序的状态怎样回应action  
返回一个新的状态
```
#####   Store在Redux中的意义
```
Store是一个JavaScript对象，他可以保存程序状态，并提供一些方法来访问状态，调度操作和侦听检测器。
应用程序的整个状态/对象树保存在单一存储中。因此，Redux 非常简单且是可预测的。
我们可以将中间件传递到 store 来处理数据，并记录改变存储状态的各种操作。所有操作都通过 reducer 返回一个新状态。
```
#####   Redux有哪些优点
```
1.结果的可预测性：因为只存在一个store,所以不存在如何将当前状态与动作和应用其他部分同步的问题
2.可维护性：代码变得更容易维护，具有可预测的结果和严格的结构
3.服务器渲染：只需将服务器上创建的store传到客户端即可。这对初始渲染非常有用，并且可以优化应用性能，从而提供更好的用户体验
4.开发人员工具：从操作到状态更改，开发人员可以实时跟踪应用中发生的所有事情
5.社区和生态系统：
6.易于测试：
7.组织：有明确的组织方式  使得团队使用时更加一致和简单
```
#####   Redux Thunk 的作用是什么
```
Redux thunk 是一个允许你编写返回一个函数而不是一个 action 的 actions creators 的中间件。如果满足某个条件，thunk 则可以用来延迟 action 的派发(dispatch)，这可以处理异步 action 的派发(dispatch)。

export function addCount() {
  return {type: ADD_COUNT}
} 
export function addCountAsync() {
  return dispatch => {
    setTimeout( () => {
      dispatch(addCount())
    },2000)
  }
}

function createThunkMiddleware(extraArgument) {
  return function({ dispatch, getState }) {
    return function(next){
      return function(action){
        if (typeof action === 'function') {
          return action(dispatch, getState, extraArgument);
        }
        return next(action);
      };
    }
  }
}
在执行行文之前 执行中间件  如果中间件返回函数   重新判定行文  否则next()执行行文
```
#####   react与redux
```
Provider 组件让store下发到所有容器组件
<Provider store={store}>
    <App />
</Provider>
<!-- connect()让组件可以 -->
const mapStateToProps = state => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}

const mapDispatchToProps = dispatch => {
  return {
    onTodoClick: id => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)
```
### React路由
#####   什么是React路由
#####   为什么React Router v4 中使用switch关键字？
```
虽然<div>用于封装React中多个路由，当你想要仅显示要在多个定义的路线中呈现的单个路线时，可以使用“switch”。
只会匹配正确的匹配项，然后进行渲染
```
#####   为什么需要路由？
```
y因为我们需要多个视图 展示页面  而创建的每个路由都会提供一个独特的视图
```
#####   React优点 ???
```
1.可以将 Router 可视化为单个根组件（<BrowserRouter>），其中我们将特定的子路由（<route>）包起来。
2.无需手动设置历史值
3.包是分开的：共有三个包，分别用于 Web、Native 和 Core。这使我们应用更加紧凑。基于类似的编码风格很容易进行切换。
```
#####   React Router 和普通路由有何不同
```
1.一个是新的文件   一个只是一个页面
2.发送请求接收html     仅更改历史属性记录
3.用户每个视图都在不同页面切换（每次整个页面刷新）    用户认为在（局部刷新）
```

### 其他
#####   Fragments
- 无包裹
#####   Context
- 跨组件传值
#####   lazy  Suspense
- 延迟加载组件
#####   错误边界

#####   Portals
- 将子节点渲染到存在于父组件以外的 DOM 节点
#####   Profiler
#####   PropsTypes