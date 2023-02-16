---
title: react-redux
tags: 
    - react
    - redux
categories: 
    - react
---

### react-redux
```
Redux有三个核心的概念：  Store、Action和Reducer。

Redux更新state的逻辑：action -> reducer -> new state
```
#####   学习state
```
var store = createStore(reducer);
```
> 这就是Store了，用来管理state的单一对象。其中有三个方法：
- 提供 getState() 方法获取 state；
- 提供 dispatch(action) 方法更新 state；
- 通过 subscribe(listener) 注册监听器 或者 返回的函数注销监听器;

#####   学习Actions
```
function changeTable(index) {
    return { type: "channgTable", data:index }
}

最终它就是一个对象。包含type、data或者还有其他元素的对象。
```
> action就是用来告诉我们的状态管理器需要做什么样的一种操作。拿以上例子来说，就是为了做一个切换table的操作，那么我就定义了这么一个action。

#####   学习Reducers
```
const reducer = function(state={"tableIndex":0}, action={}) {
    switch(action.type){
        //当发出type为changeTable的action对state的操作
        case "changeTable":
            let backup = state;
            backup["tableIndex"] = action.data;
            return Object.assign({}, state,backup);
        default :
            return Object.assign({}, state);
    }
}
```
> 以上例子就是一个reducer，它是一个会对不同action做出不同操作的函数。如当我发出切换table的action时，就是把我们之前定义action的data传递给state下的tableIndex变量，用来告诉state，我要切换table的序号。
```
combineReducers()
<!-- 把state拆分多个reducer处理，combineReducers()将其合成一个reducer -->
export default function todos(state = [], action) {
  switch (action.type) {
  case 'ADD_TODO':
    return state.concat([action.text])
  default:
    return state
  }
}
export default function counter(state = 0, action) {
  switch (action.type) {
  case 'INCREMENT':
    return state + 1
  case 'DECREMENT':
    return state - 1
  default:
    return state
  }
}
export default combineReducers({
  todos,
  counter
})
```
> 本方法只是起辅助作用！你可以自行实现不同功能的 combineReducers，甚至像实现其它函数一样，明确地写一个根 reducer 函数，用它把子 reducer 手动组装成 state 对象。
#####   react-redux (Provider)
```
<Provider store={store}>
    <Router ref="router" history={hashHistory}>
        <Route path='/' component={Index}>
            <IndexRoute  component={MainPage}></IndexRoute>
        </Route>
    </Router>
</Provider>
```
> Provider就是把我们用rudux创建的store传递到内部的其他组件。让内部组件可以享有这个store并提供对state的更新。
#####   react-redux (connect)
```
export default connect(mapStateToProps,mapDispatchToProps)(MainPage);
```
```
//需要渲染什么数据
function mapStateToProps(state) {
    return {
        tiger: state.number
    }
}
//需要触发什么行为
function mapDispatchToProps(dispatch) {
    return {
        add: () => dispatch({ type: 'add' }),
        sub: () => dispatch({ type: 'sub' })
    }
}
```
- mapStateToProps：简单来说，就是把状态绑定到组件的属性当中。我们定义的state对象有哪些属性，在我们组件的props都可以查阅和获取。
- mapDispatchToProps：在redux中介绍过，用store.dispatch(action)来发出操作，那么我们同样可以把这个方法封装起来，即绑定到我们的方法中。