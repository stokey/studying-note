##ReactNative
===
+ Android首屏加载白屏问题
	+ 问题描述：App启动时出现白屏显现，时间大概1～3秒
	+ 问题分析：应用在启动时会将js bundle读取到内存中，并完成渲染，如果此时js bundle还没有完成装载并渲染，则会出现白屏现象
	+ 解决方案：增加启动屏解决白屏等待问题
		+ 实现思路
			+ App启动的时候控制ReactActivity显示启动屏
			+ 提供关闭启动屏的公共接口
			+ 在js的适当位置（程序初始化工作完成后）调用关闭启动屏接口
		+ 具体实现
			+ 创建自定义ReactActivity类：（复制源码）
			+ 修改`getUserDeveloperSupport方法`
			
	```
	mReactRootView = createRootView();//创建根试图
	mReactRootView.startReactApplication(
		getReactNativeHost().getReactInstanceManager(),
		getMainComponentName(),
		getLaunchOptions());//加载并渲染 js bundle
	setContentView(mReactRootView);
	``` 