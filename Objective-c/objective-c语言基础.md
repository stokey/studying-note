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
