### This

#### 规律

> 在浏览器平台下运行 JS ，非函数当中的this 一般都指向 window。
> 因此这里讨论的是函数执行过程中的 this
> 需要注意在 ES6+ 的箭头函数中是没有自己this的，处理机制是使用自己上下文里的 this

通俗来说, 谁调用它, this就是谁, 它是当前函数的执行主体 (谁执行了函数), 但是不等同于执行上下文, 作用域

举例子:

bruce在肯德基吃饭

1. 吃饭是一个动作, 对应代码里的函数
2. 肯德基是一个环境, 对应着代码里的执行上下文
3. bruce是个主体, 对应本次动作的执行者, 也就是this

#### this 练习

[代码分析](https://www.processon.com/diagraming/60d96ab97d9c087f5477c624)

```js
(function () {
  console.log(this)
})()

let arr = [1, 3, 5, 7]
obj = {
  name: '拉勾教育'
}
arr.map(function (item, index) {
  console.log(this)
}, obj)
------------------------------------------------------
//? 普通函数调用
let obj = {
  fn: function () {
    console.log(this, 111)
  }
}
let fn = obj.fn;
fn()  // window
obj.fn();  // obj
(10, fn, obj.fn)();
------------------------------------------------------
var a = 3, 
  obj = { a: 5 }
obj.fn = (function () { 
  this.a *= ++a
  return function (b) {
    this.a *= (++a) + b
    console.log(a)
  }
})();
var fn = obj.fn  
obj.fn(6)
fn(4)
console.log(obj.a, a)
```

## 

