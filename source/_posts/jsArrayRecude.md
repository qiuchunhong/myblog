---
title: reduce - js #文章页面上的显示名称，一般是中文
categories: js #分类
tags: [js, reduce] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: reduce方法 && reduce实例
---

> **reduce()** 方法对数组中的每个元素执行一个由您提供的 reducer 函数(升序执行)，将其结果汇总为单个返回值。

> **语法：**
> arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])

**reducer** callback 接收 4 个参数:

1. Accumulator (acc) (累计器)
2. Current Value (cur) (当前值)
3. Current Index (idx) (当前索引) <font color="red">[可选]</font>
4. Source Array (src) (源数组) <font color="red">[可选]</font>

- initialValue 作为第一次调用 callback 函数时的第一个参数的值。 <font color="red">[可选]</font>

**注意**

- 回调函数第一次执行时，<font color=#0099ff>accumulator</font> 和<font color=#0099ff>currentValue</font>的取值有两种情况：如果调用 reduce()时提供了<font color=#0099ff>initialValue</font>，<font color=#0099ff>accumulator</font>取值为<font color=#0099ff>initialValue</font>，<font color=#0099ff>currentValue</font>取数组中的第一个值；如果没有提供 <font color=#0099ff>initialValue</font>，那么<font color=#0099ff>accumulator</font>取数组中的第一个值，<font color=#0099ff>currentValue</font>取数组中的第二个值。
  > - 如果数组为空且没有提供 initialValue，会抛出 TypeError
  > - 如果数组仅有一个元素并且没有提供 initialValue， 或者有提供 initialValue 但是数组为空，那么此唯一值将被返回并且 callback 不会被执行

### 例子

- [数组求和](#item1)
- [累加对象数组里的值](#item2)
- [将二维数组转化为一维](#item3)
- [计算数组中每个元素出现的次数](#item4)
- [按属性对 object 分类](#item5)
- [使用扩展运算符和 initialValue 绑定包含在对象数组中的数组](#item6)
- [数组去重](#item7)
- [更多...](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

#### <font id="item1">数组求和</font>
```
(()=>{
    var total = [ 0, 1, 2, 3 ].reduce(
    ( acc, cur ) => acc + cur,
    0
    );
    return total    // 6
})()
```

#### <font id="item2">累加对象数组里的值</font>
```
(()=>{
    var sum = [{x: 1}, {x:2}, {x:3}].reduce(
        (accumulator, currentValue) => accumulator + currentValue.x
        ,0
    );
    return sum  // 6
})()
```

#### <font id="item3">将二维数组转化为一维</font>
```
(()=>{
    var flattened = [[0, 1], [2, 3], [4, 5]].reduce(
    function(a, b) {
        return a.concat(b);
    },[]);
    return flattened    // [0, 1, 2, 3, 4, 5]
})()
```

#### <font id="item4">计算数组中每个元素出现的次数</font>
```
(()=>{
    var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];
    var countNames = names.reduce((allNames,name)=>{
        if(name in allNames){
            allNames[name]++
        }else{
            allNames[name] = 1
        }
        return allNames
    },{})
    return countNames
    // {Alice: 2, Bob: 1, Tiff: 1, Bruce: 1}
})()
```

#### <font id="item5">按属性对 object 分类</font>
```
(()=>{
    var people = [
    { name: 'Alice', age: 21 },
    { name: 'Max', age: 20 },
    { name: 'Jane', age: 20 }
    ];
    function groupBy(arr,attr){
        return a = arr.reduce((acc,cur)=>{
            if(cur[attr] in acc){
                acc[cur[attr]].push(acc)
            }else{
                acc[cur[attr]] = [acc]
            }
            return acc
        },{})
    }
    return groupBy(people,'age')
    // { 
    //   20: [
    //     { name: 'Max', age: 20 }, 
    //     { name: 'Jane', age: 20 }
    //   ], 
    //   21: [{ name: 'Alice', age: 21 }] 
    // }
})()
```

#### <font id="item6">使用扩展运算符和 initialValue 绑定包含在对象数组中的数组</font>
```
(()=>{
    var friends = [{
        name: 'Anna',
        books: ['Bible', 'Harry Potter'],
        age: 21
    }, {
        name: 'Bob',
        books: ['War and peace', 'Romeo and Juliet'],
        age: 26
    }, {
        name: 'Alice',
        books: ['The Lord of the Rings', 'The Shining'],
        age: 18
    }];
    var allbooks = friends.reduce((prev, curr) =>{
        return [...prev, ...curr.books];
    }, ['Alphabet']);
    return allbooks
    // ["Alphabet", "Bible", "Harry Potter", "War and peace", "Romeo and Juliet", "The Lord of the Rings", "The Shining"]
})()
```

#### <font id="item7">数组去重</font>

```
(()=>{
    var myArray = ['a', 'b', 'a', 'b', 'c', 'e', 'e', 'c', 'd', 'd', 'd', 'd'];
    var res = myArray.reduce((acc,cur)=>{
        if(!acc.includes(cur) ){    //acc.indexOf(cur) == -1 
            acc.push(cur)
        }
        return acc
    },[])
    return res  // ["a", "b", "c", "e", "d"]
})()
(()=>{
    var myArray = [1,2,1,2,3,5,4,5,3,4,4,4,4];
    var res = myArray.sort((a,b)=>a-b).reduce((acc,cur)=>{
        if( acc.length ===0 || acc[acc.length - 1] !== cur){
            acc.push(cur)
        }
        return acc
    },[])
    console.log( res ) // [1, 2, 3, 4, 5]
})()
```
