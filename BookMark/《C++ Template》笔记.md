

* 当依赖于模板参数内类型的对象需要在模板内定义时，应用typename进行修饰。

    ```cpp
    template<typename T>
    class MyClass
    {
        typename T::SubType * ptr;
    }
    ```

    在未添加typename时，会被认为是T中的静态对象SubType 和ptr的乘积。

* 模板实例化

    包含模型：将模板的声明和定义放于一个.hpp文件中，而非需要在每个使用模板的.cpp文件中包含模板的实现文件.cpp。

    ```cpp
    
    class MyClass
    {
        template<typename T> 
        void func(T value)
        {
            solveValue(value);
    	}
    }
    ```

    分离模型：使用export将模板进行导出，包括在一个文件里定义模板，并在模板的定义和声明前添加export。

    ```cpp
    export
    template<typename T>
        void print(T const & context);
    //仅声明即可使用导出
    ```

    export不适用于内联（包括隐式和显式）

    export不能和inline一起使用，若用于模板，应放在template之前。

* 预编译头文件

    将很少或不会变动的头文件组织到一个头文件中，通过使用该头文件来实现头文件中声明功能的使用。

* 模板特化

    模板特化和实例化不同。实例化是通过使用具体的值替换模板参数，从模板产出普通类、函数、成员函数等；特化是引入一个template<>实现模板的特定的替换。

    ```cpp
    template<typename T1,typename T2>
    class MyClass{};
    
    //显式特化
    template<>
    class MyClass<std:string,float>{};
    
    //局部特化
    template<T>
    class MyClass<std::string,T>{};
    ```

    

* 虚函数不可声明为模板

    虚函数表在预编译时确定，而模板实例化个数要等程序编译完成后才确定。

* 模板不能具有C链接

    即不能使用extern C来描述模板。

* 非类型模板参数

    在编译期或链接期可以确定的常值。包括整型或枚举、指针类型、引用类型。

    ```cpp
    template<typename T ,	//类型参数
    	typename T::Allocator* Allocator>//非类型模板参数
    class List;
    ```

    函数和数组也可被声明为非类型模板参数，需要先隐式转换为指针类型，也就是decay。

    ```cpp
    template<int buf[5]> class Lexer;
    template<int *> class Lexer;
    //两者等价，int*算是int buf[5]的重新声明
    ```

    非类型模板参数只能具有const 、volatile限定符，不能有static、mutable等。

