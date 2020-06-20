---
title: css预处理器 - sass #文章页面上的显示名称，一般是中文
categories: html+css #分类
tags: [css,sass] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: css预处理器主要是sass的相关操作
---

> css预处理器包含：sass, lass
> sass/less区别：less基于javascript通过客户端处理，sass基于Ruby语言开发通过服务端处理，所以less慢于sass



## 保持sass条理性和可读性最基本三个方法：[嵌套](#nest)、[导入](#import)、[注释](#annotation)
### 变量
> 变量跟es6中的let一样具有块级作用域，必须先定义后使用。 **less变量用的是<font color=#ec7259>@</font>**
```
$color:red;
$fontSize:16px;
$color:blue;
body{   
    background:$color;    //blue    使用前最后一处声明有效，前边的值被覆盖
    font-size:$fontSize
}
$color:green;
```
### <font id="nest">嵌套</font>
例1
```
.data{
    & > ul{                       //.data > ul       
        .li{
            &:hover{}            //.data ul .li:hover        &：父选择器的标识符
            body.id & {}         //body.id .data >ul .li
            &-title{}            //.data ul .li-title{}
        }
    }
    h1,h2{}        //.data h1, .data h2{}          //群组选择器
}
```
例2        嵌套属性
```
.nav{
    border: {
        style:solid;
        width:1px;
        color:red;
    }
}    ==>.nav{border:1px solid red;}
```
例3
```
.nav{
    border: 1px solid #ccc {
      left: 0px;
      right: 0px;
  }
}        ==>.nav{border: 1px solid #ccc; border-left:0px;border-right:0px;}
```
### <font id="import">导入</font>
> css中的import跟sass中的import的区别：
> css @import:在css文件中执行到@import时浏览器去下载css文件=》页面加载慢
> sass @import:生成css文件时把文件导入进来，跟原有文件合成一个文件=》无需发起额外请求下载

> 局部文件：以下划线开头，不生成css文件，只用来被引用，可被多文件引用
> 例：想导入style/_night-sky.scss    =>@import "style/night-sky"

> 导入文件又要修改文件中的全局变量：**<font color=#ec7259>!default</font>**
```
$c:pink;
@import 'src/assets/css/ab';
_ab.scss:
  $c: green !default;
  .cont {color: $c;}
```
### <font id="annotation">注释</font>
> .a{}    /*这种注释内容会出现在生成的css文件中*/
> .b{}    //这种注释不会出现在生成的css文件中，叫做：静默注释
> .c{color/*这块注释也不会出现在生成的css文件中*/ :red;}   

### 混合器
```
@mixin background($color) {
    background: $color;
}
body{
    @include background(green);
}
```
例2：
```
@mixin link-colors($normal:blue, $hover:red, $visited:green) {    //声明
    color: $normal;
    &:hover { color: $hover; }
    &:visited { color: $visited; }
}
@include link-colors( $normal: blue, $visited: green, $hover: red);    //调用1
@include link-colors( $hover: yellow);    //调用2
@include link-colors( green);    //调用3
```
> **less:声明：<font color=#ec7259>.className{}</font> 调用：<font color=#ec7259>.className()</font>**

### 继承
```
.redC{
    background:red;
    margin:10px;
}
body{
    @extend .redC;
}
```
> 混合器主要用于展示性样式的重用，而类名用于语义化样式的重用

### 操作符
> +、-、*、/、%、calc()、


#### sass中的小技巧
> 在有引号的文本字符串中使用 #{} 插值语句可以添加动态的值。使用 #{}可以避免 Sass 运行运算表达式，直接编译 CSS

例1：
```
p:before {content: "I ate #{5 + 10} pies!";}=>p:before { content: "I ate 15 pies!"; }
```
例2
```
@mixin mar($dir,$size){margin-#{$dir} : $size;}
.cont{@include mar('left',20px)}
```

#### @for循环
> @for $i from 开始值 throuth 结束值（包括结束值）
> @for $i from 开始值 to 结束值（不包括结束值）
```
@for $i from 1 throuth 3{
    .item-#{$i}{width:(20px * $i)}
}
==> .item-1{width:20px}
    .item-2{width:40px}
    .item-3{width:60px}
```





