# vue-router

## 原理

### history模式(更好看)

> 基于H5的history api实现

[history api](https://developer.mozilla.org/zh-CN/docs/Web/API/History_API#popstate_%E4%BA%8B%E4%BB%B6)

* pushState 改变浏览器的路由地址, 增加新的历史记录, 但是不会向服务端发送请求
* replaceState 替换当前的浏览器的路由地址, 不增加新的历史记录, 但是不会向服务端发送请求
* popstate 监听浏览器手动前进、后退点击, javascript操作history前进后退等

### Hash模式(兼容性更好)

> 基于window.onhashchange方法实现, 监听URL 中 # 后面的部分变化

```javascript
window.addEventListener("hashchange", funcRef, false);
```

### 基础原理

* vue的插件机制

  > vue的插件应该暴露一个 install 方法。这个方法的第一个参数是 Vue 构造器，第二个参数是一个可选的选项对象

  ```javascript
  MyPlugin.install = function (Vue, options) {
    // 1. 添加全局方法或 property
    Vue.myGlobalMethod = function () {
      // 逻辑...
    }
  
    // 2. 添加全局资源
    Vue.directive('my-directive', {
      bind (el, binding, vnode, oldVnode) {
        // 逻辑...
      }
      ...
    })
  
    // 3. 注入组件选项 (vue router走的就是这种)
    Vue.mixin({
      created: function () {
        // 逻辑...
      }
      ...
    })
  
    // 4. 添加实例方法, 原型链上, 实例都能获取到
    Vue.prototype.$myMethod = function (methodOptions) {
      // 逻辑...
    }
  }
  ```