---
title: Object.defineProperty()的参数及使用
categories: js
tags: [js]
---

> Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

### 语法
Object.defineProperty(obj, attr, descriptor)


### 参数
obj : 对象
attr : 要定义或编辑的对象属性名
descriptor : 对象类型，要定义或修改属性的描述

#### descriptor描述分类
- 数据描述符 & 存取描述符 特性
	- configurable: 是否可以重新定义 默认false
	- enumerable: 是否可枚举 默认false
	- value: 属性初始值 默认 undefined
	- writable: 是否可以修改属性值，默认false
- 存取描述符 特性
	- get: 回调函数，根据其他属性动态计算得到当前属性值。默认为 undefined。
	- set: 回调函数，监视当前属性值的变化，更新其他相关的属性值。默认为 undefined。
	
	
### 实例

```javascript
let obj = {
	firstName:'A',
	lastName:'B'
}
Object.defineProperty(obj,'fullName',{
	get:function(){
		return this.firstName+'-'+this.lastName;
	},
	set:function(value){
		this.firstName = value.split('-')[0];
		this.lastName = value.split('-')[1];
	}
})
console.log(obj.fullName);		// A-B 
obj.firstName = "C";
obj.lastName = "D";
console.log(obj.fullName);		// C-D
obj.fullName = "E-F";
console.log(obj.firstName, obj.lastName);	// E F

Object.defineProperty(obj,'fullName2',{
	configurable:false,
	enumerable:true,
	value:'G-H',
	writable:false
})
console.log(obj.fullName2);	// G-H
obj.fullName2 = "J-K";
console.log(obj.fullName2);	// G-H	// writable:false不可修改
// Object.defineProperty(obj,'fullName2',{	// typeError cannot redefine property:fullName2 // configurable:false, 不可重新定义
// 	configurable:false,
// 	enumerable:true,
// 	value:'J-K',
// 	writable:false
// })

// Object.keys 可得到属性自身可枚举属性的属性名
let names = Object.keys(obj)
console.log(names);	// ["firstName","lastName","fullName2"]
```
