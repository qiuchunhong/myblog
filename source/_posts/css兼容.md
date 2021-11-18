---
title: css兼容问题
categories: css
tags: [css兼容] #文章标签，可空，多标签请用格式，注意:后面有个空格
summary: 
---

### css的选择符
1. 通配符选择器(*)
2. id选择器
3. 类选择器
4. 标签选择器(div,h1,p)
5. 后代选择器(h1 p)
6. 相邻后代选择器(ul>li)
7. 兄弟选择器(li ~a)
8. 相邻兄弟选择器(li+a)
9. 属性选择器(a[rel="ab"])
10. 伪元素选择器(a::before\::after\::firstLetter\::firstLine\::selection) 【伪类、伪元素详细列表】
11. 伪类选择器：链接伪类(:link\:visited\:target)、动态伪类(:hover\:active) LVHA、表单相关伪类(:enabled\:disable\:checked\:focus)、结构性伪类(:nth-child(2n)\:first-child\:last-child\nth-last-child(index)\:only-child\:nth-of-type(index)\:first-of-type\:last-of-type\nth-last-type(index)\:only-of-type\:not\:empty)

### input disabled后手机端颜色比设置的浅
```css
input:disabled, input[disabled]{
    color: red;
    -webkit-text-fill-color:red;
    -webkit-opacity:1;
    opacity: 1;
}
```
### 一个文字渐变效果
```css
.text-fill-color{
    background:-webkit-linear-gradient(top,#eee,#aaa 50%,#333 51%,#000);
	-webkit-background-clip:text;
	-webkit-text-fill-color:transparent;
	font:bold 95px/1.231 georgia,sans-serif;
	text-transform:uppercase;
}
<div class="text-fill-color">text-fill-color</div>
```
![一个文字渐变效果](./gradient.png)

### 文本超出省略号显示
> 单行文本显示省略号
```
.ellipsis{
	width: 500px;	/* 设置宽度，不设置默认为100% */
	white-space: nowrap;	/* 强制文本在一行显示 */
	overflow: hidden;	/* 隐藏溢出内容 */
	text-overflow: ellipsis;	/* 溢出进行省略 */
}
```
> 多行文本显示省略号 方法一：私有属性
```
.ellipsis2{
	height: 120px;
	line-height: 30px;
	overflow: hidden;
	display: -webkit-box;
	-webkit-box-orient: vertical;	/* 朝向：垂直 */
	-webkit-line-clamp: 4;
}
```
> 多行文本显示省略号 方法二：在最后面加...
```
.ellipsis3{
	height: 120px;
	line-height: 30px;
	overflow: hidden;
	position: relative;
	padding-right: 1em;
	text-align: justify;
}
.ellipsis3::before{
	content: '...';
	position: absolute;
	bottom: 0;
	right: 0;
}
.ellipsis3::after{
	content: '';
	width: 1em;
	height: 2em;
	background-color: red;
	position: absolute;
	display: inline;
	right: 0;
}
```
> 多行文本显示省略号 方法二：在最后面加渐变
```
.ellipsis4{
	height: 120px;
	line-height: 30px;
	overflow: hidden;
	position: relative;
}
.ellipsis4::after{
	content: "";
	width: 20%;
	height: 30px;
	background:linear-gradient(to right,rgba(255,255,255,0),#fff 80%);
	position: absolute;
	bottom: 0;
	right: 0;
}
```
![文本超出省略号显示效果图](./ellipsis.png)
