---
title: javascript中的多线程
tags: 
    - javascript
categories: 
    - javascript
---

[toc]
### Web worker  客户端多线程
#####   实例
```
index.html
***************************************
var worker = new Worker('访问文件地址');
//发送数据
worker.postMessage(向访问文件发送的数据);
//回调函数
worker.onmessage = function(res) {
    console.log(res.data)
}
//??
worker.terminate();
```

```
访问文件
//访问文件导入外部数据（importScripts）
importScripts("https://unpkg.com/axios/dist/axios.min.js");
//接收传递的数据
onmessage = function(res){
    console.log(res.data);
}
postMessage(传回的数据);
```

##### 注意点
不能操作DOM

##### 应用场景
1. 计算密集
2. 耗时请求
