##Objective-C语言基础
+ 语言基础
+ Foundation框架
+ Cocoa/CocoaTouch／IOS SDK

+ 语言基础
	+ 基础类型
	+ 运算符
	+ 选择结构／循环结构
	+ 类／函数／数组／结构
	+ 独特性
	
+ .m(`Objective-C源文件`)/.mm(`Objective-C++源文件`)
+ Terminal执行.m文件命令
	+ clang -fobjc-arc main.m -o progl
	+ ./progl
+ Objective-C区分大小写
+ @autoreleasepool{`//自动释放池：应用在创建新对象时，系统能够有效地管理应用所用的内存`}
+ `	@`符号＋字符串＝`NSString`对象
+ `@interface`/`@implementation`/`program`
	+ `@interface`：用于描述类和类的方法（`.h文件中`）
	+ `@implementation`：用于描述数据，并实现在接口中声明的方法的实际代码
	+ `program`：具体调用
+ `-`负号开头表示是一个实例方法，`+`正号表示一个类方法
+ `*`表明一个对象的引用（或指针）
+ `new`方法可以将`alloc`和`init`操作结合起来

	```
Fraction *fraction = [[Fraction alloc] init];
//等价于
Fraction *fraction = [Fraction new];
	```
+ 数据类型：
	+ 基本数据类型
		+ float (`显示浮点数需要用NSLog转换字符%f或者%g`)
		+ int (`NSLog  %i`)
		+ char (`NSLog  %c`)
		+ double
	+ id数据类型：`可以存储任何类型的对象，一般对象类型`
	+ Boolean类型：`Bool`
+ 数据类型转换：支持隐性类型转换＋强制类型转换
+ condition ? : expression == condition ? condition : expression
+ OC类的声明一般放在名为class.h的文件中(`@interface`)，而类的定义通常放在相同名称的.m文件中（`@implementation`）{`oc 类 @interface`}