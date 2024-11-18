## integral_constant

用于在编译期存储一个常量值，通常用于模板元编程中，以便在编译期进行常量计算和模板推导。



```cpp
//示例
#include <type_traits>
#include <iostream>

int main() {
    using five = std::integral_constant<int, 5>;
    using true_type = std::integral_constant<bool, true>;

    std::cout << "five::value = " << five::value << std::endl; // 输出 5
    std::cout << "true_type::value = " << true_type::value << std::endl; // 输出 1 (true)

    if (true_type::value) {
        std::cout << "This is true!" << std::endl;
    }

    return 0;
}

```



### 使用tuple存值

```cpp
template <int... values> struct to_int_types{
	typedef tuple< integral_constant<int, values>... > type;
};
```

