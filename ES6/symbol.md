### symbol

一个独一无二的值, 创建的值都是唯一的

* 防止对象属性名重复的问题

  ```
  const a = Symbol()
  const b = Symbol()
  const obj = {
  	[a]: 'value1',
  	[b]: 'value2',
  }
  console.log('obj[a]', obj[a])
  ```

* 实现对象私有成员

  ```
  const name = Symbol()
  const person = {
  	[name]: 'zce',
  	say() {
  		console.log(this[name])
  	}
  }
  ```

* 全局复用一个相同的Symbol值

  * 全局变量

  * Symbol.for()

    ```
    const s1 = Symbol.for('foo')
    const s2 = Symbol.for('foo')
    console.log(s1 === s2) // true
    console.log(Symbol.for(true) === Symbol.for('true')) // true
    ```

  * 自定义toString标签[object object]

    ```
    const obj = {
    	[Symbol.toStringTag]: 'Xobject'// for in、Object.keys、JSON.stringify(obj) 无法获取
    }
    console.log(obj.toString()) // [object Xobject]
    console.log(Object.getOwnPropertySymbols(obj)) //能获取Symbol.toStringTag
    
    ```

    