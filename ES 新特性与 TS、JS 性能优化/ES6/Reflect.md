### Reflect

一个静态类,封装了一系列对对象的底层操作, 现在可用的13个方法, 为操作对象提供了一个统一的标准API.

* 为什么要有Reflect

  以前判断一个属性是否在对象中

  ```
  const person = {
  	name: 'bruce',
  	age: 24
  }
  console.log('name' in person)
  ```

* 以前在对象中, 删除一个属性

  ```
  const person = {
  	name: 'bruce',
  	age: 24
  }
  console.log(delete person.age)
  ```

* 以前在对象中, 获取对象中所有的属性

  ```
  const person = {
  	name: 'bruce',
  	age: 24
  }
  console.log(Object.keys(person))
  ```

现在可以全部在Reflect里提供了统一的方法

[`Reflect.apply(target, thisArgument, argumentsList)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/apply)

对一个函数进行调用操作，同时可以传入一个数组作为调用参数。和 [`Function.prototype.apply()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 功能类似。

[`Reflect.construct(target, argumentsList[, newTarget\])`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/construct)

对构造函数进行 [`new` ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)操作，相当于执行 `new target(...args)`。

[`Reflect.defineProperty(target, propertyKey, attributes)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/defineProperty)

和 [`Object.defineProperty()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 类似。如果设置成功就会返回 `true`

[`Reflect.deleteProperty(target, propertyKey)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/deleteProperty)

作为函数的[`delete`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/delete)操作符，相当于执行 `delete target[name]`。

[`Reflect.get(target, propertyKey[, receiver\])`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/get)

获取对象身上某个属性的值，类似于 `target[name]。`

[`Reflect.getOwnPropertyDescriptor(target, propertyKey)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/getOwnPropertyDescriptor)

类似于 [`Object.getOwnPropertyDescriptor()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)。如果对象中存在该属性，则返回对应的属性描述符, 否则返回 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined).

[`Reflect.getPrototypeOf(target)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/getPrototypeOf)

类似于 [`Object.getPrototypeOf()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/GetPrototypeOf)。

[`Reflect.has(target, propertyKey)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/has)

判断一个对象是否存在某个属性，和 [`in` 运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/in) 的功能完全相同。

[`Reflect.isExtensible(target)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/isExtensible)

类似于 [`Object.isExtensible()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible).

[`Reflect.ownKeys(target)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys)

返回一个包含所有自身属性（不包含继承属性）的数组。(类似于 [`Object.keys()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys), 但不会受`enumerable影响`).

[`Reflect.preventExtensions(target)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/preventExtensions)

类似于 [`Object.preventExtensions()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)。返回一个[`Boolean`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Boolean)。

[`Reflect.set(target, propertyKey, value[, receiver\])`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/set)

将值分配给属性的函数。返回一个[`Boolean`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Boolean)，如果更新成功，则返回`true`。

[`Reflect.setPrototypeOf(target, prototype)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/setPrototypeOf)

设置对象原型的函数. 返回一个 [`Boolean`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Boolean)， 如果更新成功，则返回`true。`

