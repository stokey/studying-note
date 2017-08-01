# Kotlin

## 与Java相比的优势

+ 大幅度精简代码 
+ 100%兼容Java代码
+ 函数式编程
+ 各种语法糖

## 适用范围

+ 服务器端
+ Android
+ JavaScript

## 基本语法

+ 基础类型：`kotlin中所有的东西都是对象`
	+ 数字：`kotlin中字符不是数字`

	| Type | Bit width |
	| :--: | :--: |
	|Double|64|
	|Float|32| 
	|Long|64|
	|Int|32|
	|Short|16|
	|Byte|8|
	
	+ 字符：`Char`
	+ 布尔：`Boolean`
	+ 数组：`Array ---> 不型变，不能把Array<String>赋值给Array<Any>`
		+ 创建
			+ `arrayOf(1,2,3) == [1,2,3]`
			+ `arrayOfNulls()：创建一个指定大小、元素都为空的数组`
			+ 工厂函数：`val asc = Array(5,{ i -> (i*i).toString()})`
		+ 常用属性
			+ `get`
			+ `set`
			+ `size`
		+ 原生类型数组
			+ `ByteArray`
			+ `ShortArray`
			+ `IntArray`
			+ 等等   
	+ 字符串：`String——不可变`
		+ 原生字符串：`内部没有转义并且可以包含换行和任何其他字符，使用——"""表示` 
		+ 字符串模板：`$`
	
+ 字面常量：`不支持八进制`
	+ 十进制
	+ 十六进制
	+ 二进制 
+ `==`与`===`区别
	+ `===`：内存地址是否相等
	+ `==`：数值是否相等
+ 数值转换：`kotlin不支持隐式转换，只能显式转换`
	+ `toByte():Byte`
	+ `toShort():Short`
	+ `toInt():Int`
	+ `toLong():Long`
	+ `toFloat():Float`
	+ `toDouble():Double`
	+ `toChar():Char`
+ 位运算列表
	+ `shl(bits) --> 有符号左移<<` 
	+ `shr(bits) --> 有符号右移>>` 
	+ `ushr(bits) --> 无符号右移>>>`
	+ `and(bits) --> 位与&`     
	+ `or(bits) --> 位或|` 
	+ `xor(bits) --> 位异或^`
	+ `inv() ---> 位非`
+ 控制流
	+ if：`表达式，会返回一个值`

	```kotlin
	// 传统用法
	var max = a
	if (a < b) {
	  max = b
	} else {
	  max = a;
	}
	// 作为表达式
	val max = if (a > b) a else b
	
	// if 分支也可以是代码块
	val max = if (a > b){
		print("Choose a")
		a
	} else {
		print("Choose b")
		b
	}
	```
	
	+ When表达式：`代替switch，如果when作为一个表达式使用，则必须有else分支`
	
	```kotlin
	when(x){
		1 -> print("x==1")
		2 -> print("x==2")
		else -> {
			// like default
			print("x is neither 1 nor 2")
		}
	}
	fun hasPrefix(x: Any) = when(x) {
    	is String -> x.startsWith("prefix")
    	else -> false
	}
	```
	
	+ For循环：`可以循环变量任何提供了迭代器的对象`

	```kotlin
	for (item in collection){
		// ...
	}
	```

	+ While循环：`while/do...while`

	
+ 类和对象
	+ 类：`class关键字声明类，类声明由类名、类头（指定其类型参数、主构造函数等）和由大括号包围的类体构成。类头和类体可选`
	
	```kotlin
	class Invoice{
	}
	// 只有类名
	class Empty
	``` 

	+ 构造函数
		+ 主构造函数：`类头的一部分，跟在类名（和可选的类型参数）后`
		
		```kotlin
		class Person constructor(firstName:String){
		}
		// 如果主构造函数没有任何注解或者可见性修饰符，则可省略constructor关键字
		class Person (firstName:String){
		}
		// 主构造函数不能包含任何代码，初始化的代码可以放在以init关键字作为前缀的初始化块
		class Customer(name:String){
			init{
				print("Customer initialized with value ${name}")
			}
		}
		// 声明属性以及从主构造函数初始化属性的简洁语法
		class Person(val firstName:String,val lastName:String, var age:Int){
			// ...
		}
		```
		
		+ 次构造函数：`不在类头，用contructor关键字修饰的方法`
		
		```kotlin
		class Person {
			constructor(parent:Person){
				parent.children.add(this)
			}
		}
		```
	