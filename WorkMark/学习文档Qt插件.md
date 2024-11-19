### 插件基类

* 定义插件的IID

    ```cpp
    #define PluginManagerInterfaceIID "aura.engine.Plugin/1.0.0"
    //定义插件的IID，类似java中的包命名：组织机构域名加产品名加版本号
    ```

* 进行接口基类构造

    ```cpp
    class EnginePlugin
    {
    public:
        EnginePlugin()=default;
        virtual ~EnginePlugin()=default;
        virtual void initPlugin()=0;
        virtual void work()=0;
        virtual QWidget* getWidget()=0;
        virtual QString getPluginName()=0;
        virtual void initApp(UIWidget*)=0;
    protected:
        QString m_pluginName    ="Default"    ;
        QWidget* m_widget       =nullptr      ;
        UIWidget* m_rootWidget  =nullptr      ;
    };
    ```

* 对接口类注册

    ```cpp
    Q_DECLARE_INTERFACE(EnginePlugin,PluginManagerInterfaceIID)
    //通过宏将该接口进行声明
    ```

    

### 插件管理类

* 初始化插件列表

    可通过统一的目录管理加载的插件，具体逻辑可自定义实现。（如获取目录下所有规范文件格式的dll）

* 加载插件

    使用`QPluginLoader`加载插件，使用插件基类获取具体对象。

    进行统一处理（初始化、加入到某消息队列里，交由某管理器进行管理）

    ```cpp
    for(auto it:m_pluginFiles)
        {
            QPluginLoader loader;
            loader.setFileName(m_pluginDir->filePath(it));
    
            bool loadFlag=loader.load();
            if(!loadFlag)
            {
                qDebug()<<"error: "<<"load "<<it<<":"<<loader.errorString();
                continue;
            }
    
            EnginePlugin * p=qobject_cast<EnginePlugin*>(loader.instance());
            if(p)
            {
                p->initPlugin();
                p->initApp(m_rootWidget);
    
                if(true)
                {
                    m_pluginWidgetList.append(p->getWidget());
                    p->work();
                }
            }
            m_pluginDictionary.insert(p->getPluginName(),p);
        }
    ```

    

* 

