

### QOpenGLContext

用于管理OpenGL 上下文的生命周期。



##### 对OpenGL 上下文的生命周期管理

创建上下文：`QOpenGLContext::create()`创建 OpenGL 上下文

使用上下文：`QOpenGLContext::makeCurrent()` 将上下文设置为当前上下文

释放上下文：`QOpenGLContext::doneCurrent()` 释放当前上下文，设置其不再是当前的活动上下文（set(0)）

* 使用示例-【创建-设置-释放-销毁】

    ```cpp
    QOpenGLContext *context = new QOpenGLContext();
    context->create();
    context->makeCurrent(surface);
    
    // 执行OpenGL操作
    
    context->doneCurrent(); // 释放当前上下文
    delete context;         // 删除上下文对象，释放资源
    ```

    



### QOffscreenSurface

与QOpenGLContext一起使用，允许任意线程中使用OpenGL进行渲染，而不用创建QWindow

该类应只创建OpenGL资源，如`texture` 或 ` framebuffer objects`

该类应该在一个单独的线程中执行一些耗时任务，避免主渲染线程停滞。

在该类的上下文中创建的资源可与主OpenGL上下文共享，比如异步纹理渲染到`QOpenGLFramebufferObject`中。

注意：该类的`create（）`只在主线程（GUI）中调用，即创建和销毁在GUI线程中执行。