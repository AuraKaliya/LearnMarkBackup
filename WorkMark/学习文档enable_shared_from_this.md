## std::enable_shared_from_this

模板类，定义于< memory >

```cpp
template<class T>class enable_shared_from_this;
```

让一个已被std::shared_ptr管理的对象在安全的情况下生成额外的std::shared_ptr实例，共享该对象的所有权.

若类T继承自std::enable_shared_from_this< T >，则会为该T提供成员函数shared_from_this。当T的对象被std::shared_ptr管理时，调用该函数，返回一个新的std::shared_ptr< T >对象，共享该对象的所有权。

### 使用场景

当类A被share_ptr管理、A中的成员函数里需要把当前类的对象作为参数传递时，需要传递一个指向自身的share_ptr。

替代this的传递。

智能指针设计之初是为了方便资源管理，所以该保持智能指针的语义，统一指针类型的使用（智能指针 or 原始指针）。

#### 注意事项

不能使用share_ptr< this >进行替代，该语句会造成两个非共享的share_ptr指向同一个对象，未增加引用计数，导致对象被析构两次。

```cpp
//错误实践
#include <memory>
#include <iostream>
 
class Bad
{
public:
	std::shared_ptr<Bad> getptr() {
		return std::shared_ptr<Bad>(this);
	}
	~Bad() { std::cout << "Bad::~Bad() called" << std::endl; }
};
 
int main()
{
	// 错误的示例，每个shared_ptr都认为自己是对象仅有的所有者
	std::shared_ptr<Bad> bp1(new Bad());
	std::shared_ptr<Bad> bp2 = bp1->getptr();
	// 打印bp1和bp2的引用计数
	std::cout << "bp1.use_count() = " << bp1.use_count() << std::endl;
	std::cout << "bp2.use_count() = " << bp2.use_count() << std::endl;
}  // Bad 对象将会被删除两次
```

```cpp
//正确实践
#include <memory>
#include <iostream>
 
struct Good : std::enable_shared_from_this<Good> // 注意：继承
{
public:
	std::shared_ptr<Good> getptr() {
		return shared_from_this();
	}
	~Good() { std::cout << "Good::~Good() called" << std::endl; }
};
 
int main()
{
	// 大括号用于限制作用域，这样智能指针就能在system("pause")之前析构
	{
		std::shared_ptr<Good> gp1(new Good());
		std::shared_ptr<Good> gp2 = gp1->getptr();
		// 打印gp1和gp2的引用计数
		std::cout << "gp1.use_count() = " << gp1.use_count() << std::endl;
		std::cout << "gp2.use_count() = " << gp2.use_count() << std::endl;
	}
	system("pause");
} 
```

