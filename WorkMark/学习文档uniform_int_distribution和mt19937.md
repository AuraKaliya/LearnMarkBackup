## std::uniform_int_distribution

定义于< random >头文件中，用于生成随机整数。

通过构造新对象生成新的分布，可用reset重置分布的内部状态；a、b获取分布参数，min、max获取潜在极值。

模板参数IntType是整型参数，默认为int。

通过（std::mt19937）获取该标准下的随机数。

```cpp
//示例代码---6面骰子
#include <random>
#include <iostream>
 
int main()
{
    std::random_device rd;  // 将用于为随机数引擎获得种子
    std::mt19937 gen(rd()); // 以播种标准 mersenne_twister_engine
    std::uniform_int_distribution<> dis(1, 6);
 
    for (int n=0; n<10; ++n)
        // 用 dis 变换 gen 所生成的随机 unsigned int 到 [1, 6] 中的 int
        std::cout << dis(gen) << ' ';
    std::cout << '\n';
}
```

```cpp

namespace Engine {

static std::random_device s_randomDevice;
static std::mt19937_64 s_randEngine(s_randomDevice());
static std::uniform_int_distribution<uint64_t> s_uniformDistribution;

static std::mt19937 s_randEngine32(s_randomDevice());
static std::uniform_int_distribution<uint32_t> s_uniformDistribution32;

Uuid::Uuid() : m_uuid(s_uniformDistribution(s_randEngine))
{
}

Uuid::Uuid(uint64_t uuid)
    : m_uuid(uuid)
{
}

Uuid::Uuid(const Uuid& other)
    : m_uuid(other.m_uuid)
{
}


Uuid32::Uuid32()
    : m_uuid(s_uniformDistribution32(s_randEngine32))
{
}

Uuid32::Uuid32(uint32_t uuid)
    : m_uuid(uuid)
{
}

Uuid32::Uuid32(const Uuid32& other)
    : m_uuid(other.m_uuid)
{
}

}

```



## std::mt19937

C++标准库中的一个伪随机数**生成器**类，实现了梅森旋转算法，作为随机数生成引擎，以生成高质量的伪随机数序列。

### std::random_device

用于生成伪随机数生成器的种子，以提高更高的随机性。

```cpp
//代码
#include <chrono>
#include <random>
#include<iostream>
 
 
int main() {
	
	std::random_device rd; // 创建一个std::random_device对象
	
	unsigned int seed = rd(); // 生成一个随机的种子值
	
	std::mt19937 engine(seed); // 使用随机的种子值创建一个伪随机数生成器
  	
  	std::cout<<engine();
  	
	return 0;
}
```



### std::chrono

使用时间戳转化为整数形式作为种子值。

```cpp
//代码
#include <chrono>
#include <random>
#include <iostream>
 
unsigned int generateSeedFromTimestamp() {
  auto now = std::chrono::system_clock::now(); // 获取当前时间点
  auto timestamp = std::chrono::duration_cast<std::chrono::milliseconds>(now.time_since_epoch()); // 转换为毫秒级的时间戳
 
  return static_cast<unsigned int>(timestamp.count()); // 将时间戳转换为整数种子值
}
 
int main() {
  unsigned int seed = generateSeedFromTimestamp(); // 生成种子值
 
  std::mt19937 engine(seed); // 使用种子值初始化伪随机数生成器
 
  std::cout<<engine();
 
  return 0;
}
```

