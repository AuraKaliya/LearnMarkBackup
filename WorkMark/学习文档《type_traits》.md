## type_traits

#### std::is_polymorphic

检查类是否是多态类（含多态信息，如虚函数表）

```cpp
#include<type_traits>
#include<iostream>

class A
{
};
class B : public A {};
class C : public B {};
class D { public: virtual void test() {}; };
class E :public B, public D {};

int main()
{
	std::cout << "A:" << std::is_polymorphic<A>::value << std::endl;
	std::cout << "B:" << std::is_polymorphic<B>::value << std::endl;
	std::cout << "C:" << std::is_polymorphic<C>::value << std::endl;
	std::cout << "D:" << std::is_polymorphic<D>::value << std::endl;
	std::cout << "E:" << std::is_polymorphic<E>::value << std::endl;

	return 0;
}

/*
A:0
B:0
C:0
D:1
E:1
*/
```

