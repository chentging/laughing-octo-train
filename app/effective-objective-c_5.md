Effective Objective-C: 5-用枚举表示状态/选项/状态码
===

枚举只是一种常量命名方式。某个对象所经历的各种状态就可以定义为一个简单的枚举集。

C++11 标准修订了枚举的某些特性。其中一项改动：可以指明用何种`底层数据类型(underlaying type)` 来保存枚举类型的变量。

## 要点

- 应该用枚举来表示状态机的状态，传递给方法的选项以及状态码等值，给这些值起个易懂的名字。
- 如果把传递给某个方法的选项表示为枚举类型，而多个选项又可同时使用，那么就将各选项值定义为2的幂，以便通过按位或操作将其组合起来。
- 用`NS_ENUM`与`NS_OPTIONS`宏来定义枚举类型，并指明其底层数据类型。这样做可以确保枚举是用开发这所选的底层数据类型实现出来的，而不会采用编译器所选的类型。
- 在处理枚举类型的`switch`语句中不要实现`default`分支。这样的话，加入新枚举之后，编译器就会提示开发者：switch语句并未处理所以枚举。