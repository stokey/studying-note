##IOS视频播放
---
+ 播放本地视频
+ 播放网络视频

IOS 9.0只支持https协议链接，如果要支持http协议链接：

`info.plist添加 NSAppTransportSecurity --> NSAllowsArbitraryLoads`

####使用到的类：
+ AVPlayerItem：一个媒体资源管理对象，管理者视频的一些基本信息和状态，如播放进度、缓存进度等。一个AVPlayerItem对应着一个视频资源
+ AVPlayer：视频操作对象，继承自NSObject，无法显示视频，需要把自己添加到一个AVPlayerLayer上
+ AVPlayerLayer：用来显示视频
+ AVAsset：用于获取多媒体的相关信息，包括获取多媒体的画面、声音等信息
+ AVURLAsset：AVAsset的子类，可以根据一个URL路径创建一个包含媒体信息的AVURLAsset对象

#### 操作步骤：
+ 获取文件URL
+ 通过文件URL生成一个AVPlayerItem
+ 监听状态改变
+ 将视频资源赋值给视频播放对象
+ 初始化视频显示layer
+ 设置显示模式

```
guard let url = NSURL()
AVPlayerItem playerItem = AVPlayerItem(URL:url)
//监听缓冲进度改变
playerItem.addObserver(self,forKeyPath:"loadTimeRanges",options:NSKeyValueObservingOptinons.New,context:nil)
//监听状态改变
playerItem.addObserver(self,forKeyPath:"status",options:NSKeyValueObservingOptions.New,context:nil)
AVPlayer avPlayer = AVPlayer(playerItem:playerItem)
//初始化视频显示layer
AVPlayerLayer playerLayer = AVPlayerLayer(player:avPlayer)
//设置显示模式
playerLayer.videoGravity = AVLayerVideoGravityResizeAspect
playerLayer.contentsScale = UIScreen.mainScreen().scale
```

#### AVAssetExportSession：导出视频工具类
+ AVURLAsset
+ AVAssetExportSession
+ NSSearchPathForDirectoriesInDomains
+ NSFileManager

#### NSNotificationCenter：通知中心，监控各种通知

```
//注册监听器
NSNotificationCenter.defaultCenter().addObserver(observer: AnyObject, selector aSelector: Selector, name aName: String?, object anObject: AnyObject?)
//取消注册
NSNotificationCenter.defaultCenter().removeObserver(self)
//监听视频播放完成后执行playerItemDidReachEnd方法
NSNotificationCenter.defaultCenter().addObserver(self,
          selector: #selector(VideoSplashViewController.playerItemDidReachEnd),
          name: AVPlayerItemDidPlayToEndTimeNotification,
          object: moviePlayer.player?.currentItem)
```

##IOS视频处理 [链接](http://www.jianshu.com/p/5433143cccd8)
+ 压缩视频
+ 添加水印
+ 旋转视频方向