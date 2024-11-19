## Q_ENUM



### 相关文档

[Qt中的枚举变量,Q_ENUM,Q_FLAG,Q_NAMESPACE,Q_ENUM_NS,Q_FLAG_NS以及其他_qt 枚举-CSDN博客](https://blog.csdn.net/qq_36179504/article/details/100895133)

[[Qt开发探幽（二）\]浅谈关于元对象，宏和Q_ENUM - 轩先生。 - 博客园 (cnblogs.com)](https://www.cnblogs.com/Leventure/p/17671697.html)

###  笔记内容

* Qt内核提供了枚举量到字符串的转换机制，使用一组宏命令完成枚举量的扩展功能。

    系列宏包括：

    ```TXT
    Q_ENUM
    Q_FLAG
    Q_ENUM_NS
    Q_FLAG_NS
    Q_DECLARE_FLAGS
    Q_DECLARE_OPERATORS_FOR_FLAGS
    ```

    

* 将枚举量注册到Qt的元对象系统内的方法

    命名空间内注册

    ```cpp
    //1 命名空间内注册
    namespace test_enum {
    	Q_NAMESPACE	//Q_NAMESPACE宏将整个命名空间注册进元对象列表中去
    		enum class Test_Enum_1 {
    		none,
    		open,
    		close,
    		stop
    	};
    	Q_ENUM_NS(Test_Enum_1) //Q_ENUM_NS宏将我们需要的枚举类型对象注册进
    }
    ```

    类内注册

    将枚举类型写入用到Q_OBJECT Q_GADGET Q_GADGET_EXPORT标记的类内

    Q_GADGET可以有Q_ENUM、Q_PROPERTY和Q_INVOKABLE，但不能有信号或插槽。

    Q_GADGET使类成员staticMetaObject可用。staticMetaObject的类型为QMetaObject，并提供对用Q_ENUM声明的枚举的访问。

    



### 其它

通过 Qt的元对象类型将一个枚举类型的成员转换成字符串，或者将字符串转回枚举类型的值

