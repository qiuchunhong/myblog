---
title: js 中 call()、apply()、bind()
categories: js
tags: [js] #文章标签，可空，多标签请用格式，注意:后面有个空格
summary: call()、apply()、bind() 都是用来重定义this这个对象的。
---

```
let age = 3;
let obj = {
    name:'Jerry',
    objAge:this.age,
    myFun:function(fm,t){
       console.log(this.name+'年龄：'+this.age+', 来自'+fm+'去往'+t)
    }
}
let db = {
    name:'Spike',
    age:5
}
console.log(obj.objAge);    // undefined
obj.myFun('a','b');   // Jerry年龄：undefined, 来自a去往b
obj.myFun.call(db,'成都','上海');   // Spike年龄：5, 来自成都去往上海 
obj.myFun.apply(db,['成都','上海']);   // Spike年龄：5, 来自成都去往上海
obj.myFun.bind(db,'成都','上海')();   // Spike年龄：5, 来自成都去往上海
obj.myFun.bind(db,['成都','上海'])();   // Spike年龄：5, 来自成都,上海去往undefined
```
## 模拟实现 call 和 apply
可以从以下几点来考虑如何实现
- 不传入第一个参数，那么默认为 window
- 改变了 this 指向，让新的对象可以执行该函数。那么思路是否可以变成给新的对象添加一个函数，然后在执行完以后删除？

```
Function.prototype.myCall = function(context){
  let c = context || window;
  c.fn = this;
  let args = [...arguments].slice(1)
  let result = c.fn(...args);
  delete c.fn;
  return result;
}
let a = {name:'lee'};
let b = {
  fn(f,t){
    console.log(`my name is ${this.name}, from ${f} to ${t}`);
  }
}
b.fn.myCall(null, "上海", "成都); // // my name is , from 上海 to 成都
b.fn.myCall(undefined, "上海", "成都); // // my name is , from 上海 to 成都
b.fn.myCall(a,'上海','成都');  // my name is lee, from 上海 to 成都
```

```
Function.prototype.myApply = function(context){
  let c = context || window;
  c.fn = this;
  let result;
  if(arguments[1]){
    result = c.fn(...arguments[1]);
  }else{
    result = c.fn()
  }
  delete c.fn;
  return result;
}
let a = {
  myfn(f,t){
    console.log('我是'+this.name+',我从'+f +'来，去往'+t)
  }
}
b = {
  name:'lee'
}
a.myfn.myApply(b,['上海','成都']);  // 我是lee,我从上海来，去往成都
```