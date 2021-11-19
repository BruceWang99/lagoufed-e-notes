操作系统IO的几种处理方式?

https://blog.csdn.net/blogs_broadcast/article/details/90400730

js是基于异步式IO实现的异步编程?

#### 异步模式

执行栈 > 消息(回调)队列

Event loop(事件循环)会一直监听异步调用的任务, 将他们放入消息(回调)队列中或者把它放到执行栈中执行

* 常见的异步模式有:发布/订阅, 时间监听, 回调函数,Promise

![image-20211101094427216](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211101094427.png)

![image-20211101094542968](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211101094543.png)

#### 回调函数

> 一件你想做的事情

由调用者定义, 执行者执行的函数

##### Promise

###### Promise.prototype.then()

* then 会返回一个全新的Promise
* 后面的then是为上一个then返回的Promise注册回调
* 前面then方法中回调函数的返回值会作为后面then方法回调的参数
* 如果回调中返回的是Promise, 那后面的then方法的回调会等待它的结束

###### 异常处理

Promise.prototype.catch()是.then(null, onRejected)的别名, 用于指定错误的回调函数

###### 静态方法

* Promise.resovle()
  * 参数是一个Promise实例时, 直接原封不动返回这个实例
  * 参数是一个有then方法的对象时, 将这个对象转化为Promise对象,并且立即执行then方法
  * 参数是没有then的对象或者不是对象时, 返回一个状态为Resolved的新的Promise对象,并且立即执行回调函数,回调函数的参数是传入的参数
  * 不带任何参数时,  返回一个状态为Resolved(fulfilled)的新的Promise对象
* Promise.reject()
  * 返回一个新的Promise实例, 状态是Rejected

###### 并行执行

静态方法 Promise.all([p1,p2,p3]) // 等待所有的Promise完成
静态方法 Promise.race**([p1,p2,p3]) // 等待第一个Promise完成, 可以用做接口超时的控制

###### 执行时序/宏任务 vs 微任务

* 宏任务和微任务一般只指回调队列中执行的任务
* 回调队列任务执行过程中, 先优先微任务,再执行宏任务
* 常见的微任务有: Promise.then(),Object.observe,MutationObserver, process.nextTick(Node.js环境)
* 常见的宏任务: 除了微任务,大多数是宏任务,有setTimeout、setInterval、UI交互事件、postMessage、script
* 顺序: 同步任务 > 异步任务(微任务 > 宏任务)

#### Generator异步方案

* 星号 *function(){}, 调用Generator函数会返回一个内部指针(即遍历器、生成器对象) g

* yield命令: 遇到yield命令就暂停, 等到执行完yield后面的代码, 再往后面走

  * let y = yield x+1, y是yield后面的代码执行结果

* g.next()

  * 参数: 如果没有对应的yield任务, 参数将会做为上个阶段异步任务的返回结果

  * 返回值(例:{value: 3, done: false} ) , 
    * value: 是yield后面代码执行的结果, 如果没有可执行的代码,就是undefined
    * done: 一个函数是否执行完毕的状态

* 错误处理

  * g.throw('出错了')
  * 函数体内的try ... catch.. 代码块捕获

* co 递归封装, 实现Async await

  ```javascript
  function co(generator) {
    let g = generator();
    function handleResult (result) {
      if(result.done) return;
      result.value.then(data=>{
        console.log('data', data);
        handleResult(g.next(data))
      }, error => {
        g.throw(error)
      })
    }
    handleResult(g.next())
  }
  ```

  

#### Asyn函数(Generator语法糖)

* 类似Generator, 封装了一个co, 回调的递归

##### 发布订阅模式

> 当一个对象的状态发送改变时，所有依赖于它的对象都将得到状态改变的通知。

