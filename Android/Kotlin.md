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
		
		+ 次构造函数：`不在类头，用contructor关键字修饰的方法。如果类有一个构造函数，每个次构造函数需要委托给主构造函数`
		
		```kotlin
		class Person {
			constructor(parent:Person){
				parent.children.add(this)
			}
		}
		// 如果一个非抽象类没有声明任何（主或次）构造函数，会有一个生成的不带参数的主构造函数，构造函数可见性是public。
		// 非默认可见性的空主构造函数
		class DontCreateMe private constructor() {
		}
		```
	+ 类成员
		+ 构造函数和初始化块
		+ 函数
		+ 属性
		+ 嵌套类和内部类
		+ 对象声明
	+ 类继承：`kotlin所有类都有一个共同的超类Any --\-> Java Object，Any类除了equals()、hashCode()和toString()外没有任何成员`  

	```kotlin
	// open标注与Java中final相反，表示允许其他类从这个类继承。默认情况下，kotlin中所有的类都是final
	open class Base(p:Int)
	class Derived(p:Int) : Base(p)  
	```
	
	+ 覆盖方法
	
	```kotlin
	open class Base {
		open fun v(){}
		fun nv(){}
	}
	class Derived() : Base() {
		// 标记为override的成员本身是开放的
		override fun v(){}
		// 禁止再次覆盖
		final override fun v(){}
	}
	```
	
	+ 覆盖属性：`必须具有兼容类型，可以用一个var属性覆盖一个val属性，反之不行`
	+ 调用超类实现
		+ 派生类访问超类：`super`
		
		```kotlin
		open class Foo {
			open fun f(){ println("Foo.f()")}
			open val x: Int get() = 1
		}
		class Bar : Foo() {
			override fun f(){
				super.f()
				println("Bar.f()")
			}
			override val x: Int get() = super.x + 1
		}
		```
		
		+ 内部类访问外部类的超类：`super@Outer` 

		```kotlin
		class Bar: Foo(){
			override fun f() { /* ...... */}
			override val x: String get() = "..."
			inner class Baz {
				fun g(){
					super@Bar.f()
					println(super@Bar.x)
				}
			}
		}
		```
		
		+ 覆盖规则：类单继承
		
		```kotlin
		open class A {
    			open fun f(){print("A")}
    			fun a() {print("a")}
		}
		
		interface B {
		    // 接口中成员默认为open
		    fun f(){print("B")}
		    fun b(){print("b")}
		}
		
		class C(): A(),B{
		    // 编译器要求覆盖f()
		    override fun f() {
		        super<B>.f()
		        super<A>.f()
		    }
		    override fun b() {
		        super.b()
		    }
		}
		```
		
		+ 抽象类／属性：`abstract(默认open)——可以用一个抽象成员覆盖一个非抽象开放成员`
		+ Kotlin类中没有静态方法——`包级函数`

+ 属性和字段
	+ 修饰符
		+ `var`：可变
		+ `val` ：只读
	+ Getter/Setter

	```kotlin
	var <propertyName>[: <PropertyType>] [= <property_initializer>]
	[<getter>]
	[<setter>]
	// 只读属性不允许设置setter
	val isEmpty: Boolean
		get() = this.size == 0
	
	var stringReprentation: String 
		get() = this.toString()
		set(value) {
			setDataFormString(value)
		} 
	```
	
	+ 幕后字段
	+ 幕后属性

	+ 编译期常量：`const修饰符标记`
		+ 修饰的属性需要满足的条件
			+ 位于顶层或object的一个成员
			+ 用String或原生类型初始化
			+ 没有自定义getter
	+ 延迟初始化属性：`lateinit修饰符标记，只能用在类体中声明的var属性，并且仅当该属性没有自定义getter或setter，同事改属性必须是非空类型，且不能是原生类型`
	
	```kotlin
	public class MyTest {
		lateinit var subject : TestSubject
		
		@SetUp fun setup(){
			subject = TestSubject()
		}
		@Test fun test(){
			subject.method()
		}
	}
	```   
	
	+ 覆盖属性
	+ 委托属性

+ 接口
	+ 定义/实现

	```kotlin
	interface MyInterface {
		fun bar()
		fun foo(){
			// 可选方法体
		}
	}
	
	// 一个类或者对象可以实现一个或者多个接口
	class Child : MyInterface {
		override fun bar(){
			// 方法体
		}
	}
	``` 
	
+ 可见性修饰符：`默认可见性为public` 
	+ 包名
		+ private：`声明它的文件内可见`
		+ protected：`不适用于顶层声明`
		+ internal：`在相同模块内随处可见`
		+ public：`默认`
	+ 类和接口：`Java相同`
		+ internal：`能见到类声明的本模块内的任何客户端都可见`
	+ `局部变量、函数和类不能有可见性修饰符` 
	+ 模块（`internal——相同模块内可见`）：`编译在一起的一套Kotlin文件`

+  扩展：`扩展一个类的新功能而无需继承该类或使用像装饰者设计模式——支持扩展函数和扩展属性。扩展不能真正修改所扩展的类`
	+ 扩展函数：`需要用一个接收者类型也就是被扩展的类型来作为他的前缀`

	```kotlin
	// 为MutableList<Int>添加一个swap函数
	fun MutableList<Int>.swap(index1:Int, index2:Int){
		val temp = this[index1]
		this[index1] = this[index2]
		this[index2] = temp
	}
	// 如果一个类定义有一个成员函数和一个扩展函数，且两个函数又有相同的接收者类型、相同的名字并且都适用给定的参数，这种情况总是取成员函数
	class C {
		fun foo() {println("member")}
	}
	fun C.foo(){println("extension")} // 输出member
	```
	
	+ 扩展属性：`属性扩展不能有初始化器，只能显式提供getter/setter定义`
	
	```kotlin
	val <T> List<T>.lastIndex: Int
		get() = size -1
	// errror
	val Foo.bar = 1 //错误：扩展属性不能有初始化器
	```

+ 数据类：`只保存数据的类，用data关键字标记`
	+ 声明

	```kotlin
	data class User(val name:String, val age:Int)
	```
	
	+ 要求
		+ 主构造函数需要至少有一个参数
		+ 主构造函数的所有参数需要标记为val或var
		+ 数据类不能是抽象、开放、密封或内部的
		+ JVM中，如果生成的类需要含有一个无参数的构造函数，则所有属性必须制定默认值
		
		```kotlin
		data class User(val name:String = "",val age: Int =0)
		``` 
	+ 复制：`copy()——复制一个对象，然后改变某些属性，其余属性保持不变`

	```kotlin
	fun copy(name:String = this.name,age:Int=this.age) = User(name,age)
	val jack = User(name="Jack",age=21)
	val olderJack = jack.copy(age=22)
	```
	
	+ 数据类和解构声明
	
	```kotlin
	val jane = User("Jane",35)
	val(name,age) = jane
	println("$name,$age years of age")
	```

+ 密封类：`sealed修饰符标记，限制类继承结构——所有子类都必须在与密封类自身相同的文件中声明`
