# 虚拟dom

> 用普通的js对象来描述DOM对象

创建虚拟dom开销比创建真实dom小很多

## 为什么要使用Virtual DOM

* MVVM 框架解决视图和状态同步问题
* 模版引擎可以简化视图操作, 没办法跟踪状态
* 虚拟DOM跟踪状态变化, 使用算法找出和上一次状态不同的DOM, 只更新不同状态的DOM

## Virtual DOM作用

*  维护视图和状态的关系
* 复杂视图情况下提升渲染性能
* 跨平台
  * 浏览器平台渲染DOM
  * 服务端渲染SSR(Nuxt.js/Next.js)
  * 原生应用 (Weex/React Native)
  * 小程序

## Virtual DOM Library

* Snabbdom
* virtual-dom

## snabbdom源码

