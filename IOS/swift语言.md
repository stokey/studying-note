##Swift语言
---
+ 类型标注：`var x:String`
+ 整数字面量
	+ 二进制数，前缀0b
	+ 八进制数，前缀0o
	+ 十六进制数，前缀0x 
+ swift不存在隐式转换，都必须使用显示转换

	```
let three = 3 //猜测类型
let point = 0.123
let new = three + point //Error，报错
let new2 = Double(three) + point //Success
	```
+ typealias:给现有类定义一个别名
+ 可选类型：没值时返回nil（nil != null,nil是一种特殊的数据类型）`var x:String?`
+ !:可选类型的强制解析操作符，使用!强制解析值之前，一定要确定可选对象为一个非nil对象否则会报错
+ 可选绑定：通过可选绑定来判断可选类型是否包含值

	```
var someOptional:String?
if let constantName = someOptional{ 
	statements
}
//如果someOptional有值则执行代码块的statements，否则不执行
	```
+ 隐式解析可选类型：把想要用作可选类型的后面问号（String?）改成感叹号（String!）来声明一个隐式解析可选类型`（一个总有值的可选类型。一个隐式解析可选类型其实就是一个普通的可选类型。但是可以被当作非可选类型来使用，并不需要每次都通过解析来获取可选值）`
+ Swift通过恒等`===`和不恒等`!==`比较符来判断两个对象是否引用同一个对象实例
+ 空合运算符
	
	```
a??b
//对可选类型a进行空判断，如果a包含一个值就进行解封，否则就返回一个默认值b
//a 必须是可选类型，默认值b类型必须要和a存储值的类型保持一致
	```
+ 字符串／结构体／枚举都是值类型
+ Set: 

	```
isSubsetOf() //是否为子类 
isSupersetOf()//是否为父类 
isStrictXXX //真子集 
isDisjointWith() //无交集
	```
+ 修改字典：

	```
airports["LHR"] = "London" //下标语法进行更新 
airports.updateValue("LHR",forKey:"London")
airports["LHR"] = nil //移除键值对LHR
airports = [:]//置空字典
	```
+ switch case 语句中case分支的模式可以使用where语句来判断额外的条件
+ fallthrough(贯穿)，case语句匹配到之后再次往下执行匹配
+ guard：使用guard语句来要求条件必须为真时以执行guard语句之后的代码，否则执行else之后的代码
	
	```
guard let name = person["name"] else{
	return
}
	```
+ 可变参数放在参数表的最后
+ 闭包表达式语法：（闭包函数参数不能设置默认值）

	```
{(parameters)-> returnType in
	statements
}
	```
+ inout修饰函数参数时表示函数参数通过引用传递的，函数内部可以直接修改外部引入的参数值。该类函数调用时需要在参数前加`&`符号作为前缀
+ Swift自动为内联闭包提供了参数名称缩写功能，可以直接通过`$0`,`$1`,`$2`来顺序调用闭包的参数
+ Swift语言中运算符就是一类函数
+ 闭包是引用类型

	```
fun makeIncrement(forIncrement amount:Int)->()->Int {
	var runningTotal = 0
	fun increment()->Int{
		runningTotal += amount
		return runningTotal
	}
	return increment
}
let incrementByTen = makeIncrement(forIncrement:10)
incrementByTen() //返回的值为10
incrementByTen() //返回的值为20
	```
+ 枚举原始值：rawValue
+ 递归枚举是一种枚举类型，它有一个或多个枚举成员使用该枚举类型的实例作为关联值。可以通过在枚举成员或者枚举类型开头加上`indirect`关键字来表明它的成员是可递归的
+ 类与结构体相比
	+ 类是可继承的
	+ 类型转换允许在运行时检查和解释一个类实例的类型
	+ 解构器允许一个类实例释放任何其所被分配的资源
	+ 引用计数允许对一个类的多次引用（结构体总是通过被复制的方式在代码中传递，不使用引用计数）
	+ 结构体为值类型，类为引用类型
+ mutating：标记一个会修改结构体/枚举属性的方法（修改值类型方法）
+ 结构体对于内部的属性和方法是传值的，类是传引用的，如果结构体中的方法需要修改结构体中的某些属性则必须通过mutating关键字修饰该方法
+ 类和结构体的选择：

  考虑构建结构体准则：
	+ 该数据结构的主要目的是用来封装少量相关简单数据值 
	+ 有理由预计该数据结构的实例在赋值或传递时，封装的数据将会被拷贝而不是被引用
	+ 该数据结构中存储的值类型属性，也应该被拷贝，而不是被引用
	+ 该数据结构不需要去继承另一个既有类型的属性或者行为］
	
+ 延迟存储属性：当第一次被调用的时候才会计算器初始值的属性。在属性声明前使用`lazy`来标示一个延迟存储属性（`必须将延迟存储属性声明为变量--var`）

+ 计算属性：计算属性不直接存储值，而是提供一个getter和一个可选的setter，来间接获取和设置其他属性或变量的值
	
	```
struct Point{
	var x=0.0,y=0.0
}
struct Size{
	var width=0.0,height=0.0
}
struct Rect {
	var origin = Point()
	var size = Size()
	var center:Point {
		get {
			let centerX = origin.x + size.width/2
			let centerY = origin.y + size.height/2
			return Point(x:centerX,y:centerY)
		}
		set {
			origin.x = newValue.x - size.width/2
			origin.y = newValue.y - size.height/2
		}
	}
}
	```
	
+ 属性观察器：属性观察器监控和响应属性值的变化，每次属性被设置值的时候都会被调用属性观察器。甚至新的值和现在的值相同的时候也不例外
	
	```
//willSet 在新的值被设置之前调用
//didSet 在新值被设置之后立即调用
class StepCounter {
	var totalSteps: Int = 0 {
		willSet(newTotalSteps) {
			print("About to set totalSteps to \(newTotalSteps)")
		}
		didSet {
			if totalSteps > oldValue {
				print("Added \(totalSteps - oldValue) steps")
			}
		}
	}
}
	```
+ 下标脚本语法：定义下标脚本使用`subscript`关键字

	```
struct TimesTable {
	let multiplier: Int
	subscript(index:Int)->Int {
		return multiplier * index
	}
}
let threeTimesTable = TimesTable(multiplier:3)
print("3的6倍是\(threeTimesTable[6])")//输出‘3的6倍是18’
```
+ `final`关键字可以防止属性或者方法被重写
+ 便利构造器：在init关键字前放置convenience关键字（`convenience init(parameters){ statements }`）。构造器之间的代理调用规则：
	+ 指定构造器必须总是向上代理
	+ 便利构造器必须总是横向代理
+ Swift两段式构造过程（类的构造过程包含两个阶段）
	+ 第一阶段，`每个存储型属性通过引入它们的类的构造器来设置初始值`
	+ 第二阶段，`每个存储型属性值被确认后，它给每个类一次机会在新实例准备使用之前进一步定制它们的存储型属性`

	Swift编译器为了确保两段式构造过程能顺利完成，将执行四种有效的安全检查：
	1. `指定构造器必须保证它所在类引入的所有属性都必须先初始化完成，之后才能将其它构造任务向上级代理给父类中的构造器（一个对象的内存只有在其所有存储型属性确定之后才能完全初始化，之后才能将其它构造任务向上代理给父类中的构造器）`
	2. `指定构造器必须先向上代理调用父类构造器，然后再为继承的属性设置新值。如果没有这么做，指定构造器赋予的新值将被父类中的构造器所覆盖`
	3. `便利构造器必须先代理调用同一类中的其它构造器，然后再为任意属性赋新值。如果没有这么做，便利构造器赋予的新值将被同一类中其它指定构造器所覆盖`
	4. `构造器在第一阶段构造完成之前，不能调用任何实例方法、不能读取任何实例属性的值，self的值不能被引用`
+ 可失败构造器：如果一个类、结构体或枚举类型的对象，在构造自身的过程中可能失败，则应该定义其为可失败构造器（`init关键字后面添加问号--init?。通过return nil 语句表示失败情况`）
+ 必要构造器：在类的构造器前添加required修饰符表明所有该类的子类必须实现该构造器（重写父类的必要构造器时，不需要添加override修饰符）
+ 解决实例之间的循环强引用：
	+ 弱引用：对于生命周期`会`变成`nil`的实例使用弱引用（`weak`关键字，必须为可选类型）
	+ 无主引用：对于初始化赋值后再也`不会`被赋值为`nil`的实例使用无主引用（非可选类型，用关键字`unowned`修饰）

+ do-catch：代码块错误处理

	```
do {
	//可将错误转换成可选值 let x = try? expression
	try expression 
	statements
} catch pattern1 {
	statements
} catch pattern2 {
	statements
}
	```
+ defer：指定清理关键字，在代码执行到要离开当前的代码段之前去执行一套语句

	```
fun processFile(fileName:String) throws {
	if exists(fileName) {
		let file = open(fileName)
		defer {
			close(file)
		}
		while let line = try file.readLine() {
			//处理文件
		}
		//在这里，作用域的最后调用close(file)
	}
}
	```
+ Any和AnyObject的类型转换
	+ AnyObject可以代表任何class类型的实例
	+ Any可以表示任何类型，包括方法类型
+ 扩展（extension）：向一个已有的类、结构体、枚举类型或者协议类型添加新功能（包括没有权限获取原始代码的情况下扩展类型的能力）。适用范围：
	1. 添加计算型属性和计算型静态属性
	2. 定义实例方法和类型方法
	3. 提供新的构造器
	4. 定义下标
	5. 定义和使用新的嵌套类型
	6. 使一个已有类型符合某个协议