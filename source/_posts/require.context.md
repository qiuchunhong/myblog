---
title: require.context()
categories: Webpack
tags: [Webpack, require.context]
summary: 一个webpack的api,通过执行require.context函数获取一个特定的上下文,主要用来实现自动化导入模块,在前端工程中,如果遇到从一个文件夹引入很多模块的情况,可以使用这个api,它会遍历文件夹中的指定文件,然后自动导入,使得不需要每次显式的调用import导入模块
---

语法：
```javascript
require.context(directory, useSubdirectories = true, regExp = /^\.\/.*$/, mode = 'sync');
```
示例：
```javascript
require.context('./test', false, /\.test\.js$/);
//（创建出）一个 context，其中文件来自 test 目录，request 以 `.test.js` 结尾。
```
```javascript
require.context('../', true, /\.stories\.js$/);
// （创建出）一个 context，其中所有文件都来自父文件夹及其所有子级文件夹，request 以 `.stories.js` 结尾。
```
## context module API 
一个 context module 会导出一个（require）函数，此函数可以接收一个参数：request。

require.context函数接受三个参数
- directory {String} - 要搜索的目录
- useSubdirectories {Boolean} - 是否还搜索其子目录
- regExp {RegExp} - 匹配文件的正则

此导出函数有三个属性：resolve, keys, id。
- resolve 是一个函数，它返回 request 被解析后得到的模块 id。
- keys 也是一个函数，它返回一个数组，由所有可能被此 context module 处理的请求组成。【由所有符合上下文模块处理的请求组成。 】
- id 是 context module 的模块 id【 是上下文模块里面所包含的模块 id】. 它可能在你使用 module.hot.accept 时会用到。

> 注意：传递给 require.context 的参数必须是字面量(literal)！【不能是变量，否则在编译阶段无法定位目录】


## 实例：组织路由
1. 分割路由
首先为了方便我们管理，我们把router目录下的文件分割为以下结构
```base
router                           // 路由文件夹
  |__index.js                    // 路由组织器：用来初始化路由等等
  |__common.js                   // 通用路由：声明通用路由
  |__modules                     // 业务逻辑模块：所以的业务逻辑模块
        |__index.js              // 自动化处理文件：自动引入路由的核心文件
        |__home.js               // 业务模块home：业务模块
        |__a.js                  // 业务模块a
```

2. modules文件夹中处理业务模块

我们通过上面提到的require.context()接下来编写自动化的核心部分index.js
```javascript
import Vue from "vue";
import VueRouter from "vue-router";
Vue.use(VueRouter);

let routeOptions = []
let files = require.context(".", false, /\.js$/);
files.keys().forEach(key=>{
  if(key == './index.js') return ;
  routeOptions = routeOptions.concat(files(key).default)
})
const routes = routeOptions.map(route=>{
  if(!route.component){
    route = {
      ...route,
      component:()=>import(`@/views/${route.filePath}`),
      meta:{
        ...route.meta
      }
    }
  }
  return route
})
const router = new VueRouter({
  mode: "history",
  routes,
  scrollBehavior(){
    return{x:0,y:0}
  }
})
export default router

```

其他模块模板
```javascript
export default [
  {
    path: "/activityFroms",
    name: "activityFroms",
    filePath: "activity/activityFroms.vue"
  }
];
```


