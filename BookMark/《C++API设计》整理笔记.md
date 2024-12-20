# 《C++ API 设计》

## 简述

API提供了对某个问题的**抽象**，以及**客户**与解决问题的**软件组件**之间**交互的方式**。

客户端的代码只能通过API的**公有接口**访问。

* 设计理念

    API是为开发者设计的接口，如同GUI是为用户设计的接口一样。

    多个应用程序可以同享一个API。

    修改API时，应尽可能保持向后兼容，应该具有可变更的控制流程。

    良好的文档和自动化测试。

* API的优势

    隐藏实现，延长系统寿命，促进系统模块化，减少代码量，易于维护和改变实现，易于优化。



## 优质的API特征

设计精巧实用性强。

* 提供良好的抽象

    依照文档，开发者可以理解接口中的概念并知道它的工作机制。

    可以使用统一建模语言（UML）描述API结构。

    可以对关键对象进行建模，但不要尝试创建比需求更通用的对象模型。

* 隐藏实现的细节

    分文件式存储。

    提供公有接口getter/setter访问私有成员变量。

    不要返回私有数据成员的非const指针或引用。（会破坏封装性）

    在类中private定义整个程序流程中不会访问到、而API会用到的原子结构。（如在火焰类中**私有封装**一个结构体火焰粒子结构。）

* 最小完备性

    若无必要，勿增实体。API的功能设计应该在满足需求上尽量逻辑清晰简洁，而不是以此为基础再高度抽象。

    * 谨慎添加虚函数

        如果不了解API的虚函数构建的代码就孤立地升级基类，可能导致脆弱基类问题。

        重写虚函数可能破坏类的内部完整性，可能会相较于发行的API少调用一个或多个必要的句柄。

        虚函数的调用必须要在运行时通过查询虚函数表确定，而非虚函数的调用在编译时就可确定。

        将虚函数声明为内联是没有意义的，因为虚函数是运行时确定，而内联是编译时进行的优化。

        没有虚函数的类比有虚函数的类更健壮且易于维护，通常使用模板方法设计模式来替代虚函数的设计。

    * 简洁的流程与复杂的实现步骤

        代码块一旦被复制，就会存在潜在的代码不一致问题。

        对于流程应尽可能简洁，通过调用功能接口的方式面向过程开发，功能的实现应尽可能详细。

* 易用性和一致性

    文档、编码规范。

    在迭代过程中遵循一定的编码规范。

* 松耦合高内聚

    * 管理器类

        通过使用一个管理器类作为API对低层数据的管理的中间平台

    * 回调、观察者和通知

        当前接口的功能实践内发生的一些事需要通知其他类。

        回调是模块A的一个函数指针，该指针被传给模块B，这样模块B就能在合适的时间调用A中的函数，且B对A一无所知，不存在对A的包含或链接依赖。在大型项目中，**回调函数是打破循环依赖的一项技术**。

        在C++中存在一个回调难点，即使用非静态方法作为回调会显得复杂，因为通常情况下，this指针也需要进行传递。所以一般来说，观察者模式更优先。

    * 







