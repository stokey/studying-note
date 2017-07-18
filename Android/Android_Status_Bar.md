### Aandroid状态栏
+ 沉浸式全屏模式：隐藏status bar使屏幕全屏，让Activity接收所有（整个屏幕）的触摸事件[`全屏时隐藏status bar，点击屏幕后显示status bar`]
+ 透明化系统状态栏：透明化系统状态栏，使得布局侵入系统栏的后面，必须启用fitSystemWindow属性来调整布局不至于被系统栏覆盖

系统状态栏跟随app主题颜色变化而变化[*`android 4.4开始引进，并且在5.0进行了改进`*]：
####解决方案：
+ 使用toolbar
	+ theme里添加style<item name="android:windowTranslucentStatus">true</item>(*`包含toolbar的内容布局就可以扩展至系统状态栏，状态栏会覆盖在toolbar上`*)
	+ 使用android:fitsSystemWindows="true",调整内容布局---给toolbar加上一个`paddingTop="25dp"` 

+ 不使用toolbar
	+ 原理：不管有没有action bar，内容布局的背景色一律设为主题颜色。如果有action bar，就将action bar与内容布局的颜色背景色同时设为主题颜色，然后每个内容布局的根布局都要设android:fitsSystemWindows="true"进行调整
 
