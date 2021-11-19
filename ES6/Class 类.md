### class 关键字

* 实例方法

  ```
  // 方法1
  class Person {
  	constructor (name) {
  		this.name = 'bruce'
  	}
  }
  // 方法2
  class Person {
  	constructor (name) {
  		
  	}
  	name = 'bruce'
  }
  ```

  

* 静态方法

  ```
  class Person {
  	constructor (name) {
  		
  	}
  	static say(){
  		console.log('hello')
  	}
  }
  Person.say()
  ```

* 继承 extends

  ```
  class Person {
  	constructor (name) {
  		this.name = 'bruce'
  	}
    say(){
  		console.log('hello')
  	}
  }
  class Student extends Person {
  	constructor (name, number) {
  		super(name)
  		this.number = number
  	}
  	hello () {
  		super.say()
  		console.log('balabala')
  	}
  }
  ```

  

