---
title: js题库
date: 2020-07-14 16:35:44
tags: js 整合
---


- [判断数据类型：typeof()、instanceof、constructor、Objece.prototype.toString.call()](@item1)

#### <font id="item1">判断数据类型：typeof()、instanceof、constructor、Objece.prototype.toString.call()</font>
1. typeof()
typeof 可以判断基础类型[null除外，null返回Object]，引用数据类型Function/Objece/Array反回的都是Object
2. [] instanceof Array 
判断引用对象的类型返回的是true||false，不能判断基础类型[全部返回false]
```
console.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false  
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true    
// console.log(undefined instanceof Undefined);      // 报错
// console.log(null instanceof Null);                // 报错
```
3. ().constructor === Array
```
console.log((2).constructor === Number); // true
console.log((true).constructor === Boolean); // true
console.log(('str').constructor === String); // true
console.log(([]).constructor === Array); // true
console.log((function() {}).constructor === Function); // true
console.log(({}).constructor === Object); // true
```
> 这里有一个坑，如果我创建一个对象，更改它的原型，constructor就会变得不可靠了
```
function Fn(){};
 
Fn.prototype=new Array();
 
var f=new Fn();
 
console.log(f.constructor===Fn);    // false
console.log(f.constructor===Array); // true 
```
4. Object.prototype.toString.call() 使用 Object 对象的原型方法 toString ，使用 call 进行狸猫换太子，借用Object的 toString 方法
```
var a = Object.prototype.toString;
 
console.log(a.call(2));
console.log(a.call(true));
console.log(a.call('str'));
console.log(a.call([]));
console.log(a.call(function(){}));
console.log(a.call({}));
console.log(a.call(undefined));
console.log(a.call(null));
```