---
title: js深拷贝
categories: js
tags: [深拷贝,弱引用,weakMap] #文章标签，可空，多标签请用格式，注意:后面有个空格
summary: 
---


## 乞丐版

在不使用第三方库的情况下，我们想要深拷贝一个对象，用的最多的就是下面这个方法。
```javascipt
JSON.parse(JSON.stringify());
```
这种写法非常简单，而且可以应对大部分的应用场景，但是它还是有很大缺陷的，比如拷贝其他引用类型、<code>循环引用</code>等情况。

## 基础版
解决对象、数组、循环引用问题

![循环引用：target.target = target报错，因为递归进入死循环导致栈内存溢出了，原因就是上面的对象存在循环引用的情况，即对象的属性间接或直接的引用了自身的情况](./error-01.png)

解决循环引用问题，我们可以额外开辟一个存储空间，来存储当前对象和拷贝对象的对应关系，当需要拷贝当前对象时，先去存储空间中找，有没有拷贝过这个对象，如果有的话直接返回，如果没有的话继续拷贝，这样就巧妙化解的循环引用的问题。
这个存储空间，需要可以存储<code>key-value</code>形式的数据，且<code>key</code>可以是一个引用类型，我们可以选择<code>Map</code>这种数据结构：

- 检查<code>map</code>中有无克隆过的对象
- 有 - 直接返回
- 没有 - 将当前对象作为<code>key</code>，克隆对象作为<code>value</code>进行存储
- 继续克隆

使用<code>weakMap</code>而不是<code>Map</code>的原因：
> `WeakMap` 对象是一组键/值对的集合，其中的键是弱引用的。其键必须是对象，而值可以是任意的。

什么是弱引用呢？
> 在计算机程序设计中，弱引用与强引用相对，是指不能确保其引用的对象不会被垃圾回收器回收的引用。 一个对象若只被弱引用所引用，则被认为是不可访问（或弱可访问）的，并因此可能在任何时刻被回收。

我们默认创建一个对象：`const obj = {}`，就默认创建了一个强引用的对象，我们只有手动将`obj = null`，它才会被垃圾回收机制进行回收，如果是弱引用对象，垃圾回收机制会自动帮我们回收。
举个例子：
如果我们使用`Map`的话，那么对象间是存在强引用关系的：
```javascript
let obj = { name : 'ConardLi'}
const target = new Map();
target.set(obj,'code秘密花园');
obj = null;
```
虽然我们手动将`obj`，进行释放，然是`target`依然对`obj`存在强引用关系，所以这部分内存依然无法被释放。

再来看 `WeakMap`：
```javascript
let obj = { name : 'ConardLi'}
const target = new WeakMap();
target.set(obj,'code秘密花园');
obj = null;
```
如果是`WeakMap`的话，`target`和`obj`存在的就是弱引用关系，当下一次垃圾回收机制执行时，这块内存就会被释放掉。
设想一下，如果我们要拷贝的对象非常庞大时，使用`Map`会对内存造成非常大的额外消耗，而且我们需要手动清除`Map`的属性才能释放这块内存，而`WeakMap`会帮我们巧妙化解这个问题。

```javascript
const target = {
	field1: 1,
	field2: undefined,
	field3: {
		child: 'child'
	},
	field4: [2, 4, 8]
};

function clone(target,map=new WeakMap()) {
	// 解决对象、数组、循环引用问题
	if(typeof target !== 'object' || target === null){
		return target
	}else{
		let cloneTarget = Array.isArray(target)?[]:{};
		if(map.get(target)){
			return map.get(target)
		}
		map.set(target,cloneTarget)
		for(const key in target){
			cloneTarget[key] = clone(target[key],map)
		}
		return cloneTarget
	}
}
target.target = target
console.log(clone(target))
```

## 其他引用类型
```javascript
function clone2(target, map = new WeakMap()) {
	if (typeof target !== 'object' || target === null) {
		return target
	} else {
		let Const = target.constructor;
		let cloneTarget;
		// 引用类型分类
		let type = Object.prototype.toString.call(target).slice(8, -1);

		if (['Array', 'Object', 'Map', 'Set', 'Argument'].includes(type)) {
			cloneTarget = new Const();
			if (map.get(target)) {
				return map.get(target)
			}
			map.set(target, cloneTarget)
			// 可遍历引用类型：
			if (type === 'Array' || type === 'Object' || type === 'Argument') {
				for (const key in target) {
					cloneTarget[key] = clone2(target[key], map)
				}
			}
			if (type === 'Map') {
				target.forEach((v, k) => {
					cloneTarget.set(k, clone2(v, map))
				})
			}
			if (type === 'Set') {
				target.forEach(v => {
					cloneTarget.add(clone2(v, map))
				})
			}
			return cloneTarget
		} else {
			// 不可遍历引用类型：
			switch (type) {
				case 'Boolean':
				case 'Number':
				case 'String':
				case 'Error':
				case 'Date':
					return new Const(target);
				case 'RegExp':
					return cloneReg(target);
				case 'Symbol':
					return Object(Symbol.prototype.valueOf.call(target));;
				case 'URL':
					return new Const(target.href)
				case 'DataView':
					return new Const(target.buffer.slice(0), target.byteOffset, target.byteLength)
				case 'BigInt':
					return Object(Const.prototype.valueOf.call(target))
				default:
					return CloneOther(target, type);
			}
		}
	}

}

function CloneOther(target, type) {
	let result = null;
	if (type.includes('Array')) {
		// ArrayBuffer TypeArray BigArray ...
		result = target.slice()
	} else if (type.includes('Function')) {
		// Function AsyncFunction GeneratorFunction
		result = target
	}
	return result
}

function cloneReg(target) {
	const result = new target.constructor(target.source, /\w*$/.exec(target));
	result.lastIndex = target.lastIndex;
	return result;
}

target2.target = target2
console.log(clone2(target2))
```

[来源：https://juejin.cn/post/6844903929705136141#heading-10](https://juejin.cn/post/6844903929705136141#heading-10)