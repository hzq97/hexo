---
title: TypeScript装饰器
tags: 
    - TypeScript
categories: 
    - ts
---

#####   类装饰器
```
// 普通装饰器   装饰器工厂（可带参）
// 定义 普通装饰器
function logClass(target:any){// 接受参数为当前类
    console.log(target)
}

@logClass
class PerSon{
    constructor(){
        
    }
}
//装饰器工厂
function logClass(params:any){// 接受参数为当前传参
     return function(target:any){// 接受参数为当前类
        console.log(params)  // 123
        console.log(target)
    }
}

@logClass("123")
class PerSon{
    constructor(){
        
    }
}

//可以进行扩展或者重载
```
#####   属性装饰器
```
function logProperty(params:any){// 接受参数为当前传参
     return function(target:any,attr:any){//接受参数为当前类 和 接受参数为当前属性名称
        console.log(params)  // 123
        console.log(target)
        console.log(attr)
    }
} 

@logProperty("ddd")
```
#####   方法装饰器
```
function logFun(params:any){// 接受参数为当前传参
     return function(target:any,methodName:any,desc:any){//接受参数为当前类 和 接受参数为当前方法名称  和  方法的描述
        console.log(params)  // 123
        console.log(methodName)
        console.log(desc)
    }
} 

//可以修改desc.value  当前方法
```
#####   方法参数装饰器 
```
function logParams(params:any){// 接受参数为当前传参
     return function(target:any,paramsName:any,paramsIndex:any){//接受参数为当前类  和  方法的名称  和参数的索引位置
        console.log(params)  // 123
        console.log(methodName)
        console.log(desc)
    }
} 

function getName(@logParams("张三") name:string){}
```
#####   装饰器执行顺序
```
属性》方法》参数》类
多个装饰器  从后向前依次执行
```