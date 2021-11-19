### JS性能优化

#### JSBench工具使用 https://jsbench.me/

> 分析js的执行效率

#### 变量局部化

* 离我们使用的地方越近, 越好: 减少了数据访问时需要查找的路径
* 数据的存储和读取

#### 缓存数据

#### 减少访问层级

#### 节流和防抖

**场景**:

*  滚动事件
* 输入的模糊匹配
* 轮播图切换
* 点击操作

##### 防抖

> 对于高频操作来说,我们只希望识别一次点击, 可以是第一次或最后一次

```
function myDebounce(handle, wait, immediate) {
			 if (typeof handle !== 'function') throw new Error('handle must be an function')
			 if( typeof wait === 'undefined') wait = 300
			 if( typeof wait === 'boolean') {
				immediate = wait
				wait = 300
			 }
			 if(typeof immediate !== 'boolean') immediate = false
			 // 管理 handle 的执行次数
			 // 如果我们想要执行最后一次, N-1次都无用
			 let timer = null
			 return function proxy(...args) {
				console.log('timer', timer);
				 let self = this, init = immediate && !timer
				clearTimeout(timer)
				timer = setTimeout(()=>{
					timer = null //清空timer, init重置
					!immediate ? handle.apply(self, args) : null
				 }, wait)
				 // 如果init是true, 需要立即执行
				 init ? handle.apply(self, args) : null
			 }
		 }
```



##### 节流

> 对于高频操作来说, 不希望它的频率那么高, 降低操作的频率

```
		function myThrottle(handle, wait) {
			if (typeof handle !== 'function') throw new Error('handle must be an function')
			if(typeof wait === 'undefined') wait = 400
			let previous = 0 // 记录上一次执行的时间
			let timer = null
			return function proxy(...args) {
				let now = new Date() // 当前时间
				let interval = wait - (now - previous)
				let self = this
				if(interval <= 0 ){ // 非高频
					handle.apply(self, args)
					previous = new Date()
					clearTimeout(timer)
				    timer = null
				} else if (!timer) { // 高频操作, 在我们定义的频次内, 只执行一次
					timer = setTimeout(() => {
						clearTimeout(timer)
						timer = null
						handle.apply(self, args)
						previous = new Date()
					}, interval)
				}
			}
		}
```



#### 减少层级嵌套

#### 减少循环体活动

* 不改变的值或操作放到循环外面定义
* 从后往前遍历, 少做条件判断

#### 字面量与构造式

* 字面量的形式创建比构造函数形式快(构造函数做的事情多)

