### Proxy

```
const person = {}
person.name = 'bruce'
person.age = 24

const personProxy = new Proxy(person, {
	get(target, property) {
		console.log(target[property])
	},
	set(target, property, value) {
		target[property] = value
	}
})
personProxy.name
// bruce
personProxy.weiget = '120kg'
personProxy.weiget
// 120kg

```

#### Proxy对比defineProperty

* 监听的范围更广

  ![image-20211105102317452](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211105102317.png)

* 重写数组的操作方法

* 对数组的监听

  ```
  let list = []
  const listProxy = new Proxy(list, {
  	set (target, property, value) {
  		console.log('set', property, value)
  		target[property] = value
  		return true // 表示设置成功
  	}
  })
  listProxy.push(100)
  ```

* 非侵入的接管了整个对象, 不需要对数组进行任何操作

