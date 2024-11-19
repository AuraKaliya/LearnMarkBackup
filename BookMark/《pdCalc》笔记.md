

## Publisher-Observer  发布者-观察者模式

### Publisher

#### 结构

* impl式
* 对外【public】开放【添加/删除】某事件的观察者、获取事件列表、事件的观察者列表等接口，对内【protected】开放事件注册、事件通知的接口。
* 在项目中处于最底层，作为基类和模块进行配置。
* 使用嵌套的哈希表unordered_map管理【事件-观察者名-观察者】
* 

#### 功能

* 注册功能

    将事件注册进表中，每新建一个数据默认创建一章观察者表。

    检查：当已存在需要注册的数据时，通过throw抛出一个Exception

    ```cpp
    void Publisher::PublisherImpl::registerEvent(const string& eventName)
    { 
        auto i = events_.find(eventName);
        if( i != events_.end() )
            throw Exception{"Event already registered"};
        
        events_[eventName] = ObserversList{};
      
        return;
    }
    ```

* 添加 & 删除事件的观察者

* 通知

    轮询通知事件对应的观察者们

* 提供基本信息

    当前事件列表、事件的观察者表

#### 其他

* 可使用using来简化容器拼写

    ```cpp
    using ObserversList = std::unordered_map<string, unique_ptr<Observer>>;
    using Events = std::unordered_map<string, ObserversList>;
    ```



### Observer

接口类。

#### 结构

* 一个observerName用于定义其名称
* 一个实现接口，用于在通知时执行对应功能。



