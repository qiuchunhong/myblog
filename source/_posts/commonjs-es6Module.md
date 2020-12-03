---
title: commonjs跟es6模块有什么区别
categories: js
tags:
  - commonJs
  - es6 Module
---

> 历史上，js一直没有模块(module)体系，无法将一个项目拆分成多个模块文件。正对这一情况，社区出现了一些统一的规范：CommonJs和AMD，前者是针对服务端的js，也就是nodejs。后者是针对浏览器的。ES6在语言标准层面上，实现了模块功能，而且实现也比较简单。完全可以取代CommonJs和AMD。达成前后端js的模块风格统一。不过目前还未完全达到这一目的。nodejs目前主流还是采用CommonJS规范。不过在v13.2版本，nodejs已经实现了ES6模块语法，还未正式替换，在考察阶段。v13.2版本将js文件以 .mjs结尾，nodejs将它视为ES6模块。以 .cjs结尾则视为CommonJS模块。也可以在包的package.json文件中增加 "type": "module"信息。nodejs则将整个包都视为ES6模块来加载运行。

- CommonJS与ES6 Module最本质的区别在于CommonJS对模块依赖的解决是“动态的”而ES6 Module是“静态的”；
- 即commonjs是运行时加载模块，ES6是在静态编译期间就确定模块的依赖；
- ES6在编译期间会将所有import提升到顶部，commonjs不会提升require；
- 两者的模块导入导出语法不同，commonjs是module.exports，exports导出，require导入；ES6则是export导出，import导入；
- commonjs导出的是一个值拷贝，会对加载结果进行缓存，一旦内部再修改这个值，则不会同步到外部。ES6是导出的一个引用，内部修改可以同步到外部；
- commonjs中顶层的this指向这个模块本身，而ES6中顶层this指向undefined。
- 然后就是commonjs中的一些顶层变量在ES6中不再存在：
```
arguments
require
module
exports
__filename
__dirname
```

## CommonJS
1. 对于基本数据类型，属于复制。即会被模块缓存。同时，在另一个模块可以对该模块输出的变量重新赋值。
2. 对于复杂数据类型，属于浅拷贝。由于两个模块引用的对象指向同一个内存空间，因此对该模块的值做修改时会影响另一个模块。
3. 当使用require命令加载某个模块时，就会运行整个模块的代码。
4. 当使用require命令加载同一个模块时，不会再执行该模块，而是取到缓存之中的值。也就是说，CommonJS模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回第一次运行的结果，除非手动清除系统缓存。
5. 循环加载时，属于加载时执行。即脚本代码在require的时候，就会全部执行。一旦出现某个模块被"循环加载"，就只输出已经执行的部分，还未执行的部分不会输出。
## ES6模块
1. ES6模块中的值属于【动态只读引用】。
2. 对于只读来说，即不允许修改引入变量的值，import的变量是只读的，不论是基本数据类型还是复杂数据类型。当模块遇到import命令时，就会生成一个只读引用。等到脚 本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。
3. 对于动态来说，原始值发生变化，import加载的值也会发生变化。不论是基本数据类型还是复杂数据类型。
4. 循环加载时，ES6模块是动态引用。只要两个模块之间存在某个引用，代码就能够执行。

CommonJs模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化不会影响到这个值
``` javascript
// common.js
var count = 1;
var printCount = () =>{ 
  return ++count;
}
module.exports = {
  printCount: printCount,
  count: count
};
// index.js
let v = require('./common');
console.log(v.count);         // 1
console.log(v.printCount());  // 2
console.log(v.count);         // 1
```
明明common.js里面改变了count，但是输出的结果还是原来的。这是因为count是一个原始类型的值，会被缓存。除非写成一个函数，才能得到内部变动的值。将common.js里面的module.exports 改写成
```javascript
module.exports = {
     printCount: printCount,
     get count(){
         return count
     }
};
```
这样子的输出结果是 1，2，2

而在ES6当中，写法是这样的，是利用export 和import导入的
```javascript
// es6.js
export let count = 1;
export function printCount() {
  return  ++count;
}
// main1.js
import  { count, printCount } from './es6';
console.log(count)          // 1
console.log(printCount());  // 2
console.log(count)          // 2 
```

```javascript
// export default
let count = 1;
function printCount() {
  return  ++count;
} 
export default { count, printCount}
// main3.js
import res form './main3.js'
console.log(res.count)
```