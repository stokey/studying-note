### Android线程池：（1+4）
+ ThreadPoolExecutor[`1`]：主要通过构造函数来进行配置

```java
if  (当前任务线程 >= 核心线程)  {
	if (当前任务线程 >= 线程池任务队列大小[核心线程缓存区]) {
		if ( 当前任务线程 >= （核心线程数 + 非核心线程数 = 最大线程数）) {
			throw java.util.concurrent.RejectedExecutionException
		} else {
			// 开启一个非核心线程来执行任务
		}
	} else {
		// 进入任务队列中等待执行
	}
} else {
	// 开启一个核心线程去执行
}
```
+ FixedThreadPool：线程数量固定，空闲不会被回收，更快的响应速度，无超时机制，无大小限制[`需要大量长时间面向连接的线程`]
+ CachedThreadPool：最大线程数为`Int`最大值，超时时间为`60s`，没有核心线程。当整个线程池都处于闲置状态超过`60s`的时候，线程池会被回收，几乎不占用任何系统资源[`适合执行大量的耗时较少的任务`]
+ ScheduledThreadPool：核心数量固定，非核心数量无限制，非核心线程闲置时立即被回收[`执行定时任务和具有周期性的重复任务`]
+ SingleThreadExecutor：只有一个核心线程
`后面四个线程池底层由TheadPoolExecutor构建`

### WorkQueue：BlockingQueue类型
+ ArrayBlockingQueue：固定大小，FIFO
+ LinkedBlockingQueue：大小不确定，默认大小Integer.MAX_VALUE
+ PriorityBlockingQueue：和LinkedBlockingQueue类似，但是存取顺序按照元素Comparator来决定（`数据必须实现Comparator接口`）
+ SynchronousQueue：同步的队列，线程安全
