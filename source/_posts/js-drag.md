---
title: javascript中的拖拽
tags: 
    - javascript
categories: 
    - javascript
---

###     drag拖拽
#####   1.给标签添加拖拽属性draggable
```     
<div class="box" draggadle></div>
```
#####   2.关于拖拽事件
######  a.被拖拽的事件
```
1.ondragstart  开始拖拽事件
    box.ondragstart = function(e) {
        ...
    }
2.ondrag  拖拽中的事件
3.ondragend  拖拽结束事件
```
######  b.拖拽目标容器事件
```
1.ondragenter   进入目标容器事件
2.ondragover    在目标容器内悬浮事件
3.ondrop        在目标容器内松手事件
目标容器.ondrop = function(e) {
    ...
}
4.ondragleave   离开目标容器事件
```

#####   关于事件参数
```
e.target.id  //id名
e.target.className //class名
......
...

//设置数据 (被拖拽里写)
e.dataTransfer.setData("Text", e.target.id); //这里的键名一定要用 Text 因为 IE 有兼容性问题
e.dataTransfer.setData('名字',e.target.id)

//获取数据
e.dataTransfer.getData('名字');
/***可以通过查找元素然后移动到目标位置***/
appendChild(); insertbefore(); ...等

```