---
title: redux中间件 react-thunk
tags: 
    - react
    - redux
categories: 
    - react
---
redux store 仅支持同步数据流。thunk等中间件可以帮助redux应用中实现异步性。可以将thunk看做store的dispatch()方法的封装器

> Action 发出后，Reducer 立即计算出 State,这叫同步；Action发出以后，过一段时间再执行 Reducer，这就是异步

- redux-thunk是一个常用的redux异步action中间件。通常用来处理axios请求。
- redux-thunk中间件可以让action创建函数先不返回一个action对象，而是返回一个函数

```
npm install redux-thunk
const store = createStore(
  reducers,
  applyMiddleware(thunk)
);

action:
// 使用了 redux-thunk, action函数可以返回一个函数
export const getTodoList = () => {
    return (dispatch) => {
        axios.get('/api/list.json').then(res => {
            const { data } = res;
            const action = initListAction(data);
            dispatch(action);
        })
    }
}

// 调用
const action = getTodoList();
// 这里取到的action是一个函数，当执行dispatch时，会自动执行该函数
store.dispatch(action);
applyMiddlewares()是Redux的原生方法
```