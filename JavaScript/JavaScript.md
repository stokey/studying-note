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