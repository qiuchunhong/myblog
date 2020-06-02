---
title: vue directive 指令
categories: vue
tags: [vue] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: directive的使用以及记录指令的场景、实现
---

### 为什么要使用 VueDirective

在 Vue 实例的方法中写 DOM 操作不可以吗？为什么使用 Vue Directive 来封装 DOM 操作呢？
为了实现 View 和 ViewModel 的分离，我们必须封装 DOM 操作，View 层负责页面上的显示，ViewModel 层负责改变操作数据，由于 Vue 是数据驱动的，属于 ViewModel 层，那么其中就不应该出现 View 层上的 DOM 操作，而且，使用 Vue Directive 是和 DOM 元素的创建、销毁绑定的。Vue Directive 的生命周期方法能让我们更优雅的去在合适的时机进行 DOM 的操作。而在 ViewModel 里则没有和 DOM 元素对应的方法。因为 Vue Directive 是属于 View 层面的，所以 DOM 操作应该被封装在 Vue Directive 里而不是出现在 Vue 实例中。

[基础使用](https://github.com/qiuchunhong/vueProject/blob/master/src/views/directives/index.vue)

### 业务场景

> - 某个按钮按下去之前需要先判断它是否登陆，如果没有登陆需要跳转到对应的登陆页面，否则就继续该按钮之后的操作。

**store 需要 import 进来**

```
const directive = Vue.directive('permission-click', {
  bind: (el, binding, vnode) => {
    el.addEventListener('click', (e) => {
      if (!store.getters.isLogin) {
        store.dispatch('showLogin')
      } else {
        typeof binding.value === 'function' && binding.value()
      }
    })
  }
})
directives: {
    permissionClick: {
        bind (el, binding, vnode) {
            el.addEventListener('click', (e) => {
                if (!store.getters.isLogin) {
                    store.dispatch('user/showLogin')
                } else {
                    typeof binding.value === 'function' && binding.value()
                }
            })
        }
    }
}
```

> - v-loading 全局加载

```
<div v-loading="loading"></div>
<div @click="showLoading">change loading status</div>
methods: {
    showLoading () {
        this.loading = true
        let timer = setTimeout(() => {
            this.loading = false
            // clearTimeout(timer)
        }, 1000)
    }
},
 Vue.directive('loading', {
        update(el, binding, vnode) {
          if (binding.value) {
            const div = document.createElement('div')
            div.innerText = '加载中...'
            div.setAttribute('id', 'loading')
            div.style.position = 'absolute'
            div.style.left = 0
            div.style.top = 0
            div.style.width = '100%'
            div.style.height = '100%'
            div.style.display = 'flex'
            div.style.justifyContent = 'center'
            div.style.alignItems = 'center'
            div.style.color = 'white'
            div.style.background = 'rgba(0, 0, 0, .7)'
            document.body.append(div)
          } else {
            document.body.removeChild(document.getElementById('loading'))
          }
        }
      })
```

> - 根据路由角色控制 DOM **(一般是按钮多)** 是否显示

```
 bind: function (el, binding, vnode) {
  // 获取按钮权限  路由中 meta 中的角色
  let btnPermissions = vnode.context.$route.meta.btnPermissions.split(",");
  if (btnPermissions.indexOf('admin')<0) {
  	// 不是admin 就不显示
  	// 当然此处也可以 将 el 样式隐藏也行
   el.parentNode.removeChild(el);
  }
}
```

> - 根据后台下发 权限 字段，控制当前 DOM 是否展示

```
 // 很多时候，服务器下发了  showBtn 字段就代表 显示某个 按钮，前端一般把控制权限的数据都放在内存中
 // 这样写 在 bind update 都会执行
Vue.directive('check-permission', function (el, binding) {
  var success = checkPermission(binding.value); // 拿到绑定的值
  if(!success){
    el.style.display = 'none';
  }
})

function checkPermission(permissionKey) {
  var values = localStorage.getItem("userPermissions");
  if (!values) {
    console.log('can not find permissions config from localstorage, function access will be denied.');
    return false;
  }
  var ret = values.split(',').find(item => item === permissionKey) != null;
  console.log('===========check permission, key:' + permissionKey + ", result:" + ret + "==========");
  return ret;
}

// 使用
v-check-permission="'showBtn'"
```
