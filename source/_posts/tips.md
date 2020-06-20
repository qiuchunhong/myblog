---
title: 记录一些小技巧
categories: vue
tags: [vue] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: forceUpdate、git reset、
---

## vue
### 页面v-for中修改item属性值后页面v-if不改变的问题
> 添加this.$forceUpdate();进行强制渲染，效果实现

## git 
### git reset 
> --hard : 回退版本库，暂存区，工作区。（因此我们修改过的代码在本地就没了）<br> 
> --mixed: 回退版本库，暂存区。(--mixed为git reset的默认参数，即当任何参数都不加的时候的参数)<br> 
> --soft: 回退版本库。

## js
### js (v,k)||(k,v)总结
> ary.forEach((v,k)=>{})