---
title: 记录一些小技巧
categories: other
tags: [小技巧] #文章标签，可空，多标签请用格式，注意:后面有个空格
top: true
---

## vue

### 页面 v-for 中修改 item 属性值后页面 v-if 不改变的问题

> 添加 this.$forceUpdate();进行强制渲染，效果实现

---

## git

### git reset

> --hard : 回退版本库，暂存区，工作区。（因此我们修改过的代码在本地就没了）<br>
> --mixed: 回退版本库，暂存区。(--mixed 为 git reset 的默认参数，即当任何参数都不加的时候的参数)<br>
> --soft: 回退版本库。

---

## js

### Math.round/ceil/floor

```javascript
Math.round(); // 四舍五入
Math.ceil(); // 向上取整
Math.floor(); // 向下取整
```

### promise

[promise](/2020/12/01/js-promise/)

---

## 手机

> 安卓手机文本框获取焦点后，键盘把文本框挡住了

```
window.addEventListener("resize", function() {
    if (
        document.activeElement.tagName === "INPUT" ||
        document.activeElement.tagName === "TEXTAREA"
    ) {
        if ("scrollIntoView" in document.activeElement) {
            document.activeElement.scrollIntoView();
        } else {
            document.activeElement.scrollIntoViewIfNeeded();
        }
    }
})
```
> iphone x 高度:100%; 兼容设置
1. 让客户端设置webview的高度为100%;
2. html代码里面添加 viewport-fit=cover ，这个ios11的新属性可以使得页面内容完全覆盖整个窗口
相关代码：
```
<meta name="viewport" content="width=device-width, viewport-fit=cover">
```

## 浏览器

> 浏览器是`60Hz`的刷新率，每`16ms`才会更新一次

## 面试题

1. vue key 作用 diff 算法
2. vue 组件传参
3. [实现 promise.allsettled](/2020/12/01/js-promise/)
4. css 垂直水平居中
5. webpack 相关
6. 闭包
7. [原型链](/2020/11/16/js原型/)
8. [commonjs 和 es6 模块的区别](/2020/12/02/commonjs-es6Module/)
9. 浏览器缓存
10. 首屏加载
11. 新需求下来，要做哪些准备
12. babel webpack
13. webpack 的 plugin 跟 loader
