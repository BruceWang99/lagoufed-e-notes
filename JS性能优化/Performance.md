### performance

内存监控

* 内存泄漏: 内存使用持续升高
* 内存膨胀: 在多数设备都存在性能问题
* 频繁垃圾回收: 内存变化图分析

#### 监控内存的几种方式

* 浏览器的任务管理器
* Timeline时序图记录
* 堆快照查找分离DOM
* 判断是否存在频繁的垃圾回收

#### 任务管理器监控内存

chrome-右上角-更多工具-任务管理器

![image-20211108180456212](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211108180456.png)

1: 内存: dom节点的内存

3: 界面中JavaScript所有可达对象正在使用的内存大小 (一直增大, 就有问题)

#### Timeline记录内存

Chrome-控制台-performance
![image-20211108181620278](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211108181620.png)

#### 堆快照查找分离DOM

##### 什么是分离DOM

从DOM树脱离了,但是js代码里还引用着的DOM

堆快照工具位置: chrome-控制台-memory

找分离dom,然后清除

![image-20211109105038344](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211109105038.png)

#### 判读是否存在频繁的GC

* Timeline中频繁的上升下降
* 任务管理器中数据频繁的增加减小