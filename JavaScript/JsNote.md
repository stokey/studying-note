###JavaScript知识点
####创建对象
+ 工厂模式

```
// 优点：完成了返回一个对象的要求
// 缺点：1. 无法通过Constructor识别对象 2. 每次创建Person对象时，所有say方法都是一样的，却存储多次。造成资源浪费
function Person(){
	var o = new Object();
	o.name = 'hello';
	o.say = function(){
		console.log(this.name);
	}
	return o;
}
var person1 = Person();
```
+ 构造函数模式

```
// 优点： 1. 通过Constructor或者instanceof可以识别对象实例的类别 2. 可以通过new关键字创建对象实例
// 缺点：相同方法存储多次，造成资源浪费
function Person(){
	this.name = 'hello';
	this.say = function(){
		console.log(this.name);
	}
	return o;
}
var person1 = new Person();
```
+ 原型模式

```
// 优点：1. say方法是共享的，所有的实例say方法都指向同一个 2. 可以动态的添加原型对象的方法和属性，并直接反映在对象实例上
// 缺点：1. 创建的对象共用相同存储区域 2. 第一次调用say方法或者name属性的时候会搜索两次，第一次是在实例上寻找say方法，没找到就去原型对象(Person.prototype)上找say方法，找到后就会在实例上添加这些方法或者属性 3. 所有的方法都是共享的，没法办创建实例自己的属性和方法。也没办法像构造函数那样传递参数 
function Person() {
	Person.prototype.name = 'stokey',
	Person.prototype.say = function() {
		alert(this.name);
	}
}
Person.prototype.friends = ['lli'];
var persona1 = new Persona();
```
+ 构造函数和原型组合模式

```
// 优点：1. 解决了原型模式对于引用对象的缺点 2. 解决了原型模式没有办法传递参数的缺点 3. 解决了构造函数模式不能共享方法的缺点
function Person(name){
	this.name = name;
	this.friends = ['lli'];
}

Person.prototype.say = function(){
	console.log(this.name);
}
var per1 = new Person('hello');
per1.say();
```
+ 动态原型模式

```
// 优点：1. 可以在初次调用构造函数的时候就完成原型对象的修改 2. 修改能体现在所有的实例中
funtion Person(name){
	this.name = name;
	if(typeof this.say != 'function'){
	 Person.prototype.say = function(){
	 	alert(this.name);
	  }
	}
}
```
+ 稳妥构造模式

```
// 优点：安全，name好像变成了私有变量。只能通过say方法去访问
// 缺点：不能区分实例的类别
function Person(name){
	var o = new Object();
	o.say = function(){
		alert(name);
	}
}
var person1 = new Person('hello');
person1.name;//undefined
person1.say();//hello
```
