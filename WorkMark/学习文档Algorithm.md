## Algorithm



### std::all_of 、std::any_of、std::none_of

检查给定范围内所有元素是否满足算法的条件。

```cpp
#include <iostream>
#include <vector>
#include <algorithm> // 包含 std::all_of
#include <functional> // 包含 std::greater

int main() {
    std::vector<int> numbers = { 2, 4, 6, 8, 10 };

    // 检查所有元素是否都是偶数
    bool all_even = std::all_of(numbers.begin(), numbers.end(),[](int i) { return i % 2 == 0; });

    if (all_even) {
        std::cout << "所有元素都是偶数。" << std::endl;
    }
    else {
        std::cout << "存在奇数元素。" << std::endl;
    }

    bool any_even = std::any_of(numbers.begin(), numbers.end(), [](int i) { return i % 2 == 1; });

    if (any_even) {
        std::cout << "存在奇数元素。" << std::endl;
    }
    else {
        std::cout << "所有元素都是偶数。" << std::endl;
    }

    bool none_even = std::none_of(numbers.begin(), numbers.end(), [](int i) { return i % 2 == 1; });

    if (none_even) {
        std::cout << "所有元素都是偶数。" << std::endl;
    }
    else {
        std::cout << "存在奇数元素。" << std::endl;
    }

    return 0;
}
```

### std::binary_search

一个查找算法，使用二分法查找给定范围内是否包含特定值，要求元素已排序。

```cpp
#include <iostream>
#include <vector>
#include <algorithm> // 包含 std::binary_search

int main() {
    std::vector<int> numbers = { 1, 3, 5, 7, 9, 11 };

    int value_to_find = 5;

    // 使用 std::binary_search 检查是否包含特定值
    bool found = std::binary_search(numbers.begin(), numbers.end(), value_to_find);

    if (found) {
        std::cout << "找到了值 " << value_to_find << "。" << std::endl;
    }
    else {
        std::cout << "未找到值 " << value_to_find << "。" << std::endl;
    }

    return 0;
}

```



### std::make_heap

将一定范围内的元素创建成堆序。

```cpp
// range heap example
#include <iostream>     // std::cout
#include <algorithm>    // std::make_heap, std::pop_heap, std::push_heap, std::sort_heap
#include <vector>       // std::vector

int main () {
  int myints[] = {10,20,30,5,15};
  std::vector<int> v(myints,myints+5);

  std::make_heap (v.begin(),v.end());
  std::cout << "initial max heap   : " << v.front() << '\n';

  std::pop_heap (v.begin(),v.end()); v.pop_back();
  std::cout << "max heap after pop : " << v.front() << '\n';

  v.push_back(99); std::push_heap (v.begin(),v.end());
  std::cout << "max heap after push: " << v.front() << '\n';

  std::sort_heap (v.begin(),v.end());

  std::cout << "final sorted range :";
  for (unsigned i=0; i<v.size(); i++)
    std::cout << ' ' << v[i];

  std::cout << '\n';

  return 0;
}
```

