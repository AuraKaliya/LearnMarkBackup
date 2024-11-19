## EnTT

entt 是由现代C++编写的仅头文件的ECS库

其中，E-Entity实体，C-Component组件，S-system系统。

一般来说，ECS系统包含以下几个部分。

* World ： 管理整个ECS数据的部分，在Entt中为Registry，==entt::registry==
* Querier：查询器，用来得到含有某个特定组件的实体，或从实体得到组件，在Entt中是View，==entt::view==

* Resource：资源，本质上是组件，但不附加在任何实体上，全局只存在共有的一份。在Entt中是Resource_cache，==entt::resource_cache==
* Syestem：系统，用于对组件进行实际操作，即算法实现的部分。Entt中无特定类型，自行编写。

### Component

代表了附加在Entity上的数据。

一般来说，Component应为纯数据类型，不包含任何行为。

在Entt中没有为Component指定一个公共的基类，只是用模板做了限制。



### Registry

将Entity、Component、System组织起来，用于存储和管理所有的Entity和Component。

类似其他的CES实现中的World。

使用的是std::allocator作为内存分配器。

```cpp
enum class entity : id_type {};

template<typename Entity = entity, typename = std::allocator<Entity>>
class basic_registry;

/*! @brief Alias declaration for the most common use case. */
using registry = basic_registry<>;
```

* 创建/销毁Entity

    ```cpp
    // constructs a naked entity with no components and returns its identifier
    auto entity = registry.create();
    
    // destroys an entity and all its components
    registry.destroy(entity);
    
    registry.clear();
    ```

* 筛选Entity

    ```cpp
    // destroys all the entities in a range
    auto view = registry.view<a_component, another_component>();
    //筛选出所有同时具有a_component和another_component的Entity.
    registry.destroy(view.begin(), view.end());
    //销毁
    ```

* 为Entity添加Component

    ```cpp
    registry.emplace<position>(entity, 0., 0.);
    
    // ...
    
    auto &vel = registry.emplace<velocity>(entity);
    vel.dx = 0.;
    vel.dy = 0.;
    //emplace 用于原地构造Component
    
    // default initialized type assigned by copy to all entities
    registry.insert<position>(first, last);
    
    // user-defined instance assigned by copy to all entities
    registry.insert(from, to, position{0., 0.});
    //insert 用于插入现成的Component
    ```

* 查询Component是否存在

    ```cpp
    // true if entity has all the given components
    bool all = registry.all_of<position, velocity>(entity);
    
    // true if entity has at least one of the given components
    bool any = registry.any_of<position, velocity>(entity);
    ```

* 删除Component

    ```cpp
    registry.erase<position>(entity);
    registry.remove<position>(entity);
    registry.clear<position>();
    ```

* 从Entity上获取Component

    ```cpp
    const auto &cregistry = registry;
    
    // const and non-const reference
    const auto &crenderable = cregistry.get<renderable>(entity);
    auto &renderable = registry.get<renderable>(entity);
    
    // const and non-const references
    const auto [cpos, cvel] = cregistry.get<position, velocity>(entity);
    auto [pos, vel] = registry.get<position, velocity>(entity);
    
    //try_get get 用于获取
    ```

* 
