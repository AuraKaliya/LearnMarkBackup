### 一种通用的遍历tuple的方法

#### tuple介绍

#### C17折叠表达式

#### 第一版

使用多参模板和std::index_sequence 可以将tuple的各种信息分割后放入模板函数中，使用折叠表达式可提供一种通用的方案对tuple进行遍历。

```cpp
/*
此处模板函数功能为返回tuple的值的和
std::make_index_sequence<std::tuple_size_v<decltype(t)>>() 作用是生成基于tuple容器所含值的数量的一个序列。
std::make_index_sequence<5>() ==>std::index_sequence<0,1,2,3,4>
通常用于配合模板多参展开和化简递归方式
该模板是std::integer_sequnce<>的偏特化std::integer_sequnce<size_t,0,1,2,3,4>
(size_t 也可替换为char等)
*/

template <size_t... Idx, typename Tuple >
int VisitTuple(Tuple tuple, std::index_sequence<Idx...>)
{
    int num = 0;
   ((num += std::get<Idx>(tuple)), ...);
    return num;
}
int main()
{
    int num = VisitTuple(t,std::make_index_sequence<std::tuple_size_v<decltype(t)>>());
    std::cout << num << std::endl;
}
```

#### 第二版

第一版的((num += std::get< Idx >(tuple)), ...);语句本质上是一项方法对参数的顺序处理，使用C17中lambda的参数自动类型推导可实现功能的自定义和多元化。

```cpp
 template <size_t... Idx, typename Tuple,typename Function >
void VisitTuple(Tuple tuple,Function &&f, std::index_sequence<Idx...>)
{
    (f(std::get<Idx>(tuple)), ...);
}
int main()
{
        VisitTuple(t, 
                   [](auto&& elem) {
        std::cout << elem << std::endl;
        }, 
                   std::make_index_sequence<std::tuple_size_v<decltype(t)>>());
}
```

若需要设置传出参数，可直接使用捕获来实现。