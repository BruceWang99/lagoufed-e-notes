### for of

* 可以使用break终止
* 可以作为遍历所有数据结构的统一方式(实现了Iterator接口就行)
* 数组、伪数组、Set、Map、Object



#### 迭代器(Iterator)

![image-20211105141225576](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211105141225.png)

![image-20211105141248636](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211105141248.png)

#### 对象实现可迭代接口(Iterable)

![image-20211105141738275](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211105141738.png)

#### 迭代器模式(设计模式)

统一处理迭代的逻辑部分

![image-20211105142119944](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211105142119.png)

#### generator(生成器函数)

惰性执行

```
function * foo {
	yield 100
	yield 200
}
const generator = foo()
generator.next()
generator.next()
generator.next()
```



#### 遍历对象

```
const obj = {
	a: 1,
	b: 2,
	c: 3
}
for(const [key, value] of Object.entries(obj)){
	console.log(key, value)
}
```

