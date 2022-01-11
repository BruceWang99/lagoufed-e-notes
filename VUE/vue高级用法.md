# Vue高级用法

## 自定义指令

> 主要用于对普通DOM元素进行底层操作

```javascript
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 只调用一次，指令第一次绑定到元素时调用。
  bind: function (el) {
    
  }
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
  // 所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。
  update: function (el) {
    
  }
  // 只调用一次，指令与元素解绑时调用。
  unbind: function (el) {
    
  }
})
```

## 混入

> 全局注册一个混入，影响注册之后所有创建的每个 Vue 实例。插件作者可以使用混入，向组件注入自定义的行为。

```javascript
// 为自定义的选项 'myOption' 注入一个处理器。
Vue.mixin({
  created: function () {
    var myOption = this.$options.myOption
    if (myOption) { // 这个判断是为了不让混入影响每个 Vue 实例, 只有实例中有myOption这个属性, 才做某一些操作
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello!'
})
// => "hello!
```

## vue的插件

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

## render函数

> 用于更灵活的编程体验, 直接使用Javascript来编写页面时好用

```javascript
Vue.component('anchored-heading', {
  render: function (createElement) {
    return createElement(
      'h' + this.level,   // 标签名称
      attrs
      this.$slots.default // 子节点数组
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})
```



