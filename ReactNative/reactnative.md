##ReactNative
===
+ Android首屏加载白屏问题
	+ 问题描述：App启动时出现白屏显现，时间大概1～3秒
	+ 问题分析：应用在启动时会将JS bundle读取到内存中，并完成渲染，如果此时JS bundle还没有完成装载并渲染，则会出现白屏现象
		
		```
	mReactRootView = createRootView();//创建根试图
	mReactRootView.startReactApplication(
		getReactNativeHost().getReactInstanceManager(),
		getMainComponentName(),
		getLaunchOptions());//加载并渲染 JS bundle
	setContentView(mReactRootView);
	``` 
	+ 解决方案：增加启动屏解决白屏等待问题
		+ 实现思路
			+ App启动的时候控制ReactActivity显示启动屏
			+ 提供关闭启动屏的公共接口
			+ 在js的适当位置（程序初始化工作完成后）调用关闭启动屏接口
		+ 具体实现
			+ 创建自定义ReactActivity类：（复制源码）
			+ 修改`getUserDeveloperSupport方法`(getUseDeveloperSupport方法是受保护类型，我们无法在它所属包以外访问到该方法。此时必须通过反射机制进行处理)
			
			```
		protected boolean getUseDeveloperSupport(){
			ReactNativeHost rn = ((ReactApplication) 			getApplication()).getReactNativeHost();
        	Class<?>cl = rn.getClass();
    		Object support= null;
    		try {
        		Method method = cl.getDeclaredMethod("getUseDeveloperSupport", new Class[]{});
        		method.setAccessible(true);
        		support = method.invoke(rn);
    		} catch (NoSuchMethodException e) {
        		e.printStackTrace();
    		} catch (InvocationTargetException e) {
        		e.printStackTrace();
    		} catch (IllegalAccessException e) {
        		e.printStackTrace();
    		}
    		return (boolean) support;
		}
			```
			
			+  创建View容器，装载启动屏视图和React Native根视图
			
			```
		@verride
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			mRootView = new FrameLayout(this);//创建View容器
        	splashView = LayoutInflater.from(this).inflate(R.layout.launch_screen, null);//启动页视图
        	mReactRootView = createRootView();
        	mReactRootView.startReactApplication(
                getReactNativeHost().getReactInstanceManager(),
                getMainComponentName(),
                getLaunchOptions());
                //注意添加顺序
        	mRootView.addView(mReactRootView);
        	mRootView.addView(splashView, new ViewGroup.LayoutParams(
                ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT));
        	setContentView(mRootView);
        	mDoubleTapReloadRecognizer = new DoubleTapReloadRecognizer();
      	}
			```
			
			+ 提供切换界面接口（关闭启动页）
			
			```
			//JS不能直接调用Java方法，需要搭建Native Modules
		public void hide() {
    		if (mRootView == null || splashView == null) return;
    		AlphaAnimation fadeOut = new AlphaAnimation(1, 0);
    		fadeOut.setDuration(1000);
    		splashView.startAnimation(fadeOut);
    		fadeOut.setAnimationListener(new 			Animation.AnimationListener() {
    			@Override
        		public void onAnimationStart(Animation animation) {
        		}
		       @Override
        		public void onAnimationEnd(Animation animation) {
            		mRootView.removeView(splashView);
            		splashView = null;
        		}
        		@Override
        		public void onAnimationRepeat(Animation animation) {
        		}
    		});
		}
			```
			
			+ 创建LaunchScreenModule提供hide方法给JS调用
			+ JS调用LaunchScreenModule的hide方法
+ iOS白屏(默认会加载默认启动页)
	+ 问题描述：问题等同Android白屏
	+ 解决方案：等同Android也是通过增加启动页
	+ 具体实现：Xcode中编辑LaunchScreen.xib
	
+ 可取消异步操作
	+ 可取消Promise
	+ 可取消网络请求fetch(fetch返回的是一个Promise)	
	
	```
	//为Promise包裹一层可取消的外衣
const makeCancelable = (promise)=>{
	let hasCanceled = false;
	const wrappedPromise = new Promise((resolve,reject)=>{
		promise.then((val)=> hasCanceled ? reject({isCanceled: true}) : resolve(val));
		promise.catch((error)=> hasCanceled ? reject({isCanceled : true}) : reject(error));		
		});
	 return {
	 	promise: wrappedPromise,
	 	cancel(){
	 		hasCanceled = true;
	 	},
	 };
};
	```