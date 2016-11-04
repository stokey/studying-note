##Android基础架构
---
### 目录结构
+ ####base目录
	+ BaseActivity
		+ Activity抽象基类
		+ 注册ViewServer
		+ 注册BaseApp
		+ 增加页面过渡动画
		+ startActivity(){`overridePendingTransition(R.anim.dock_right_enter, R.anim.dock_hold);`}
		+ startActivityForResult(){`overridePendingTransition(R.anim.dock_right_enter, R.anim.dock_hold);`}
		+ 自定义finish方法
			+ finish()
			+ finishAfterTransition():SDK>=21
	+ BaseFragment	
	+ BaseFragmentActivity
	+ BaseApp
		+ 继承自MultiDexApplication：支持多Dex
		+ getVersionName/Code
		+ initImageLoader/DisplayImageOptions.Builder
		+ getMetaData/getDataDir
		+ onApplicationCreate/Start/Stop/Destroy
		+ onActivityCreate/Start/Stop/Destroy(`mActivityCount/mForegroundActivityCount`)
	+ BaseBean
		+ 数据结构实际存储
		+ 定义静态类Immutable（`实现Parcelable接口`）
	+ BaseCursor
		+ Cursor包装类
		+ ContentObserver
		+ DatasetObserver

	+ BaseEvent
		+ EventBus抽象接口

	+ BaseManager
		+ 控制模块基类
		+ init/release方法

	+ BaseModel
		+ 模块数据操作接口
	+ BaseService
		+ Service抽象类
	+ BaseWebActivity
		+ 用于加载html页面Activity
		+ setWebSettings/setWebViewClient/setWebChromeClient 
+ ####constant
+ ####download
+ ####event
+ ####exception
+ ####module
+ ####network
+ ####provider
+ ####service
+ ####util
+ ####widget





### HierarchyViewer
+ Android自带的UI性能优化工具
+ 可视化的角度直观地获得UI布局设计结构和各种属性的信息
+ 出于安全考虑，Hierarchy Viewer只能连接Android开发版手机或者是模拟器的android系统。
+ 使用方式（`BaseActivity/BaseInputService`）
	+ onCreate(){ `ViewServer.get(this).addWindow(this)`};
	+ onResume()／onStartInput(){`ViewServer.get(this).setFocusedWindow(this)`};
	+ onDestroy(){`ViewServer.get(this).removeWindow(this)`};


### ContentObserver/DatasetObserver
+ ContentObserver：主要通过Uri来监听特定Databases的表，如果该Databases表有变动则会通知更新cursor中的数据
+ DatasetObserver：当注册它的cursor中发生变动时会调用其中的方法，让用户做一些界面刷新等操作

### Back键底层的实现是否调用了finish方法
	
		