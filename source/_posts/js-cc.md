---
title: javascript中的本地存储
tags: 
    - javascript
categories: 
    - javascript
---

#   本地存储
### 1.localStorage 

#### 特点
```
1. 域名独享(a.com 设置  b.com 无法读取)
2. 永久保存, 除非手动删除
3. 仅在客户端保存
4. 存储空间一般为 5M
5. 写入数据的时候 只支持字符串.
```
#### 应用场景 
```
1. 统计页面浏览次数.
2. 浏览历史的保存.
3. 购物车功能.
4. 验证令牌保存(afdajlkdsfajlk).( /songs?name=林依晨&page=1&num=20&sign=1231141osdjalkjflas)

//写数据
localStorage.setItem('name','张三');
//删数据
localStorage.removeItem('name');
//读数据
localStorage.getItem('name');
//清空
localStorage.clear();
```
### 2.sessionStorage

#### 特点
```
1. 域名独享
2. *仅在页面打开阶段数据有效*.
3. 仅在客户端保存
4. 存储空间一般为 5M
5. 写入数据的时候 只支持字符串.
```

#### 应用场景
```
1. 多步骤表单数据处理
2. 页面之间的传值(登陆回跳)
3. 状态记录(页面滚动位置)

//写数据
sessionStorage.setItem('name','张三');
//删数据
sessionStorage.removeItem('name');
//读数据
sessionStorage.getItem('name');
//清空
sessionStorage.clear();
```



### 离线应用
```
在离线情况下也可以访问应用
```
#### 操作步骤
```
1. 在 HTML 标签中添加 manifest 属性  `<html manifest="cache.manifest"></html>`
2. 在服务器创建路由 `/cache.manifest`
3. 返回格式

HTTP/1.1 200 OK
Content-type: text/cache-manifest
....

CACHE MANIFEST
/js/app.js
/css/app.css
/images/app.jpg
```
### cookie
```
localStorage 出现之前，cookie被滥用当做了存储工具，什么数据都放在cookie中，即使这些数据只在页面中使用、而不需要随请求传送到服务端
当然cookie也做了一些限制：大小受限、每个域名下生成的cookie数量受限
```
####    cookie是什么？
```
当客户端要发送http请求时，浏览器会先检查下是否有对应的cookie。有的话，则自动地添加在request header中的cookie字段。注意，每一次的http请求时，如果有cookie，浏览器都会自动带上cookie发送给服务端。
```
####    cookie缺点
```
cookie的缺点：

(1) 每个特定域名下的cookie数量有限：

IE6或IE6-(IE6以下版本)：最多20个cookie

IE7或IE7+(IE7以上版本)：最多50个cookie

FF:最多50个cookie

Opera:最多30个cookie

Chrome和safari没有硬性限制

当超过单个域名限制之后，再设置cookie，浏览器就会清除以前设置的cookie。IE和Opera会清理近期最少使用的cookie，FF会随机清理cookie；

(2) 存储量太小，只有4KB；

(3) 每次HTTP请求都会发送到服务端，影响获取资源的效率；

(4) 需要自己封装获取、设置、删除cookie的方法；
```
####    cookie操作
```
document.cookie = "name=lynnshen";
document.cookie = "age=18";
```
```
document.cookie.split('; ');//用“;”和空格来划分cookie
```
```
setCookie(name, "1", -1)//第二个value值随便设个值，第三个值设为-1表示：昨天就过期了，赶紧删除
```