## CMake



### 使用CMake编译库并安装(install)

* 设置输出目录和pakage目录

    ```cmake
    set(MODULE_OUTPUT_PATH "${CMAKE_CURRENT_SOURCE_DIR}")
    set(PACKAGE_PREFIX_DIR "${MODULE_OUTPUT_PATH}/plugin")
    ```

* 设置取消前缀

    ```cmake
    set(CMAKE_STATIC_LIBRARY_PREFIX "")
    set(CMAKE_SHARED_LIBRARY_PREFIX "")
    set(CMAKE_SHARED_MODULE_PREFIX "")
    ```

* 整合源代码和头文件

    ```cmake
    file(GLOB_RECURSE AURAUI_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/include/AuraComponentUI.h)
    file(GLOB_RECURSE UI_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/include/UI/*.h ${CMAKE_CURRENT_SOURCE_DIR}/include/UI/*.hpp)
    file(GLOB_RECURSE TOOL_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/include/TOOL/*.h ${CMAKE_CURRENT_SOURCE_DIR}/include/TOOL/*.hpp)
    file(GLOB_RECURSE UI_SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)
    
    set(SRC_FILES
        ${AURAUI_HEADERS}
        ${UI_HEADERS}
        ${TOOL_HEADERS}
        ${UI_SOURCES}
    )
    ```

* 添加编译过程：动态库、静态库

    ```cmake
    # 动态库
    add_library(AuraComponentUI SHARED
        ${SRC_FILES}
    )
    set_target_properties(AuraComponentUI PROPERTIES OUTPUT_NAME "AuraComponentUI")
    set_target_properties(AuraComponentUI PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${MODULE_OUTPUT_PATH}/plugin/lib)
    
    # 静态库
    add_library(AuraComponentUIStatic STATIC
        ${SRC_FILES}
    )
    set_target_properties(AuraComponentUIStatic PROPERTIES OUTPUT_NAME "AuraComponentUI")
    set_target_properties(AuraComponentUIStatic PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${MODULE_OUTPUT_PATH}/plugin/arc PREFIX "")
    ```

* 链接外部库

    ```cmake
    # 链接库
    target_link_libraries(AuraComponentUI PRIVATE
        Qt${QT_VERSION_MAJOR}::Widgets
        Qt${QT_VERSION_MAJOR}::PrintSupport
    )
    
    target_link_libraries(AuraComponentUIStatic PRIVATE
        Qt${QT_VERSION_MAJOR}::Widgets
        Qt${QT_VERSION_MAJOR}::PrintSupport
    )
    ```

* 搜索头文件

    ```cmake
    # 使用相对路径
    target_include_directories(AuraComponentUI PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    )
    
    target_include_directories(AuraComponentUIStatic PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    )
    ```

* 安装

    ```cmake
    # 安装目标
    install(TARGETS
        AuraComponentUI
        AuraComponentUIStatic
            EXPORT AuraComponentUITargets
            LIBRARY DESTINATION "${MODULE_OUTPUT_PATH}/plugin/lib"
            ARCHIVE DESTINATION "${MODULE_OUTPUT_PATH}/plugin/arc"
            INCLUDES DESTINATION "include")
    
    install(EXPORT AuraComponentUITargets
            FILE AuraComponentUITargets.cmake
            DESTINATION "${MODULE_OUTPUT_PATH}/plugin")
    
    install(FILES "${CMAKE_CURRENT_SOURCE_DIR}/AuraComponentUIConfig.cmake"
            DESTINATION "${MODULE_OUTPUT_PATH}/plugin")
    ```

    

* 完整CMakeList文件

    ```Cmake
    cmake_minimum_required(VERSION 3.14)
    
    project(AuraComponentUI LANGUAGES CXX)
    
    set(CMAKE_AUTOUIC ON)
    set(CMAKE_AUTOMOC ON)
    set(CMAKE_AUTORCC ON)
    set(CMAKE_CXX_STANDARD 17)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
    
    set(MODULE_OUTPUT_PATH "${CMAKE_CURRENT_SOURCE_DIR}")
    
    set(PACKAGE_PREFIX_DIR "${MODULE_OUTPUT_PATH}/plugin")
    
    set(CMAKE_STATIC_LIBRARY_PREFIX "")
    set(CMAKE_SHARED_LIBRARY_PREFIX "")
    set(CMAKE_SHARED_MODULE_PREFIX "")
    
    
    find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
    find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets PrintSupport)
    
    file(GLOB_RECURSE AURAUI_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/include/AuraComponentUI.h)
    file(GLOB_RECURSE UI_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/include/UI/*.h ${CMAKE_CURRENT_SOURCE_DIR}/include/UI/*.hpp)
    file(GLOB_RECURSE TOOL_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/include/TOOL/*.h ${CMAKE_CURRENT_SOURCE_DIR}/include/TOOL/*.hpp)
    file(GLOB_RECURSE UI_SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)
    
    set(SRC_FILES
        ${AURAUI_HEADERS}
        ${UI_HEADERS}
        ${TOOL_HEADERS}
        ${UI_SOURCES}
    )
    
    # 动态库
    add_library(AuraComponentUI SHARED
        ${SRC_FILES}
    )
    set_target_properties(AuraComponentUI PROPERTIES OUTPUT_NAME "AuraComponentUI")
    set_target_properties(AuraComponentUI PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${MODULE_OUTPUT_PATH}/plugin/lib)
    
    # 静态库
    add_library(AuraComponentUIStatic STATIC
        ${SRC_FILES}
    )
    set_target_properties(AuraComponentUIStatic PROPERTIES OUTPUT_NAME "AuraComponentUI")
    set_target_properties(AuraComponentUIStatic PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${MODULE_OUTPUT_PATH}/plugin/arc PREFIX "")
    
    # 链接库
    target_link_libraries(AuraComponentUI PRIVATE
        Qt${QT_VERSION_MAJOR}::Widgets
        Qt${QT_VERSION_MAJOR}::PrintSupport
    )
    
    target_link_libraries(AuraComponentUIStatic PRIVATE
        Qt${QT_VERSION_MAJOR}::Widgets
        Qt${QT_VERSION_MAJOR}::PrintSupport
    )
    
    # 使用相对路径
    target_include_directories(AuraComponentUI PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    )
    
    target_include_directories(AuraComponentUIStatic PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    )
    
    # 安装目标
    install(TARGETS
        AuraComponentUI
        AuraComponentUIStatic
            EXPORT AuraComponentUITargets
            LIBRARY DESTINATION "${MODULE_OUTPUT_PATH}/plugin/lib"
            ARCHIVE DESTINATION "${MODULE_OUTPUT_PATH}/plugin/arc"
            INCLUDES DESTINATION "include")
    
    install(EXPORT AuraComponentUITargets
            FILE AuraComponentUITargets.cmake
            DESTINATION "${MODULE_OUTPUT_PATH}/plugin")
    
    install(FILES "${CMAKE_CURRENT_SOURCE_DIR}/AuraComponentUIConfig.cmake"
            DESTINATION "${MODULE_OUTPUT_PATH}/plugin")
    
    ```

* 备注

    使用install进行安装时，若出现无法定位正确的安装目标地址，即生成的dll或a文件无法复制时，建议拆分，一次install一个即可。

### configure_file

实现将input文件的内容复制到output中，替换 @ VAR @ 或${ VAR }变量。

```cpp
configure_file(<input> <output>
               [COPYONLY] [ESCAPE_QUOTES] [@ONLY]
               [NEWLINE_STYLE [UNIX|DOS|WIN32|LF|CRLF] ])
/*
    COPYONLY :仅进行拷贝，不做变量替换。
    ESCAPE_QUOTES :使用反斜杠(C语言风格)进行转义。
    @ONLY :仅对@@的变量进行替换。
    NEWLINE_STYLE :指定文件输出的新行的格式(Linux、Unix:\n Windows:\r\n)。（与COPYONLY冲突，不可同时使用）
    
    input文件通常以XXX.h.in命名，output文件输出为.h
    
*/
```

适用于自动生成Version、时间戳、项目路径配置等。