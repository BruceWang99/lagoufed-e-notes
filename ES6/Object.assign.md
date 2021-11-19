### Object.assign

> 用后面对象的属性去覆盖目前对象的属性, 有就覆盖,没有就添加, 返回一个新的对象

```
const source1 = {
	a: 111,
	b: 222
}
const source2 = {
	b: 893,
	d: 124
}
const target = {
	a: 345,
	C:567
}
const result = Object.assign(target, source1, source2)
console.log(result)
```

