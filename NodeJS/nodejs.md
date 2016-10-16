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
	+ 文本编码
		+ BOM字符：用于标记文本文件使用的Unicode编码，其本身是一个Unicode字符（`\uFEFF`）。BOM字符本身不属于文本内容的一部分。
		
		```
		//在不同的Unicode编码下，BOM字符对应的二进制字节
		Bytes         Encoding
		----------------------
		FE EF         UTF16BE
		FF FE         UTF16LE
		EF BB BF       UTF8
		```
		
		识别和去除UTF8 BOM的功能
		
		```
		function readText(pathName){
			var bin = fs.readFileSync(pathName);
			if(bin[0]===0xEF && bin[1]===0xBB && bin[2]===0xBF){
				bin = bin.slice(3);
			}
			return bin.toString('utf-8');
		}
		```
		
		+ 单字节编码（binary）：都以单字节编码保存数据，能够避免大于0xEF的单个字节编码应编码格式不同造成的乱码。
		
		```
		function replace(pathname){
			var str = fs.readFileSync(pathname,'binary');
			str = str.replace('foo','bar');
			fs.writeFileSync(pathname,str,'binary');
		}
		```
+ #### 网络操作（`Linux系统监听1024以下端口需要root权限`）
	+ `http`模块即可以当作HTTP服务器监听HTTP客户端的请求返回响应，也可以当作客户端使用，发起一个HTTP客户端请求，获取服务端响应
	+ `https`模块基本与`http`模块相同，`https`模块需要额外处理`SSL证书`
	+ 如果目标服务器使用的SSL证书是自制的，默认情况下`https`模块会拒绝链接，提示证书安全问题。此时需要在`options`里加入`rejectUnauthorized:false`字段可以禁止对证书有效性的检查
	+ `URL`模块：用于解析URL、生成URL以及拼接URL
	+ `QueryString`模块：用于实现URL参数字符串与参数对象的相互转换
	+ `Zlib`模块：用于数据压缩和解压的功能
	+ NodeJS在处理从别的客户端或者服务器端收到的头字段时，都统一地转换成小写字母格式
	+ `socket hang up`错误
		+ 出现原因：`http`模块提供了一个全局客户端http.globalAgent，使得我们在使用.request或.get方法时不用手动创建客户端，但是全局客户端默认只允许5个并发Socket连接
		+ 解决方法：通过将`http.gloableAgent.maxSockets`属性数值改大即可。（`https`模块通过修改`https.gloableAgent.maxSockets`属性数值）
		
	+ `Net`模块：用于创建Socket服务器或者Socket客户端
	
+ #### 进程管理
	+ Process:process不是内置模块，而是一个全局对象，可以在任何地方都直接使用
	+ Child Process:使用`child_process`模块可以创建和控制子进程。最核心的是`.spawn`
	+ Cluster:是对`child_process`模块的进一步封装，专用于解决单进程NodeJS Web服务器无法充分利用多核CPU的问题
	+ 获取命令行参数：`process.argv`命令获取命令行参数（node执行程序路径和主模块文件路径固定占据了`argv[0]`和`arvg[1]`两个位置）
	+ 退出程序：`process.exit(code)`(正常退出程序的退出状态码为0)
	+ 控制输入输出
		+ 标准输入流：`process.stdin`[只读数据流]
		+ 标准输出流：`process.stdout`[只写数据流]
		+ 标准错误流：`process.stderr`[只写数据流]
	+ 如何降权：`Linux系统监听1024以下端口需要root权限，但是一旦完成端口监听后，继续让程序运行在root权限下存在安全隐患`
		+ 如果是通过`sudo`获取`root`权限的，运行程序的用户的`UID`和`GID`保存在环境变量`SUDO_UID`和`SUDO_GID`里边。如果是通过`chmod＋s`方式获取root权限的可直接通过`process.getuid`和`process.getgid`方法获取
		+ `process.setgid`和`process.setuid`方法只接受number类型的参数
		+ 降权时必须`先降GID再降UID`，否则顺序反过来的话就没权限更改程序的`GID`了
		+ ?????如何查看权限
	```
	http.createServer(callback).listen(80,function(){
		var env = process.env,
			uid = parseInt(env['SUDO_UID'] || process.getuid(),10),
			did = parseInt(env['SUDO_GID'] || process.getgid(),10);
		process.setgid(did);
		process.setuid(uid);
	});
	```
	
	+ 如何创建子进程:`child_process.spawn(exec,args,options)`
		+ 第一个参数是执行文件路径
		+ 第二个参数中数组中的每个成员都按顺序对应一个命令参数
		+ 第三个参数用于配置子进程的执行环境与行为[`可以将子进程的输入输出重定向到任何数据流上，或者让子进程共享父进程的标准输入输出流`]
	
	```
	var child = child_process.spawn('node',['xxx.js']);
	child.stdout.on('data',function(chunk){
		console.log('stdout:'+chunk);
	});
	child.stderr.on('data',function(){
		console.log('stderr:'+data);
	});
	child.on('close',function(code){
		console.log('childe process exited with code:'+code);
	});
	```
	
	+ 进程间如何通信
		+ Linux系统下进程通过信号相互通信
	```
	//父进程通过.kill方法向子进程发送SIGTERM信号，子进程监听process对象的SIGTERM事件响应信号
	/* parent.js*/
	var child = child_process.spawn('node',['child.js']);
	child.kill('SIGTERM');
	/*child.js*/
	process.on('SIGTERM',function(){
		cleanUp();
		process.exit(0);
	});
	```
		+ NodeJS父子进程之间可以通过IPC双向传递数据
			+ 父进程在创建子进程时，在`options.stdio`字段中通过`ipc`开启了一条IPC通道
			+ 开启`IPC`通道之后父进程就可以监听子进程对象的`message`事件接受来自子进程的消息［子进程相同］
		```
		//parent.js
		var child = child_process.spawn('node',['child.js'],{
			stdio:[0,1,2,'ipc']
		});
		child.on('message',function(msg){
44		child.send({hello:'hello'});
		//child.js
		process.on('message',function(msg){
			msg.hello = msg.hello.toUpperCase();
			console.log('receive message event data:'+msg.hello);
			process.send(msg);
		});
		```
		
		+ 守护子进程
		
		```
		//daemon.js
		function spawn(mainMoudle){
			var worker = child_process.spwan('node',[mainMoudle]);
			work.on('exit',function(code){
				if(code!==0){
					spawn(mainMoudle);
				}			
			});
		}
		spwan('work.js');
		```
		
+ #### 异步编程：`依托回调来实现`
	+ 在NodeJS中几乎所有异步API都按照回调函数中第一个参数是`err`来设计（`异步函数执行中以及执行之后产生的异常冒泡到执行路径被打断的位置时，如果一直没有遇到try语句，就会作为一个全局的异常抛出`）
	+ `domain`模块：用于简化异步代码异常处理（多重嵌套）
		+ NodeJS通过`process`对象提供了捕获全局异常的方法（`process.on('uncatchException',function(err){ console.log(err);})`）
		+ 通过创建一个子域的方式和全局异常捕获方法从而达到简化异步操作异常处理
	+ NodeJS官方文档中强烈建议处理完异常后立即重启程序，如果不立即退出程序可能会发生严重的内存泄漏或者表现的很怪异。［JS本身的throw..try..catch异常处理机制并不会导致内存泄漏。而是NodeJS中的内部用C／C++实现的API可能会引发内存泄漏等问题］
+ #### Chrome调制NodeJS
	+ 安装nightly build:`npm install -g node-nightly ====> node-nightly`
	+ 使用`--inspect`标志来运行NodeJS代码：`node-nightly --inspect index.js //或者使用--debug-brk 标记让代码在运行到第一行时就停住`

+ #### WebSocket/Socket/HttpClient
	+ WebSocket: 为了满足基于Web的日益增长的实时通信需求而产生的（解决HTTP协议消耗带宽（HTTP HEAD较大）过大和CPU浪费（没有信息也要接受请求））