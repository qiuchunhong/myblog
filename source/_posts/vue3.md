---
title: vue3
categories: vue
tags: [vue, vue3] #文章标签，可空，多标签请用格式，注意:后面有个空格
summary: 
---

## vue2.0和vue3.0的区别
1. 打包方式：
  2.0是通过:npm run dev
  3.0是：npm run serve
  至于为什么会变，来看一下package.json
2. 文件夹目录：
  3.0取消掉了config目录、build目录、static目录  ,还有最重要的一点，3.0的安装项目时自动下载node-model
  vue.config.js也没了，需要手动添加。
3. 创建项目方式：
  3.0的安装：vue create 3.0project；
  2.0的安装：vue init webpack(据我所知有五个，这个是我常用的)  2.0project(项目名)

## vue create 3.0project
```
 ◉ Choose Vue version
 ◉ Babel
 ◉ TypeScript
 ◯ Progressive Web App (PWA) Support
 ◉ Router
 ◉ Vuex
 ◉ CSS Pre-processors
 ◉ Linter / Formatter
 ◉ Unit Testing
❯◯ E2E Testing
```
Progressive Web App (PWA) Support PWA 支持。

CSS Pre-processors 支持 CSS 预处理器。

Linter / Formatter 支持代码风格检查和格式化。

Unit Testing 支持单元测试。

E2E Testing 支持 E2E 测试。

## vue 3.0 的 404 路由配置

```js
{
  path: "/:catchAll(.*)",
  name: "404",
  component:  () => import( "../views/404.vue"),
},
```
