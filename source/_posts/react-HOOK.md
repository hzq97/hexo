---
title: react HOOK
tags: 
    - react
categories: 
    - react
---

#   HOOK
##  简单说明
> HOOK是React 16.8新增特性，是不写在class下使用state以及其他的特性

##  api
### 基础HOOK
####    useState
- 返回一个state以及更新该state的函数
```
const [state,setState] = useState(initSatate)
```
- 例子说明
```
function Counter({initialCount}) {
  const [count, setCount] = useState(initialCount);
  return (
    <>
      Count: {count}
      <button onClick={() => setCount(initialCount)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
    </>
  );
}
```
####    useEffect
- 该HOOK接受一个包含命令式、且可能有副作用代码的函数
1. 更新 DOM 之后运行
2. 跟 class 组件中的 componentDidMount、componentDidUpdate 和 componentWillUnmount 具有相同的用途，只不过被合并成了一个 API。
- 清除effect
组件卸载时需要清除 effect 创建的诸如订阅或计时器 ID 等资源。要实现这一点，useEffect 函数需返回一个清除函数。
```
useEffect(() => {
  const subscription = props.source.subscribe();
  return () => {
    // 清除订阅
    subscription.unsubscribe();
  };
});

//每次下次执行之前会取消上次定时器的订阅
useEffect(() => {
      let num = 1;
      const timer = setInterval(function(){
         num = num + 1;
        document.title = `You clicked ${num} times`;
      },1000)
      return () => {
          clearInterval(timer)
      }
  });
```
- 可以给 useEffect 传递第二个参数，它是 effect 所依赖的值数组。(只有在该值发生变化时才会触发)

####    useConcent
- 接受一个context对象

> const useContext(ThemeContex)

```
// App.js
export const ThemeContext = React.createContext("next");
function App() {
  return (
    <div className="App">
      <ThemeContext.Provider value="22222">
            <HelloMessage></HelloMessage>
      </ThemeContext.Provider>
    </div>
  );
}
// index.jsx
import { ThemeContext } from "../../App.js";
const value = useContext(ThemeContext);  // value="22222"
```
### 其他HOOK
####    useReducer
- 更复杂的useState(); 类似于redux

> const [state, dispatch] = useReducer(reducer, initialArg, init);

```
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```
####    useCallback
- 返回一个回调函数  可以与React.memo 配合使用
```
// 当依赖项 a、b发生改变时触发
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```
####    useMemo
- 返回一个值，当某个依赖项发生改变时触发；类似于VUE的计算属性

>   const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);


####    useRef
- 返回一个可变的 ref 对象，其 .current 属性被初始化为传入的参数

> const refContainer = useRef(initialValue);

```
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` 指向已挂载到 DOM 上的文本输入元素
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```
####    useImperativeHandle
####    useLayoutEffect
####    useDebugValue