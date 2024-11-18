## Q_DECLARE_METATYPE

```cpp
Q_DECLARE_METATYPE(Type)
```

### 官方解释

为了让QMetaType知道有Type这个数据类型，并提供一个预设的拷贝构造函数和析构函数。QVariant需要使用Q_DECLARE_METATYPE这个宏来定制类型。(实现向QVariant进行注册)

使用该宏一般要求Type是一个完整的数据类型。可以使用Q_DECLARE_OPAQUE_POINTER()来注册一个指针用于转发声明类型。

【官方】一般把该宏放到结构体末尾或类的末尾，用以注册。

（或者单独开一个头文件放进去也行的）

在结构体、类中使用

```cpp
  struct MyStruct
  {
      int i;
      ...
  };
 
  Q_DECLARE_METATYPE(MyStruct)
```

在命名空间中使用

```cpp
  namespace MyNamespace
  {
      ...
  }
 
  Q_DECLARE_METATYPE(MyNamespace::MyStruct)
```

在QVariant中使用

```cpp
  MyStruct s;
  QVariant var;
  var.setValue(s); // copy s into the variant
 
  ...
 
  // retrieve the value
  MyStruct s2 = var.value<MyStruct>();
```



### 相关博客

[Qt文档阅读笔记-关于Q_DECLARE_METATYPE原理以及使用_qdeclaremetatype用法-CSDN博客](https://blog.csdn.net/qq78442761/article/details/82084295)