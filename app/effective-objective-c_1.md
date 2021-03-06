Effective Objective-C: 1-了解Objective-C语言的起源
===

`Objective-C`语言由消息型语言`Smalltalk`演变而来

## 消息与函数调用之间的区别

- 使用消息结构的语言，其运行时所应执行的代码由运行环境来决定；使用函数调用的语言，则由编译器决定
- 调用的函数是多态时，在运行时按照`虚拟方法表(virtual table)`来查出到底应该执行哪个函数实现；消息结构，总是在运行时才会去查找所要执行的方法，接收消息的对象也要在运行时处理，其过程叫做`动态绑定(dynamic binding)`

## 运行期组件

Objective-C的 __面向对象特性__ 所需的全部数据结构及函数都在`运行期组件`里面，比如包含`内容管理方法`等

`运行期组件`本质上是`动态库`

## 要点

- Objective-C 为C语言添加了面向对象特性，是其超集。
Objective-C使用动态绑定的消息结构，也就是说，在运行时才会检查对象类型。
接收一个消息之后，执行何种代码，油运行期环境而非编译器来决定。
- 理解C语言的核心概念有助于写好Objective-C程序。尤其要掌握内存模型和指针。
