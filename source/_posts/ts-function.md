---
title: TypeScript函数
tags: 
    - TypeScript
categories: 
    - ts
---

[toc]
##### 带有返回值的参数
```
//带有返回值的参数
function add():number{
    
}

function add():string{
    
}
```
##### 不带有返回值的参数
```
//不带有返回值的参数  void
function add():void{
    
}
```
##### 带参数
```
// 带参数
function add(a:number,b:number):number{
    
}
```
##### 可选参数
```
//注意：可选参数放在最后面
function add(a:number,b?:number):number{
    
}
```
##### 默认参数
```
//默认参数
function add(a:number=5,b?:number):number{
    
}
```
##### 参数不固定
```
// ...扩展符  result类似于es5的argument
function add(...result:number[]):number{
    
}
```
#####   函数重载
```
//函数重载：指出现两个或者两个以上的同名函数
function add(a:number):number{
    
}
function add(a:string):number{
    
}
function add(a:any):number{
    if(typeof a === "string"){
        
    }else if(typeof a === "number"){
        
    }
}

add(1)
add("123")
add(true) //error
```