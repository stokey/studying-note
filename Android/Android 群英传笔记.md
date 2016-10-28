## Android 群英传笔记
---
+ Android系统架构
	+ Linux内核层：最底层最核心部分
		+ 硬件驱动
		+ 进程管理
		+ 安全系统
		+ 等等 
	+ 库和运行时
		+Dalvik与ART（`Android 5.X版本以后ART取代Dalvik`）
		
		+ 两者区别： 
	+ Framework层
	+ 应用层

![Android系统架构图](./images/android_struct.jpg)

+ Context创建时间：（创建Context的实现类的时候）
	+ 创建Application
	+ 创建Activity
	+ 创建Service
getApplicationContext()方法获取的是整个App的Context
+ Makefile：自动化编译Android代码（可定义编译规则，打包规则等）
+ Android系统目录
	+ /system/ 目录 
		+ /system/app/：系统App文件目录
		+ /system/bin/：Linux自带组件目录
		+ /system/build.prop：系统的属性信息
		+ /system/fonts/：系统字体存放目录（root后可下载TTF格式字体替换原字体，从而到达修改系统字体效果）
		+ /system/framework/：系统的核心文件、框架层
		+ /system/lib/：存放几乎所有的共享库(so)文件
		+ /system/media/：存放系统提示音、系统铃声
		+ /system/usr/  ：用户配置文件（键盘布局、共享、时区文件等）
	+ /data/(用户大部分数据信息)
		+ /data/app/：用户安装的App或者升级的App
		+ /data/data/：App的数据信息、文件信息、数据库信息等（以包名来区分各个应用）
		+ /data/system/：包含手机的各项系统信息
		+ /data/misc/：保存了大部分的WI-FI、VPN信息
+ View（以树形结构形式存在，上层控件负责下层控件的测量与绘制，并传递交互事件） 
	+ Android界面的架构图：(自顶向下)
		+ Activity：每个Activity都包含一个Window对象
			+ PhoneWindow：Android中Window对象
				+ DecorView：整个应用窗口的根View，封装了一些窗口操作的通用方法（所有的View的监听事件，都通过Window ManagerService来接收，并通过Activity对象来回调相应的onClickListener）
					+ TitlteView
					+ ContentView ：ID为content的FrameLayout，activity_main.xml就是设置在这里面
