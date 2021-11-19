### js创建对象3种方式对比

> JavaScript中创建一个对象可以采用3种方式：
> `new Object()`,`Object.create()`,或字面量写法。

#### new Object()

new Object()这种方式即我们常说的“使用构造函数创建对象”，[new](https://link.zhihu.com/?target=https%3A//developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new)运算符实际做了以下4件事情：
1）创建一个空的javascript对象；
2）链接该对象（即设置该对象的constructor）到另一个对象；
3）将步骤1中新创建的对象作为this上下文；
4）如果该函数没有自己的返回对象则返回this。

#### Object.create()

ES5新增的**`Object.create()`**方法创建一个新对象，使用现有的对象（传入的第一个参数）来作为新创建对象的原型。(即将创建的新对象的__proto__指向已存在对象的原型)

其作用（只传1个参数情况下）与道格拉斯·克罗克福德的“原型式继承”函数相同：（JavaScript高级程序设计P170）

```js
function object(o) {
  function F () {} // 先创建一个临时性的构造函数
  F.prototype = o // 然后将传入的对象作为这个构造函数的原型
  return new F() // 最后返回了这个临时类型的一个新实例
}
```

#### 对象字面量{…}

使用花括号`{...}`快速创建对象。

* 以字面量方式创建的对象属性默认是可写，可枚举和可配置的
* 对象的原型默认为Object.prototype