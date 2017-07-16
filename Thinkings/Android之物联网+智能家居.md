# Android之物联网+智能家居

## 专业技能要点

+ 精通底层协议（网络TCP/UDP，蓝牙协议，Wi-Fi协议等等）
+ 精通JNI编程
+ 精通C/C++

## 精通JNI的原因
+ 当应用程序需要调用硬件特定的功能时，此时只能通过C或C++封装对应功能的JNI接口来供上层调用
+ 现阶段主要轮子都是用C/C++写的

## JNI开发流程
+ 编写声明了native方法的Java类
+ 将Java源代码编译成class字节码文件
+ 用javah -jni命令生成.h头文件（-jni参数表示将class中用native声明的函数生成JNI规则的函数）
+ 用本地代码实现.h头文件中的函数
+ 将本地代码编译成动态库（Windows: *.dll, Linux/Unix:\*.os, mac OS X:\*.jnilib）
+ 拷贝动态库至java.library.path本地库搜索目录下，并运行Java程序