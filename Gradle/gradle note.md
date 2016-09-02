##Gradle简介
---

+ apply plugin:'java'：表示应用引入了Gradle的Java插件
+ 简单Android项目的build.gradle文件
	
	```
build script {
	repositories{
		mavenCentral()
	}
	dependencies{
		classpath 'com.android.tools.build:grade:2.0.10'
	}
}
apply plugin:'android'
android{
	compileSdkVersion 19
	buildToolsVersion '19.0.0'
}
	```
	+ *buildscript{...}*－－配置驱动构建过程的代码。在这个部分声明了适用Maven仓库，并且声明了一个maven文件的依赖路径（`gradle插件`）
	+ *apply plugin*：增加插件
	+ *android{...}*－－配置所有android构件过程需要的参数
+ gradle属于任务驱动型构建工具，它的构建过程时基于Task的。
	+ assemble：将会组合项目的所有输出
	+ check：将会执行所有检查
	+ build：将会执行assemble和check两个task的所有工作
	+ clean：将会清空项目的输出
	
	控制台命令格式:`./gradlew xxx`
	
+ 配置manifest选项：
	1. minSdkVersion
	2. targetSdkVersion
	3. versionName
	4. versionCode
	5. applicationId
	6. package name for the test application
	7. signingConfig 
	
	```
android {
	compileSdkVersion 19
	buildToolsVersion '19.0.0'
	defaultConfig {
		versionCode 12000
		versionName '1.2.0'
		minSdkVersion 16
		targetSdkVersion 16
	}
}
//使用applicationId来配置manifest文件中的packageName属性。（为了消除应用程序的PackageName和Java包名引起的混乱）
//构建文件是动态的，可以从一个文件或者其它自定义的逻辑代码中读取版本信息
def computeVersionName(){
	...
}
android {
	compileSdkVersion 19
    buildToolsVersion "19.0.0"
    defaultConfig {
        versionCode 12
        versionName computeVersionName()
        minSdkVersion 16
        targetSdkVersion 16
    }
}
//不要使用与在给定范围内的同名getter方法，否则可能引起冲突。defaultConfig{...}中调用get VersionName()将会自动调用defaultConfig.getVersionName()方法而不是自定义的getVersionName()方法
	```
+ BuildType：配置Debug和Release版本相关信息
	
	```
android {
	buildTypes {
		debug {
			applicationIdSuffix ".debug"//设置包名为<app applicationId>.debug，以便能够在同一设备上安装debug和release版本的apk
		}
	}
}
	```
+ 签名配置
	+ keystore文件
	+ keystore密码
	+ key alias
	+ key 密码
	+ 签名文件的类型

	```
android {
	signingConfigs {
		debug {
			storeFile file("debug.keystore")
		}
		myConfig {
			storeFile file('other.keystore')
			storePassword 'android'
			keyAlias 'androidDebugKey'
			keyPassword 'android'
		}
	}
	buildTypes {
		debug {
			 signingConfig signingConfigs.debug
		}
		release{
			signingConfig signingConfigs.myConfig
		}
	}
}
	```
+ 本地包依赖
	+ compile：编译主module
	+ androidTestCompile：编译主module的测试
	+ debugCompile：debug Build Type的编译
	+ releaseCompile：release Build Type的编译

	```
dependencies {
	compile fileTree(dir:'libs',include:['*.jar'])
}
	```
+ 远程包依赖：Gradle支持从Maven或lvy仓库中拉取依赖文件
+ buildTypes相关属性说明：
	+ minifyEnabled：是否混淆
	+ zipAlignenabled：是否zip对齐
	+ applicationId：包名
	+ shrinkResources：移除未使用的资源
	+ multiDexEnabled：支持多Dex