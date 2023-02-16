---
title: javascript闭包、原型链
tags: 
    - javascript
categories: 
    - javascript
---

#### 原型/构造函数/实例
```
原型（prototype）：一个简单的对象，用于实现对象的属性继承。可以简单的理解成对象的爹。在Firefox和Chrome中，每个JavaScript对象中都包含一个_proto_（非标准）的属性指向他爹（该对象的原型），可obj._proto_进行访问
构造函数：可以通过new来新建一个对象的函数
实例：通过构造函数和new创建出来的对象，便是实例。实例通过proto指向原型，通过constructor指向构造函数
<!-- 实例 -->
const instance = new Object()
<!-- 原型 -->
const prototype = Object.prototype

三者关系
实例._proto_ === 原型   
原型.contructor === 构造函数
构造函数.condtructor === 原型

向原型里添加值或函数，可以让继承的时候也拥有值或函数
```
####    原型链
```
原型链由原型对象组成，每个对象都有proto属性，指向了创建该对象的构造函数的原型，proto将对象连接组成了原型链。是一个用来实现继承和共享属性的有限的对象链
```

####    闭包
```
闭包属于一种特殊的作用域，称为静态作用域。它的定义可以理解为：父函数被销毁的情况下，返回出的子函数[[scope]]中任然保留着父级的单位变量和作用域链，因此可以继续访问到父级的变量对象，这样的函数称为闭包

　　function f1(){
　　　　var n=999;
　　　　function f2(){
　　　　　　alert(n); 
　　　　}
　　　　return f2;
　　}
　　var result=f1();
　　result(); // 999
　　
作用：可以读取函数内部变量  让这些变量的值始终保持在内存中。
注意：由于闭包会使得函数的变量都保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页性能问题，ie中可能导致内存泄漏   所以退出函数之前 把不使用的变量删除
```