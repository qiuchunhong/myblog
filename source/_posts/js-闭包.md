---
title: 闭包
categories: 闭包
tags: [js] #文章标签，可空，多标签请用格式，注意:后面有个空格
summary: 闭包让你可以在一个内层函数中访问到其外层函数的作用域；外层函数调用之后其变量对象本应该被销毁，但闭包的存在使我们仍然可以访问外层函数的变量对象，这就是闭包的重要概念
---

> 闭包让你可以在一个内层函数中访问到其外层函数的作用域；外层函数调用之后其变量对象本应该被销毁，但闭包的存在使我们仍然可以访问外层函数的变量对象，这就是闭包的重要概念


## 例题一
```javascript
function createCounter() {
  var counter = 5
  const myFunction = function() {
    counter = counter + 1
    return counter
  }
   return myFunction
 }
const increment = createCounter()
const c1 = increment()
const c2 = increment()
const c3 = increment()
const increment2 = createCounter()
const d1 = increment2()
const d2 = increment2()
console.log('example increment', c1, c2, c3, d1, d2);// 6 7 8 6 7

```