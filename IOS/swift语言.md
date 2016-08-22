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
var someOptinal:String?
if let constantName = someOptinal{ 
	statements
}
//如果someOptinal有值则执行代码块的statements，否则不执行
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
ariports["LHR"] = "London" //下标语法进行更新 
ariports.updateValue("LHR",forKey:"London")
ariports["LHR"] = nil //移除键值对LHR
airports = [:]//置空字典
	```
+ 枚举默认值：rawValue
+ mutating：标记一个会修改结构体的方法
+ 结构体对于内部的属性和方法是传值的，类是传应用的，如果结构体中的方法需要修改结构体中的某些属性则必须通过mutating关键字修饰该方法
+ extension 用来为现有的类型添加新的方法和属性