---
title: js继承、class
tags: [js] #文章标签，可空，多标签请用格式，注意:后面有个空格
summary: 继承是为了子类可以使用父类的所有功能，并且能对这些功能进行扩展。
---

## 继承
> 下面代码中，b是B类的实例，它的constructor方法就是B类原型的constructor方法。
```
class B {}  // === function B(){}
let b = new B();

b.constructor === B.prototype.constructor // true
```

> 由于类的方法都定义在prototype对象上面，所以类的新方法可以添加在prototype对象上面。Object.assign方法可以很方便地一次向类添加多个方法。
```
// es5写法
function U(){}
U.prototype.say = function(){console.log('hi')}
let u = new U()
u.say()
// es6写法
class P{}
Object.assign(P.prototype,{say(){console.log('hello')}})
let p = new P()
p.say()
```

**实例1**
```
class Point {
  constructor(x,y){
    this.x = x;
    this.y = y;
  }
  sum(){
    return this.x+ this.y
  }
  static hello() {
    console.log('hello world');
  }
}

class ColorPoint extends Point{
  constructor(x,y,color){
    // this.color = color; // ReferenceError
    super(x,y)
    this.color = color; // 正确
  }
  say(){
    console.log('x='+this.x +', y='+this.y+', color='+this.color+', x+y='+this.sum())
  }
}
let a  = new ColorPoint(12,3,'green')
a.say();  //x=12, y=3, color=green, x+y=15
console.log( a instanceof ColorPoint );  // true
console.log( a instanceof Point ); // true
ColorPoint.hello(); // hello world

```
**解析：**
- super()必须写到子类的constructor中的最上方。只有调用super之后，才可以使用this关键字，否则会报错。这是因为子类实例的构建，基于父类实例，只有super方法才能调用父类实例。如果子类没有定义constructor方法，这个方法会被默认添加，代码如下
```
class ColorPoint extends Point {
}
// 等同于
class ColorPoint extends Point {
  constructor(...args) {
    super(...args);
  }
}
```
> - super作为函数时，super()只能用在子类的构造函数之中，用在其他地方就会报错。
> - super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。

```
class A {
  constructor() {
    this.x = 1;
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
    super.x = 3;
    console.log(super.x); // undefined
    console.log(this.x); // 3
  }
}
let b = new B();
```
上面代码中，super.x赋值为3，这时等同于对this.x赋值为3。而当读取super.x的时候，读的是A.prototype.x，所以返回undefined。

- 父类的静态方法，也会被子类继承。hello()是A类的静态方法，B继承A，也继承了A的静态方法。