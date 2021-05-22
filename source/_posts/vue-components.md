---
title: vue中组件传参
categories: vue
tags: [vue, components] #文章标签，可空，多标签请用格式，注意:后面有个空格
summary: 父子组件相关、组件之间数据传递
---

## Vue 的父子组件生命周期钩子函数执行顺序
- 加载渲染过程

父 beforeCreate->父 created->父 beforeMount->子 beforeCreate->子 created->子 beforeMount->子 mounted->父 mounted

- 子组件更新过程

父 beforeUpdate->子 beforeUpdate->子 updated->父 updated

- 父组件更新过程

父 beforeUpdate->父 updated

- 销毁过程

父 beforeDestroy->子 beforeDestroy->子 destroyed->父 destroyed

