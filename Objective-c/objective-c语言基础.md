##Objective-C语言基础
+ 语言基础
+ Foundation框架
+ Cocoa/CocoaTouch／IOS SDK

+ 语言基础
	+ 基础类型
	+ 运算符
	+ 选择结构／循环结构
	+ 类／函数／数组／结构
	+ 作用域
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
+ 自动生成设值方法和取值方法
	+ 接口部分使用`@property`指令标识属性
	+ 类文件中使用`@synthesize`指令声明属性
+ 使用`@property`指令之后对于是否使用`@synthesize`指令则不是必须的，此时编译器会自动生成setter和getter。但是如果没有使用`@sythesize`指令则此时编译器生成的实例变量会以下划线（`_`）字符作为其名称的第一个字符
+ `.`运算通常用在属性上，用于设置或取得变量的值
+ 方法参数
	+ 多参数方法：`-(void) setTo:(int) n over:(int) d;` 
	+ 不带参数名的方法：`－(int) set : (int) n: (int) d;//这个方法为set::，即方法名为set，有两个参数，参数之间用分号分隔`
+ static 关键字：只在程序开始执行的时初始化一次
	
	```
@implementation
-(int) addStatic
{
    static int pageCount = 0;
    //++pageCount return value 1;pageCount++ return value 0
    return ++pageCount;
}
@end
@atuoreleasepool{
	for(int i=0;i<10;i++){
		NSLog(@"Static number is:%i",[fraction addStatic]);
		//print 1,2...10
	}
}
	```
	
+ 在实现部分声明和`synthesize`的实例变量是私有的，子类不能直接访问
+ `@class`指令引入类名，但是无法引用类相关方法
+ float 类型不是一个精确的值：`float x =201.10 //可能为201.100002342`。如何比较值？？？？

+ `@try{} @catch(e){}`捕获异常
+ `@instancetype`特殊类型：`-(instancetype) init(){...}`
+ 访问权限：（`方法没有访问修饰符`）
	+ `@public`：类外可以使用，通过`->`指向命令调用
	+ `@private`
	+ `@protect`
	+ `@package`：在包内相当于受保护，在包外相当于私有无法被访问