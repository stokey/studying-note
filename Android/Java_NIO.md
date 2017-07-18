## [Java NIO](http://blog.csdn.net/tzs_1041218129/article/details/54917857)
----
是从Java 1.4版本开始引入的一个新的IO API。可替代标准的Java IO API。

+ NIO与标准IO区别
	+ Channels and Buffers：标准的IO基于字节流和字符流进行操作，而NIO是基于Channel和Buffer进行操作。数据总是通过Channel读取到缓冲区中，或者从缓冲区写入到通道中。
	+ Asynchronous IO：NIO可以异步使用IO
	+ Selectors：选择器用于监听多个通道的事件（例如：连接打开，数据到达）
