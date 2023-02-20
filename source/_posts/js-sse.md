---
title: javascript 服务器推送
tags: 
    - javascript
categories: 
    - javascript
---

### SSE 服务器主动向客户端推送消息

#####   实例
```
index.html
var source = new EventSource(url路径);
source.onmeaasge = function(res) {
    console.log(res.data);
}

```

```
服务器（node）
1. 必须设置响应头信息
res.setHeader('Content-Type','text/event-stream');
2.设置推送消息
setInterval(function(){
    res.write('data:推送内容\n\n');
},1000);
3.不用res.end()
```
#####   应用场景
1. 站内信息
2. 通告
3. 短消息
...