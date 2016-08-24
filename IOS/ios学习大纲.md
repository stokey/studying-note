##ios 学习大纲：
---
1. ###swift语言基础
2. ###IOS基础
	+ **UIView**
	+ **IOS程序结构：AppDelegate & UIApplication**
	+ **Storyborad & XIB**
	+ **IBOutlet & IBAction**
	+ **UIView Animation & 核心动画**
	+ **Quartz2D && CoreGraphics (了解)**
	+ **UIViewController**
3. ###触摸事件 & 手势处理 & 加速器 & 手机功能
4. ###IOS高级
	+ ####**数据存储**
	  - Plist:UserDefault
	  - NSCoder
	  - SQLite
	  - Core Data
	  - JSON
	  - KeyChain
	+ ####**网络通信**
	  - Socket
	  - CFNetwork
	  - NSStream
	  - NSURLConnection
	  - 网络框架
	+ ####**多线程**
	  - NSThread
	  - NSOperation
	  - GCD
	+ ####**多媒体**
	  - 音频／音效及音频处理
	  - 视频
	  - 相机／相册
	  - 流媒体／滤镜
	4.5 多任务
5. ###常用类库及框架 


###IOS层级结构：
---
1. ###Cocoa Touch 最高层
2. ###Media 多媒体层
3. ###Core Services 服务层:
#####Foundation.Framework + CoreFoundation.Framework [Foundation:提供了一系列处理字符串，排列，组合，日历，时间等基本功能--C的API] *[提供Security,Core Location,SQLite,AddressBook]*
4. ###Core OS 最底层：
#####硬件驱动，内存管理，程序管理，线程管理，文件系统，网络，以及标准输入输出


###核心基础框架(CoreFoundation.framework) 提供iPhone应用的基本数据管理和服务功能：
---
1. #####Collection数据类型（Arrays／Sets等）
2. #####Bundles
3. #####字符串管理
4. #####日期和时间管理
5. #####原始数据块管理
6. #####首选项管理
7. #####URL和Stream操作
8. #####线程和运行循环
9. #####端口和Socket通信

###CFNetwork框架：
---
1. #####BSD Sockets
2. #####利用SSL或TLS创建
3. #####解析DNS Hosts
4. #####解析Http协议，鉴别HTTP和HTTPS服务器
5. #####在FTP服务器工作
6. #####发布／解析和浏览Bonjour服务

###Cocoa Touch框架:
---
1. #####UIKit.framework［最核心－－应用程序界面各种组件的呈现，屏幕的多点触摸事件，文字的输出，图片／网页的显示，相机／文件的存取，以及加速感应的部分等］
2. #####Foundation.framework[基础框架－－Collection数据类型，Bundles,字符串管理，时间／日期管理，原始数据块管理，首选项管理，线程和循环，URL和Stream处理，Bonjour，通信端口管理，国际化]
3. #####Address Book UI Framework
