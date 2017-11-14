Android下玩JNI的新老三种姿势
===

tags:
- NDK
- Android
- JNI

[**转**] -> [(原址)](http://blog.csdn.net/mabeijianxi/article/details/68525164)

原作者： [剑西](http://blog.csdn.net/mabeijianxi)

> 说明：本篇不撸代码，只玩编译,其包含了Android studio 2.2最新的JNI玩法
编译环境：macOS 10.12.3
工具包含：Android Studio 2.2  NDK-r14 

只需如图勾选然后OK即可，我的版本是r14，值得一提的是 google ndk-build 命令在 r13 后默认使用 Clang，并将在后续版本中移除 GCC，其编译速度更快、编译产出更小、出错提示更友好。

1. 徒手编写Android.mk然后ndk-build编译：
1. 通过配置AS中build.gradle来编译：
1. 通过AS的cMake插件编译：

---

Android NDK开发之旅(2)：一篇文章搞定Android Studio中使用CMake进行NDK/JNI开发
===

tags:
- NDK
- Android
- JNI

[**转**] -> [(原址)](http://blog.csdn.net/AndrExpert/article/details/72904462)

原作者： [裂缝中的阳光dg](http://my.csdn.net/AndrExpert)

1. 安装CMake、LLDB
1. 配置NDK路径
1. 创建支持C/C++开发的Android工程
1. 运行效果
1. 创建新的C文件
1. 本地代码调试


----

 Android NDK开发之旅(3)： 详解JNI数据类型与C/C++、Java之间的互调
 ===
 
 tags:
- NDK
- Android
- JNI

[**转**] -> [(原址)](http://blog.csdn.net/andrexpert/article/details/72851294)

原作者： [裂缝中的阳光dg](http://my.csdn.net/AndrExpert)


1. JNI数据类型
	2. JNI数据类型
	3. 函数原型解析
1. Java调用C/C++本地函数
	2. C or C++
	3. JNI中字符串处理
1. C/C++访问Java层方法、属性
	1. C/C++层访问对象的属性
	2. C/C++层访问Java类的静态属性
	3. C/C++层访问Java对象的方法
	4. C/C++层访问Java类的静态方法
	5. C/C++层实现指向子类对象访问父类的方法


GitHub项目地址：[https://github.com/jiangdongguo/JniLearning](https://github.com/jiangdongguo/JniLearning)