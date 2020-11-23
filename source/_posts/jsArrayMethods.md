---
title: js Array 方法总结 #文章页面上的显示名称，一般是中文
categories: js #分类
tags: [js] #文章标签，可空，多标签请用格式，注意:后面有个空格
summary: 
---


### 改变原数组的方法
- push  数组末尾追加元素
- pop   删除数组最后元素
- unshift   数组开始添加元素
- shift     删除数组开始元素
- [splice   实现数组的增加、删除、修改](#item1)
- sort  数组排序
- reverse   数组倒序

[删除数组末尾一项的几种方法](#item2)
[向数组末尾追加项的几种方法](#item3)

### 不改变原数组的方法
- [slice - 由begin和end决定的原数组的浅拷贝](#item4) 
- concat 拼接两个数组
- toString()  数组转字符串，转换后用逗号分隔
- join 数组转字符串 未指定分隔符用逗号分隔
- indexOf/lastIndexOf 检测当前项的index值 - ary.indexOf(val)
- includes 检测数组是否包含某项 ary.includes(val)=>true||false
- forEach 遍历数组中每一项，无返回值
- map 数组映射 支持返回值 新数组跟旧数组长度一样
- [reduce 累加器](https://qiuchunhong.github.io/2020/06/21/jsArrayRecude/)
- filter((val,index,array)=>{ return }) 过滤
- find 返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined。arr.find( (element,index,arry)=>{return} )
- findIndex 返回数组中满足提供的测试函数的第一个元素的索引。否则返回-1。arr.findIndex( (element,index,arry)=>{return} )
- some 一项条件成立就返回true都不成立返回false。arr.some(callback(element[, index[, array]])[, thisArg])
- every 所有的都成立才返回true,有一项不成立返回false。arr.every(callback(element[, index[, array]])[, thisArg])
- [flat 扁平化 newArray = arr.flat([depth]) depth默认1，Infinity](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)

[slice 方法将一个类数组（Array-like）对象/集合转换成一个新数组](#item5)
************
#### <font id="item1">splice 实现数组的增加、删除、修改</font>
> 使用方法：array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
**start​**
指定修改的开始位置（从0计数）。如果超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位（从-1计数，这意味着-n是倒数第n个元素并且等价于array.length-n）；如果负数的绝对值大于数组的长度，则表示开始位置为第0位。
**deleteCount**<font color=red>【可选】</font>
整数，表示要移除的数组元素的个数
如果 deleteCount 大于 start 之后的元素的总数，则从 start 后面的元素都将被删除（含第 start 位）。
如果 deleteCount 被省略了，或者它的值大于等于array.length - start(也就是说，如果它大于或者等于start之后的所有元素的数量)，那么start之后数组的所有元素都会被删除。
如果 deleteCount 是 0 或者负数，则不移除元素。这种情况下，至少应添加一个新元素
**item1, item2, ...**<font color=red>【可选】</font>


【删除】返回值是一个数组，里面是删除项
- ary.splice(0):可以清空数组，把原始数组中的内容基于新数组储存起来（有点类似于数组克隆）
- ary.splice(ary.length-1):删除最后一项
- ary.splice(n):从第n位开始删除后面所有元素
- ary.splice(0,1):删除第一项

【新增】 ary.splice(n,0,x);在索引n的前面添加了x项；
- ary.splice(ary.length,0,x)：在数组最后增加x项；
- ary.splice(0,0,x):在数组开头增加x项；

【修改】 用x替代删除的m即可

#### <font id="item2">删除数组末尾一项的几种方法 </font>
- arr.length--
- arr.pop()   ：返回结果 删除的项
- arr.splice(arr.length-1)   ： 返回结果是 [删除的项]
- delete arr[arr.length-1]    ： 虽然能删除，但是length长度不变（一般不用）

#### <font id="item3">向数组末尾追加项的几种方法 </font>
- arr.push("增加的项")
- arr[arr.length]="增加的项"
- arr.splice(arr.length , 0 , "增加的项")



### <font id="item4">slice</font>
> slice() 方法返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括end）。原始数组不会被改变。

> **begin**<font color=red>【可选】</font>
提取起始处的索引（从 0 开始），从该索引开始提取原数组元素。
如果该参数为负数，则表示从原数组中的倒数第几个元素开始提取，slice(-2) 表示提取原数组中的倒数第二个元素到最后一个元素（包含最后一个元素）。
如果省略 begin，则 slice 从索引 0 开始。
如果 begin 大于原数组的长度，则会返回空数组。
**end**<font color=red>【可选】</font>
提取终止处的索引（从 0 开始），在该索引处结束提取原数组元素。slice 会提取原数组中索引从 begin 到 end 的所有元素（包含 begin，但不包含 end）。
slice(1,4) 会提取原数组中从第二个元素开始一直到第四个元素的所有元素 （索引为 1, 2, 3的元素）。
如果该参数为负数， 则它表示在原数组中的倒数第几个元素结束抽取。 slice(-2,-1) 表示抽取了原数组中的倒数第二个元素到最后一个元素（不包含最后一个元素，也就是只有倒数第二个元素）。
如果 end 被省略，则 slice 会一直提取到原数组末尾。
如果 end 大于数组的长度，slice 也会一直提取到原数组末尾。

### <font id="item5">slice 方法将一个类数组（Array-like）对象/集合转换成一个新数组</font>
```
function list() {
  return Array.prototype.slice.call(arguments);
}
var list1 = list(1, 2, 3); // [1, 2, 3]
```