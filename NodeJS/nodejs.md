##NodeJS
---
+ #### 模块：每个文件可以称为一个模块，文件路径则为模块名
	+ 模块的引入与导出
		+ require:`用于在当前模块中加载和使用别的模块`
		
		```
		//传入一个模块名，返回一个模块导出对象
		//模块名可疑使用相对路径(./)，或者绝对路径(/home/user)，同时文件扩展名可以被省略
		var foo = require('./foo');
		//加载JSON文件
		var data = require('./data.json');
		//内置模块直接传递模块名称，无需使用相对位置或者绝对位置
		var fs = require('fs');
		```
		+ exports:`当前模块的导出对象，用于导出模块公有方法和属性`
		
		```
		exports.hello = function(){
			console.log('hello world');
		}
		```
		+ module:`通过该对象可以访问到当前模块的一些相关信息，但最多的用途是替换当前模块的导出对象`
		
		```
		//模块默认导出对象被替换成一个函数
		module.export = function(){
			console.log('hello world');
		}
		```
	+ 模块初始化：`发生在第一次被使用的时候，之后被缓存起来重复利用`
	+ 主模块：`通过命令参数传递给NodeJS以启动程序的模块被称为主模块`
+ #### 代码的组织和部署
	+ 包：`把由多个子模块组成的大模块称为包，并把所有子模块放在同一目录里`
		+ index.js:`加载包的入口模块(index.js作为包的入口模块可以使用模块所在目录的路径代替模块文件路径)`
		
		```
		//等价
		var cat = require('/home/user/lib/cat');
		var cat = require('/home/user/lib/cat/index');
		```
		+ package.json:`用于自定义入口模块的文件名和存放位置`
		
		```
		//package.json
		{
			"name":"cat",
			"main":"./lib/main.js"
		}
		//此时也可以通过使用require('/home/user/lib/cat')的方式加在模块
		```
	+ 工程目录结构：
	
	```
	- /home/user/workspace/node-echo/   # 工程目录
    - bin/                          # 存放命令行相关代码
        node-echo
    + doc/                          # 存放文档
    - lib/                          # 存放API相关代码
        echo.js
    - node_modules/                 # 存放三方包
        + argv/
    + tests/                        # 存放测试用例
    package.json                    # 元数据文件
    README.md                       # 说明文件
	```
	+ NPM: `包管理工具`
+ #### 文件操作：（调用内置fs模块进行相关操作）
	+ 小文件拷贝
	
	```
	var fs = require('fs');
	function copy(src,dat){
		fs.writeFileSync(dat,fs.readFileSync(src));
	}
	function main(argv){
		copy(argv[0],argv[1]);
	}
	main(process.argv.slice(2));
	//process是一个全局变量，可通过process.argv获得命令行参数
	//argv[0]固定等于NodeJS之行程序的绝对路径，argv[1]固定等于主模块的绝对路径。因此第一个命令行参数argv[2]位置开始
	```
	+ 大文件拷贝
	
	```
	var fs = require('fs');
	function copy(src,dst){
		fs.createReadStream(src).pipe(fs.createWriteStream(dst));
	}
	function main(argv){
		copy(argv[0],argv[1]);
	}
	main(process.argv.slice(2));
	```
	+ Buffer: `［出现的原因：JS没有二进制数据类型］`
		+ 初始化：`var bin = new Buffer([0x68,0x65]);//可以通过.length获取长度，通过下标获取对应位置二进制数值`
		+ 与字符串的相互转换：`var str = bin.toString('utf-8); var bin = new Buffer('hello','utf-8');`
		+ Buffer与字符串的区别：字符串是只读的，Buffer更像是可以做指针操作的C语言数组`［Buffer.slice方法返回的不是一个新的Buffer，而更像是返回了指向原Buffer中间的某个位置指针，因此对.slice方法返回的Buffer的修改会作用于原Buffer。因此要想拷贝一份Buffer，得首先创建一个新的Buffer，然后通过.copy方法把原Buffer中的数据复制过去］`
	+ Stream: `Stream基于事件机制工作，所有Stream的实例都继承于NodeJS提供的EventEmitter` 