# vue 响应式原理

## 数据驱动

### 数据响应式

* 数据改变时, 视图会进行更新, 避免了繁琐的DOM操作, 提高了开发效率

### 双向绑定

* 数据改变, 视图改变;视图改变, 数据也随之改变
* 使用v-model在表单元素上创建双向数据绑定

### 数据驱动是Vue最独特的特性之一

* 开发中, 只需要关注数据本身, 不需要关心数据是如何渲染到视图的

## vue2的响应式的原理

* [vue2深入响应式原理官方文档](https://cn.vuejs.org/v2/guide/reactivity.html)
* [Object.defineProperty() MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

```javascript
// 多个属性的get set
		// 代理的数据
		let data = {
			msg: 'hello'
		}
		let vm = {} // vue实例
    
		proxyData(data)

		// 代理数据
		function proxyData(data) {
			Object.keys(data).forEach((key)=> {
				Object.defineProperty(vm, key, {
					enumerable: true, // 该属性才会出现在对象的枚举属性中
					configurable: true,// 可描述, 可被删除, 可被defineProperty修改
					get() {
						console.log('get', key);
						return data[key]
					},
					set(newVal) {
						console.log('set', key);
						if(data[key] === newVal) return
						data[key] = newVal
						document.getElementById('app').textContent = data[key]
					}
				})
			})
			
		}
```

## vue3的响应式的原理

* [ProxyMDN文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

```javascript
// 多个属性的get set
		// 代理的数据
		let data = {
			msg: 'hello',
			count: 1
		}
		// vue实例
		let vm = new Proxy(data, {
			// target目标对象。property被获取的属性名。receiverProxy或者继承Proxy的对象
			get: function(target, property, receiver) {
				return target[property]
  			},
			set: function(target, property, value, receiver) {
				if(value === target[property]) return
				target[property] = value
				document.getElementById('app').textContent = target[property]
			},
		})
		vm.msg = 'world'
```

## 发布/订阅模式

> 三个角色: 订阅者、发布者、信号中心. 就像微信公众号的订阅号, 订阅号的作者发布文章, 微信用户订阅公众号, 微信app就是这个信号中心. 作者写好文章后, 发布到微信, 订阅了这个公号的用户接受文章后阅读

### vue的自定义事件

* vue自定义事件实现

```javascript
	// 事件触发器
		class EventEmitter {
			constructor() {
				// {'click': [fn, fn2]}
				this.subs = Object.create(null)
			}
			$on (eventType, handler) {
				this.subs[eventType] = this.subs[eventType] || []
				this.subs[eventType].push(handler)
			}

			$emit(eventType) {
				if(this.subs[eventType]) {
					while(this.subs[eventType].length) {
						this.subs[eventType].shift()()
					}
					
				}
				
			}
		}
		let em = new EventEmitter()
		em.$on('datachange', ()=>{
			console.log('datachange');
		})
		em.$on('datachange', ()=>{
			console.log('datachange2');
		})
		em.$emit('datachange')
```

## 观察者模式

> 和发布订阅模式的区别: 没有信息中心

```javascript
		// 发布者 - 目标
		class Dep {
			// 记录所有订阅者
			constructor() {
				this.subs = []
			}
			// 添加订阅者
			addSub(sub) {
				if(sub && sub.update) {
					this.subs.push(sub)
				}
			}
			// 发布通知
			notify() {
				this.subs.forEach(sub => {
					sub.update()
				})
			}
		}
		// 订阅者 - 观察者
		class Watcher {
			update() {
				console.log('update');
			}
		}

		let dep = new Dep()
		let watcher = new Watcher()
		dep.addSub(watcher)
		dep.notify()
```

## 发布订阅模式和观察者模式区别

* 观察者模式由具体目标调度, 比如当事件触发, Dep 就会去调用观察者的方法,所以观察者模式的订阅者与发布者是存在依赖的
* 发布/订阅模式由统一调度中心调用, 因此发布者和订阅者不需要知道对方的存在

![image-20211229170615147](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211229170615.png)

理解例子:
发布订阅模式更像是房产中介,出租者给中介发布信息, 租客去订阅中介的信息, 做出相应决定

观察者模式更像是大房东直租, 大房东发布房屋信息, 租客订阅到信息后,做出相应决定

## vue响应式原理流程

![image-20211230193324151](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211230193324.png)