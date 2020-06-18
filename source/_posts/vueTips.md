---
title: vue的一些小技巧
categories: vue
tags: [vue] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: forceUpdate、
---

### 页面v-for中修改item属性值后页面v-if不改变的问题
> 添加this.$forceUpdate();进行强制渲染，效果实现

