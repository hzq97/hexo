---
title: redux-router
tags: 
    - react
    - reactRouter
categories: 
    - react
---

###    BrowserRouter：h5路由（history API）      HashRouter：哈希路由
####    BrowserRouter
```
使用 HTML5 历史 API 记录（ pushState，replaceState 和 popstate 事件）的 <Router> 使您的UI与URL保持同步。
```
```
1.basename:string
所有位置的基本URL
<BrowserRouter basename="/calendar"/>（所有路由路径都在/calendar后面）
<Link to="/today"/> // renders <a href="/calendar/today">

2.getUserConfirmation: func
用于确认导航的功能  离开一个导航到另一个导航

3.forceRefresh:bool
如果为 true，则路由器将在页面导航中使用整页刷新。

4.keyLength: number
location.key 的长度。默认为 6。

5.children: node
一个用于渲染的 single child element
```
####    HashRouter
####    Link
```
1.to:string
链接位置的字符串表示，通过连接位置的路径，搜索和hash属性创建
2.to:object
    pathname: 表示要链接到的路径的字符串。
    search: 表示查询参数的字符串形式。
    hash: 放入网址的 hash，例如 #a-hash。
    state: 状态持续到 location。
3.replace:bool
如果为 true，则单击链接将替换历史堆栈中的当前入口，而不是添加新入口。
4.innerRef: function
允许访问 ref 组件的底层
5.others  您还可以传递您想要放在 <a> 上的属性，例如标题，ID，className 等。
```
####    NavLink
```
一个特殊版本的Link，当它与当前 URL 匹配时，为其渲染元素添加样式属性。
```
```
1.activeClassName: string
要给出的元素的类处于活动状态时。默认的给定类是 active。它将与 className 属性一起使用。
2.activeStyle: object
当元素处于 active 时应用于元素的样式。
3.exact: bool
如果为 true，则仅在位置完全匹配时才应用 active 的类/样式。
4.strict: bool
当情况为 true，要考虑位置是否匹配当前的URL时，pathname 尾部的斜线要考虑在内。有关更多信息，请参见 <Route strict> 文档。
5.isActive: func
一个为了确定链接是否处于活动状态而添加额外逻辑的函数，如果你想做的不仅仅是验证链接的路径名与当前 URL 的 pathname 是否匹配，那么应该使用它
6.location: object
isActive 比较当前的历史 location（通常是当前的浏览器 URL ）。为了与不同的位置进行比较，可以传递一个 location。
```
####    Prompt
```
从核心重新导出 Prompt。
```
####    Redirect
```
<Redirect to="/router3" from="/router3333"/>
```
```
1.to:string
重定向到的 URL，可以是任何 path-to-regexp 能够理解有效 URL 路径。在 to 中使用的 URL 参数必须由 from 覆盖。
2.to:object
重定向到的 location，pathname 可以是任何 path-to-regexp 能够理解的有效的 URL 路径。
3.push: bool
当 true 时，重定向会将新地址推入 history 中，而不是替换当前地址。
4.from: string
重定向 from 的路径名。
5.exact: bool
完全匹配 from；
6.strict: bool
严格匹配 from；
```
####    Route
```
<Route> 有三种渲染的方法：
    <Route component>
    <Route render>
    <Route children>
所有三种渲染方法都将通过相同的三个 Route 属性。
    match
    location
    history
```
```
1.component
只有当位置匹配时才会渲染的 React 组件。
2.render: func
这允许方便的内联渲染和包裹
3.children: func
有时你需要渲染路径是否匹配位置。在这些情况下，您可以使用函数 children 属性，它的工作原理与渲染完全一样，不同之处在于它是否存在匹配。
4.path: string
任何 path-to-regexp 可以解析的有效的 URL 路径
5.exact: bool
如果为 true，则只有在路径完全匹配 location.pathname 时才匹配。
6.strict: bool
如果为 true 当真实的路径具有一个斜线将只匹配一个斜线
7.location: object
一个 <Route> 元素尝试其匹配 path 到当前的历史位置（通常是当前浏览器 URL ）。但是，也可以通过location 一个不同 pathname 的匹配。
8.sensitive: bool
如果路径区分大小写，则为 true ，则匹配。
```
####    Router
```
history: object
用来导航的 history 对象。
children: node
需要渲染的单一组件。
```
####    StaticRouter
```
1.basename: string
所有 location 的基本 URL。
2.location: string
服务器收到的 URL，或许 req.url 会位于节点服务器上。
3.location: object
一个位置对象形似 { pathname, search, hash, state }。
4.context: object
一个普通的 JavaScript 对象。
5.children: node
一个用来渲染的 单一的子元素。
```
####    Switch
```
1.location: object
用于匹配子元素而不是当前历史位置（通常是当前浏览器 URL ）的 location。
2.children: node
<Switch> 的所有子级都应该是 <Route> 或 <Redirect> 元素。将渲染当前位置匹配的第一个子级。
```
####    路由传参
```
<Link to="/about/511">about</Link>
<Route path='/about/:ID' component={Path}/>

组件接受参数
this.props.match.params.name

缺点:只能传字符串，并且，如果传的值太多的话，url会变得长而丑陋。
```
```
<Route path='/query' component={Query}/>
<Link to={{ path : ' /query' , query : { name : 'sunny' }}}>
this.props.history.push({pathname:"/query",query: { name : 'sunny' }});
读取参数用: this.props.location.query.name
缺点：刷新地址栏，参数丢失
```
```
<Route path='/sort ' component={Sort}/>
<Link to={{ path : ' /sort ' , state : { name : 'sunny' }}}> 
this.props.history.push({pathname:"/sort ",state : { name : 'sunny' }});
读取参数用: this.props.location.query.state 
```
```
<Route path='/web/departManange ' component={DepartManange}/>
<link to="web/departManange?tenantId=12121212">xxx</Link>
this.props.history.push({pathname:"/web/departManange?tenantId" + row.tenantId});
读取参数用: this.props.location.search
```