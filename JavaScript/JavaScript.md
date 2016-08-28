## JavaScript
---
+ JavaScript是一门弱类型语言
+ `NaN`是一个数值，表示一个不能产生正常结果的运算结果。NaN不等于任何值，包括它自己。可以用`isNaN（*number*）`检测NaN
+ `Infinity`表示所有大于`1.79769313486231579e+308`的值
+ `Math.floor(*number*)`方法将一个数字转换成一个整数（`parseInt()方法也可以实现整数转换`）~~javascript只有number类型，那为什么还要parseInt，Math.floor方法~~
+ 条件语句中`假条件`：
  	+ false
	+ null
	+ 空字符串' '
	+ 数字`0`
	+ 数字`NaN`
+ `for in`/`for of`：
	+ `for in`：用于遍对象，会枚举一个对象的所有属性名称`[不应该用于遍历数组，遍历数组时取到下标值]`
	+ `for of`：用于遍历可迭代对象（*`Array,Map,Set,String,TypedArray,arguments`*）。遍历的事不同属性的属性值，遍历数组时取到的值而非下标。
+ Object.hasOwnProperty(*variable*)来确定这个属性名是否是该对象的成员
+ JavaScript数据类型：
	+ 数字（*number*）
	+ 字符串（*string*）
	+ 布尔值（*Bool*）
	+ null值
	+ undefined值
	+ 对象（*数组，函数，正则表达式等*）
+ JavaScript里的对象对新属性的名字和属性的值没有限制，属性值可以是除`undefined`之外的任何值（可以通过给对象属性设值为`undefined`达到删除对象属性的效果）
+ 尝试从`undefined`的成员属性中取值将会导致TypeError异常，此时可以通过`&&`运算符来避免错误:` flight && flight.equipment`
+ 原型连接在更新时是不起作用的。当我们对某个对象作出改变时，不会触及该对象的原型。原型连接只有在检索值的时候才能用到。`如果我们尝试去获取对象的某个属性值，但该对象没有此属性名，那么JavaScript会试着从原型对象中获取属性值，如果那个原型对象也没有该属性，则继续向上寻找，直到终点Object.prototype。如果想要的属性完全不存在于原型链中，那么结果就是undefined值。`
+ `typeof`用于确定属性的类型，`hasOwnProperty`方法用于检查对象独有的属性，不会检查原型链
+ `delete`运算符可以用来删除对象的属性。不会触及原型链上的对象，只删除该对象本身属性。

	```
another.nickname //'Moe'
//删除another的nickname属性，从而暴露出原型的nickname属性
delete another.nickname;
another.nickname //'Curly'
	```
+ 减少全局变量污染。最小化全局变量的方法之一就是创建一个唯一的全局变量，其它全局性资源作为该全局变量的属性存储到该全局变量中
+ **`所谓编程，就是将一组需求分解成一组函数与数据结构的技`**
+ JavaScript中的函数就是对象。对象字面量产生的对象连接到Object.prototype，函数对象连接到Function.prototype(该原型对象连接到Object.prototype)。每个函数在创建时会附加两个隐藏属性：函数的上下文和实现函数行为的代码
+ 每个函数对象在创建时会拥有一个prototype属性，它的值时一个拥有constructor属性且值即为该函数的对象
+ 函数在传递参数过程中除了接受声明时定义的形式参数，还需要接收两个附加的参数：`this`和`arguments（实际参数）`。参数`this`在面向对象编程中非常重要，它的取值决定于调用的模式
+ JavaScript中的调用模式:
	+ 方法调用模式：`如果调用表达式包含一个提取属性的动作时则被视为一个方法来调用，即通过点表达式或者下标表达式调用（obj.increment()）。this绑定到调用对象上`
	+ 函数调用模式：`当一个函数并非一个对象的属性时，则被视为当做一个函数来调用（ var sum = add(3,4);）。this被绑定到全局对象`
	+ 构造器调用模式：`一个函数如果创建的目的就是希望结合new前缀来调用，那么它就被称构造器函数。`
	
		```
//大写约定，保存在大写格式命名的变量里
var Quo = function(string){
	this.status = string;
}
Quo.prototype.get_status = function(){
	return this.status;
}
//如果在一个函数前面带上new来调用，那么背地里将会创建一个连接到该函数的prototype成员的新对象，同时this会被绑定到那个新对象上
var myQuo = new Quo("confused");
myQu0.get_status();
		```
	+ apply调用模式：`apply方法接收两个参数，第一个是要绑定的给this的值，第二个就是参数数组。函数通过arguments数组传递参数列表。因为语言设计错误，arguments并不是一个真正的数组，只是一个“类似数组”的对象，拥有一个length属性，但没有任何数组的方法`
	*不同调用模式在初始化关键字this上存在差异*
+ 调用运算符：`()`，函数声明时的参数为形式参数名。实际参数个数和形式参数个数可以不匹配，如果实际参数过多则多出的参数会被忽略，如果实际参数过少则少掉的参数会被设置为`undefined`
+ 当一函数被保存为一个对象的属性时，我们称它为一个方法。［**方法就是一个被类绑定的函数?**］。`this`到对象的绑定发生在函数调用的时候（**ES6可以通过bind()方法手动绑定this**)）
+ 函数调用模式带来的问题及解决：
	+ 问题：`函数调用模式this被绑定到全局对象（严格模式下绑定到undefined）。内部函数取到的this也为全局对象，这导致内部函数无法访问到外部函数私有成员，无法起到相应作用`
	+ 解决：`在外部函数定义个变量并给它赋值为this，则内部函数可以通过这个变量访问到this`
+ JavaScript允许给语言的基本类型扩充功能
	
	```
	//通过给Function.prototype增加一个Methode方法，下次给对象增加方法的时候就不必键入prototype这几个字符
	Function.prototype.method = function (name,func){
		if(!this.prototype[name]){
			this.prototype[name] = func;
		}
		return this;
	}
	Number.method('integer',function(){
		return Math[this < 0 ? 'ceil' : 'floor'](this);
	});
	(-10/3).integer();//-3
	String.method('trim',function(){
		return this.replace(/^\s+|\s+$/g,'');
	});
	```
+ 作用域：控制着变量和参数的可见性及生命周期。从而减少了名称冲突，并且提供了自动内存管理
+ JavaScript的代码块语法貌似支持块级作用域，但是实际上JavaScript并不支持。JavaScript拥有函数作用域。意味着函数中的参数和变量在函数外部是不可见的，而在一个函数内部任何位置定义的变量，在该函数内部任何地方都可见。

	```
function testFunc(){
	var num1 = 10,num2 =11;
	console.log(`num1:${num1},num2:${num2}`);
	{
		var num3 = 12;
	} 
	console.log(`num3:${num3}`);
}
//不支持块级作用域，如果支持块级作用域num3应该输出为undefined
//为了减少这一特性的影响，最好的做法就是在函数体的顶部声明函数中可能用到的所有变量
testFunc();//输出num1:10,num2:11 num3:12
	```
+ 闭包：函数可以访问它被创建时所处的上下文环境
+ 避免在循环中创建函数，它可能只会带来无谓的计算和混淆。

	```
//内部函数能访问外部函数的实际变量而无需复制
var add = function(nodes){
	var i;
	for (i=0;i<nodes.length;i+=1){
		nodes[i].onClick = function(e){
			alert(i);//每次弹出都是节点总长度
		}
	}
}
//修改
var add = function (nodes){
	//	通过辅助函数绑定当前的i值后就不会发生混淆了
	var helper = function(i){
		return function(e){
			alert(i);
		}
	}
	var i;
	for (i=0;i<nodes.length;i+=1){
		nodes[i],onClick = helper(i);
	}
}
	```
+ 模块：一个提供接口却隐藏状态与实现的函数或对象。可以通过使用函数和闭包来构造模块