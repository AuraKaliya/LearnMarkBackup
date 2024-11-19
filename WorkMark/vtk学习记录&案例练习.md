



## 基本概念

vtk是基于opengl的三维图像处理可视化库，具有设备无关性。

vtk的基础模型有两类：图形模型和可视化模型。

基本开发流程：数据准备 ==> 渲染，图元 ==> 图形。







## 库文件学习





## 一些重要的类

### vtkNew

模板类，功能和vtkSmartPointer一样，都是智能指针，在局部变量中使用更优，全局还是使用vtkSmartPointer更好。

### vtkSmartPointer



### vtkGenericOpenGLRenderWindow

继承关系：vtkGenericOpenGLRenderWindow ==> vtkOpenGLRenderWindow  (抽象类) ==>vtkRenderWindow  ==> vtkWindow

相关文档：[VTK: vtkGenericOpenGLRenderWindow Class Reference](https://vtk.org/doc/nightly/html/classvtkGenericOpenGLRenderWindow.html#a6cfabd09a744addb6ed7a2656b600363)

实现平台无关的渲染窗口。具有默认交互样式。实际使用中，可通过创建对象配置自己的OpenGL上下文，用于渲染场景，显示三维模型、体积数据等。

常用方法：

```c++
/*
MakeCurrent()						将 OpenGL 上下文设置为当前上下文。
IsCurrent()							检查当前上下文是否为该窗口的上下文。
SupportsOpenGL()					检查是否支持 OpenGL。
SetFrontLeftBuffer(unsigned int)	设置绘制缓冲区。
SetOwnContext(vtkTypeBool)			设置是否拥有自己的 OpenGL 上下文。
*/
```

使用步骤

```c++
vtkNew<vtkGenericOpenGLRenderWindow> renderWindow;
qvtkWidget->setRenderWindow(renderWindow);

// qvtkWidget  class:QVTKOpenGLNativeWidget      --创建设置renderWindow-设置renderWindow
```

### vtkContextView

继承关系：vtkContextView ==> vtkRenderViewBase ==> vtkView ==> vtkObject

相关文档：[VTK: vtkRenderViewBase Class Reference](https://vtk.org/doc/nightly/html/classvtkRenderViewBase.html)

用于创建和显示基于2D场景的渲染窗口。具有默认交互样式和渲染器。默认背景是白色。

常用方法：

```c++
/*
SetContext(vtkContext2D*)    	设置vTkContext2D对象，用于渲染2D场景
GetContext() 					获取视图关联的vtkContext2D对象
SetScene(vtkContextScene*)		设置场景对象，将其与视图关联
GetScene()						获取视图的场景对象
SetRenderWindow()				--vtkRenderViewBase的  设置渲染窗体
*/
```

使用步骤：

```c++
vtkNew<vtkContextView> view;
view->SetRenderWindow(renderWindow);
view->GetRenderer()->SetBackground(backgroundColor.GetData());
view->GetRenderWindow()->SetSize(640, 480);
view->GetScene()->AddItem(chart);
/*
	vtkNew<vtkChartXY> chart;
    vtkNew<vtkNamedColors> colors;
    vtkColor3d backgroundColor = colors->GetColor3d("Seashell");
*/
```

### vtkTable

继承关系：vtkTable ==> vtkDataObject ==> vtkObject ==>vtkObjectBase

相关文档：[VTK: vtkTable Class Reference](https://vtk.org/doc/nightly/html/classvtkTable.html)

一个基本的数据结构，用于存储相似类型的数据列。能确保每一列具有相同数量的条目，并提供行访问和单条目访问的功能。

常用方法：

```cpp
/*
GetNumberOfRows()								获取表中的行数。
SetNumberOfRows(vtkIdType)						设置表中的行数。
GetRow(vtkIdType row)							获取表中的一行作为 vtkVariantArray，其中每一列对应一个条目。
SetRow(vtkIdType row, vtkVariantArray *values)	使用一个 vtkVariantArray 设置表中的一行。
InsertRow(vtkIdType row)						在指定索引处插入一行。
RemoveRow(vtkIdType row)						从表中删除一行。
GetNumberOfColumns()							获取表中的列数。
GetColumnName(vtkIdType col)					获取指定列的名称。
*/
```

使用步骤：

```cpp
vtkNew<vtkTable> table;

vtkNew<vtkIntArray> arrMonth;
arrMonth->SetName("Month");
table->AddColumn(arrMonth);

vtkNew<vtkIntArray> arr2008;
arr2008->SetName("2008");
table->AddColumn(arr2008);

vtkNew<vtkIntArray> arr2009;
arr2009->SetName("2009");
table->AddColumn(arr2009);

vtkNew<vtkIntArray> arr2010;
arr2010->SetName("2010");
table->AddColumn(arr2010);

table->SetNumberOfRows(12);
for (int i = 0; i < 12; i++)
{
table->SetValue(i, 0, i + 1);
table->SetValue(i, 1, data_2008[i]);
table->SetValue(i, 2, data_2009[i]);
table->SetValue(i, 3, data_2010[i]);
}

 vtkPlot* line = 0;

line = chart->AddPlot(vtkChart::BAR);
line->SetInputData(table, 0, 1);
auto rgba = colors->GetColor4ub("YellowGreen");
line->SetColor(rgba[0], rgba[1], rgba[2], rgba[3]);

line = chart->AddPlot(vtkChart::BAR);
line->SetInputData(table, 0, 2);
rgba = colors->GetColor4ub("Salmon");
line->SetColor(rgba[0], rgba[1], rgba[2], rgba[3]);

line = chart->AddPlot(vtkChart::BAR);
line->SetInputData(table, 0, 3);
rgba = colors->GetColor4ub("CornflowerBlue");
line->SetColor(rgba[0], rgba[1], rgba[2], rgba[3]);


// chart: item, vtkNew<vtkChartXY> chart;

```



### **vtkEventQtSlotConnect**

继承关系：vtkEventQtSlotConnect ==> vtkObject ==>vtkObjectBase

相关文档：[VTK: vtkEventQtSlotConnect Class Reference](https://vtk.org/doc/nightly/html/classvtkEventQtSlotConnect.html)

用于管理VTK事件和QT槽的连接的类，允许VTK对象的事件与Qt对象的槽相连接，并提供了connect和disconnect方法进行连接的建立和断开。

常用方法：

```cpp
/*
Connect                         连接一个 VTK 对象的事件到一个 Qt 对象的槽。
Disconnect                      断开一个 VTK 对象的事件与一个 Qt 对象的槽的连接。
GetNumberOfConnections          查询已经设置的连接数量。
*/
```

使用步骤：

```cpp
     vtkNew<vtkEventQtSlotConnect> slotConnector;
     Connections = slotConnector;
Connections->Connect(
         m_vtkWidget->renderWindow()->GetInteractor(),
         vtkCommand::LeftButtonPressEvent, this,
         SLOT(slot_clicked(vtkObject*, unsigned long, void*, void*)));
//vtkSmartPointer<vtkEventQtSlotConnect> Connections;
```



### vtkLookupTable

继承关系：vtkLookupTable ==> vtkScalarsToColors ==> vtkObject ==>vtkObjectBase

相关文档：[VTK: vtkLookupTable Class Reference](https://vtk.org/doc/nightly/html/classvtkLookupTable.html)

用于将标量值映射到颜色。通常与vtk中的映射器（mapper）一起使用，以便在渲染时将数据值转换为颜色，从而在可视化中更好地区分数据的不同部分。

使用步骤：

```cpp
//封装成模块/函数
//使用setTableValue或Build生成颜色表
vtkNew<vtkLookupTable> GetPlatonicLUT()
{
  vtkNew<vtkLookupTable> lut;
  lut->SetNumberOfTableValues(20);

  lut->SetTableRange(0.0, 19.0);
  lut->Build();

  lut->SetTableValue(0, 0.1, 0.1, 0.1);
  lut->SetTableValue(1, 0, 0, 1);
  lut->SetTableValue(2, 0, 1, 0);
  lut->SetTableValue(3, 0, 1, 1);
  lut->SetTableValue(4, 1, 0, 0);
  lut->SetTableValue(5, 1, 0, 1);
  lut->SetTableValue(6, 1, 1, 0);
  lut->SetTableValue(7, 0.9, 0.7, 0.9);
  lut->SetTableValue(8, 0.5, 0.5, 0.5);
  lut->SetTableValue(9, 0.0, 0.0, 0.7);
  lut->SetTableValue(10, 0.5, 0.7, 0.5);
  lut->SetTableValue(11, 0, 0.7, 0.7);
  lut->SetTableValue(12, 0.7, 0, 0);
  lut->SetTableValue(13, 0.7, 0, 0.7);
  lut->SetTableValue(14, 0.7, 0.7, 0);
  lut->SetTableValue(15, 0, 0, 0.4);
  lut->SetTableValue(16, 0, 0.4, 0);
  lut->SetTableValue(17, 0, 0.4, 0.4);
  lut->SetTableValue(18, 0.4, 0, 0);
  lut->SetTableValue(19, 0.4, 0, 0.4);
  return lut;
}

//使用
 auto lut = GetPlatonicLUT();

 vtkNew<vtkPlatonicSolidSource> source;
 source->SetSolidTypeToOctahedron();

 vtkNew<vtkPolyDataMapper> mapper;
 mapper->SetInputConnection(source->GetOutputPort());
 mapper->SetLookupTable(lut);
 mapper->SetScalarRange(0, 19);

 vtkNew<vtkActor> actor;
 actor->SetMapper(mapper);
```

```cpp
/*
	1.定义颜色映射表
	2.配置颜色
	3.数据映射
*/
 //定义颜色映射表
    vtkSmartPointer<vtkLookupTable> pColorTable = vtkSmartPointer<vtkLookupTable>::New();
//设置颜色表中的颜色
    pColorTable->SetNumberOfColors(256);
    pColorTable->SetHueRange(0.67, 0.0);        //色调范围从红色到蓝色
    pColorTable->Build();
//数据映射
    vtkSmartPointer<vtkPolyDataMapper> cubeMapper = vtkSmartPointer<vtkPolyDataMapper>::New();
    cubeMapper->SetInputData(cube);
    cubeMapper->SetScalarRange(0, 7);
    cubeMapper->SetLookupTable(pColorTable);
    vtkSmartPointer<vtkActor> cubeActor = vtkSmartPointer<vtkActor>::New();
    cubeActor->SetMapper(cubeMapper);
```



## 基础练习

1. 两个空间点的距离

    ```C++
    //test1   --<vtkMath.h>
    #include <vtkMath.h>
    
    double p0[3]={0.0,0.0,0.0};
    double p1[3]={1.0,1.0,1.0};
    double squardDistance=vtkMath::Distance2BetweenPoints(p0,p1);
    double distance=std::sqrt(squardDistance);
    ```

2. 点到线的距离

    ```c++
    //test2    --<vtkLine.h>  <vtkNew.h>
    #include <vtkLine.h>
    #include <vtkNew.h>
    
    double lineP0[3]={0.0,0.0,0.0};
    double lineP1[3]={2.0,0.0,0.0};
    
    double p0[3]={1.0,0.0,0.0};
    double p1[3]={1.0,2.0,0.0};
    double dist0=sqrt(vtkLine::DistanceToLine(p0,lineP0,lineP1));
    double dist1=sqrt(vtkLine::DistanceToLine(p1,lineP0,lineP1));
    //line 是向量
    //点到线距离最近的点   closest是输出参数   t是点到线上的投影比例位置（P0起始到P1结束）
    {
        //
        double t;
        double closest[3];
        double dist0=vtkLine::DistanceToLine(p0,lineP0,lineP1,t,closest);
        cout<<"Dist0: "<<dist0<< " closest point :"<<closest[0]<<" "<<closest[1]<<" "<<closest[2]<<endl;
    
        double dist1=vtkLine::DistanceToLine(p1,lineP0,lineP1,t,closest);
        cout<<"Dist1: "<<dist0<< " closest point :"<<closest[0]<<" "<<closest[1]<<" "<<closest[2]<<endl;
        cout<<" t "<<t<<endl;
    }
    ```

3. 非法除0检测

    ```c++
    //test3    --<vtkFloatingPointExceptions.h>
    #include <vtkFloatingPointExceptions.h>
    #ifdef _MSC_VER
    #pragma warning(disable 4723)
    #endif
    //pragma warning disable   忽略指定警告
    {
        vtkFloatingPointExceptions::Enable();
        double x=0.0;
        double y=1.0/x ;
    }
    //y=inf
    ```

4. 生成均值为0、标准差为2.0的高斯分布函数

    ```c++
    //test4    --<vtkBoxMuellerRandomSequence.h>  <vtkNew.h>
    #include <vtkBoxMuellerRandomSequence.h>
    unsigned  int numRand=3;
    vtkNew<vtkBoxMuellerRandomSequence> randomSequence;
    
    auto mean=0.0;
    auto standardDeviation=2.0;
    
    for(unsigned int i=0;i<numRand;++i)
    {
        auto a=randomSequence->GetScaledValue(mean,standardDeviation);
        randomSequence->Next();
    }
    
    ```

5. 进行标准投影变换和透视变换

    ```c++
    //test5    --<vtkMatrix4x4.h>   <vtkNew.h>    <vtkPerspectiveTransform.h>    <vtkTransform.h>
    #include <vtkMatrix4x4.h>
    #include <vtkPerspectiveTransform.h>
    #include <vtkTransform.h>
    
    vtkNew<vtkMatrix4x4> m;
    m->SetElement(0, 0, 1);
    m->SetElement(0, 1, 2);
    m->SetElement(0, 2, 3);
    m->SetElement(0, 3, 4);
    m->SetElement(1, 0, 2);
    m->SetElement(1, 1, 2);
    m->SetElement(1, 2, 3);
    m->SetElement(1, 3, 4);
    m->SetElement(2, 0, 3);
    m->SetElement(2, 1, 2);
    m->SetElement(2, 2, 3);
    m->SetElement(2, 3, 4);
    m->SetElement(3, 0, 4);
    m->SetElement(3, 1, 2);
    m->SetElement(3, 2, 3);
    m->SetElement(3, 3, 4);
    
    vtkNew<vtkPerspectiveTransform> perspectiveTransform;
    perspectiveTransform->SetMatrix(m);
    
    vtkNew<vtkTransform> transform;
    transform->SetMatrix(m);
    
    double p[3]={1.0,2.0,3.0};
    
    double normalProjection[3];
    transform->TransformPoint(p,normalProjection);
    
    double perspectiveProjection[3];
    perspectiveTransform->TransformPoint(p,perspectiveProjection);
    
    /*vtkPerspectiveTransform
    
    
    */
    /*vtkTransform
    
    */
    ```

    

6. 点投影到平面上  起点+法线==>平面投影点

    ```c++
    //test6  <vtkNew.h>     <vtkPlane.h>
    #include <vtkPlane.h>
    
    vtkNew<vtkPlane> plane;
    plane->SetOrigin(0.0,0.0,0.0);
    plane->SetNormal(0.0,0.0,1.0);
    
    double p[3]={23.1,54.6,9.2};
    double origin[3]={0.0,0.0,0.0};
    double normal[3]={0.0,0.0,1.0};
    double projected[3];
    
    plane->ProjectPoint(p,origin,normal,projected);
    
    /*
    vtkPlane
    
    */
    ```

    

7. 产生随机序列，seed播种

    ```c++
    //test7  <vtkMinimalStandardRandomSequence.h>    <vtkNew.h>
    #include <vtkMinimalStandardRandomSequence.h>
    
    vtkNew<vtkMinimalStandardRandomSequence> sequence;
    
     sequence->SetSeed(1);
    
     double x=sequence->GetRangeValue(-1.0,1.0);
     sequence->Next();
    
     double y=sequence->GetValue();
     sequence->Next();
    
     double z=sequence->GetValue();
    
    /*
    vtkMinimalStandardRandomSequence
    */
    ```

    

8. 产生0-2内的3个均匀分布的点

    ```C++
    //test8 <vtkMinimalStandardRandomSequence.h>    <vtkNew.h>    <time.h>
    #include <time.h>
    
    unsigned int numRand=3;
    
    vtkNew<vtkMinimalStandardRandomSequence> randomSequence;
    randomSequence->SetSeed(time(NULL));
    
    for(unsigned int i=0;i<numRand;++i)
    {
        auto a=randomSequence->GetRangeValue(0,2.0);
        randomSequence->Next();
        cout<<"a:"<<a<<endl;
    }
    
    ```

    

## Qt栏练习

如从ui进行编辑设计，则需要先将QOpenGL类升级为QVTKOpenGLNativeWidget。

1. 柱状图表

    ①数据准备；

    ②构建vtk窗体QVTKOpenGLNativeWidget、构建渲染窗体vtkGenericOpenGLRenderWindow，预设置颜色vtkNamedColors+vtkColor3d；

    ③构建图元-图表vtkChartXY，对图表参数进行设置vtkAxis、Title等。

    ④构建2D场景View：vtkContextView，将Item添加到View的Scene中。

    ⑤数据结构转换，将数据存放到table中vtkTable。

    ⑥给chart添加元素柱状图vtkPlot，数据从table中获取。

    ⑦其他设置。

    ⑧重新给vtk窗体设置渲染窗体。

    ```cpp
    //.h文件
    #ifndef WIDGET_H
    #define WIDGET_H
    
    #include <QWidget>
    #include <QVTKOpenGLNativeWidget.h>
    
    QT_BEGIN_NAMESPACE
    namespace Ui { class Widget; }
    QT_END_NAMESPACE
    
    class Widget : public QWidget
    {
        Q_OBJECT
    
    public:
        Widget(QWidget *parent = nullptr);
        ~Widget();
    
    private:
        Ui::Widget *ui;
        QVTKOpenGLNativeWidget* m_vtkWindow;
    };
    #endif // WIDGET_H
    
    
    
    
    //.cpp文件
    #include "widget.h"
    #include "./ui_widget.h"
    
    #include <vtkAxis.h>
    #include <vtkChartXY.h>
    #include <vtkContextView.h>
    #include <vtkGenericOpenGLRenderWindow.h>
    #include <vtkIntArray.h>
    #include <vtkNamedColors.h>
    #include <vtkNew.h>
    #include <vtkPlot.h>
    #include <vtkRenderWindow.h>
    #include <vtkRenderWindowInteractor.h>
    #include <vtkRenderer.h>
    #include <vtkTable.h>
    #include <vtkTextProperty.h>
    #include <vtkVersion.h>
    
    #include <array>
    #include <iostream>
    #include <string>
    
    #if VTK_VERSION_NUMBER >= 89000000000ULL
    #define VTK890 1
    #endif
    
    #if VTK_VERSION_NUMBER >= 90000000000ULL
    #define VTK900 1
    #endif
    
    namespace {
    // Monthly circulation data.
    int data_2008[] = {10822, 10941, 9979,  10370, 9460, 11228,
                       15093, 12231, 10160, 9816,  9384, 7892};
    int data_2009[] = {9058,  9474,  9979,  9408, 8900, 11569,
                       14688, 12231, 10294, 9585, 8957, 8590};
    int data_2010[] = {9058,  10941, 9979,  10270, 8900, 11228,
                       14688, 12231, 10160, 9585,  9384, 8590};
    } // namespace
    
    Widget::Widget(QWidget *parent)
        : QWidget(parent)
        , ui(new Ui::Widget)
    {
        ui->setupUi(this);
    
        //构建vtk窗体
        m_vtkWindow=new QVTKOpenGLNativeWidget(this);
        m_vtkWindow->setGeometry(0,0,this->width(),this->height());
    
        //构建渲染窗体
        vtkNew<vtkGenericOpenGLRenderWindow> renderWindow;
        m_vtkWindow->setRenderWindow(renderWindow);
    
        //预设置颜色
        vtkNew<vtkNamedColors> colors;
        vtkColor3d backgroundColor = colors->GetColor3d("Seashell");
        vtkColor3d axisColor = colors->GetColor3d("Black");
        vtkColor3d titleColor = colors->GetColor3d("MidnightBlue");
    
        //构建图元---图表
        vtkNew<vtkChartXY> chart;
    
        // 设置图表属性
        //----底部属性   --x轴
        vtkAxis* xAxis = chart->GetAxis(vtkAxis::BOTTOM);
        xAxis->SetTitle("Monthly");
        xAxis->GetTitleProperties()->SetColor(axisColor.GetData());
        xAxis->GetTitleProperties()->SetFontSize(16);
        xAxis->GetTitleProperties()->ItalicOn();
        xAxis->GetLabelProperties()->SetColor(axisColor.GetData());
        xAxis->SetGridVisible(true);
        //rgba
        xAxis->GetGridPen()->SetColor(colors->GetColor4ub("Black"));
    
        //左边--y轴
        vtkAxis* yAxis = chart->GetAxis(vtkAxis::LEFT);
        yAxis->SetTitle("Circulation");
        yAxis->GetTitleProperties()->SetColor(axisColor.GetData());
        yAxis->GetTitleProperties()->SetFontSize(16);
        yAxis->GetTitleProperties()->ItalicOn();
        yAxis->GetLabelProperties()->SetColor(axisColor.GetData());
        yAxis->SetGridVisible(true);
        yAxis->GetGridPen()->SetColor(colors->GetColor4ub("Black"));
    
        //设置图表标题
        chart->SetTitle("Circulation 2008, 2009, 2010");
        chart->GetTitleProperties()->SetFontSize(24);
        chart->GetTitleProperties()->SetColor(titleColor.GetData());
        chart->GetTitleProperties()->BoldOn();
    
        //  window中包括了多view，每个view配置一个scene，scene里存放Item
        //构建2D场景的View，设置显示窗体和添加Item
        vtkNew<vtkContextView> view;
        view->SetRenderWindow(renderWindow);
        view->GetRenderer()->SetBackground(backgroundColor.GetData());
        view->GetRenderWindow()->SetSize(640, 480);
        view->GetScene()->AddItem(chart);
    
        // 数据结构转换，将数据存放到table中
        vtkNew<vtkTable> table;
    
        vtkNew<vtkIntArray> arrMonth;
        arrMonth->SetName("Month");
        table->AddColumn(arrMonth);
    
        vtkNew<vtkIntArray> arr2008;
        arr2008->SetName("2008");
        table->AddColumn(arr2008);
    
        vtkNew<vtkIntArray> arr2009;
        arr2009->SetName("2009");
        table->AddColumn(arr2009);
    
        vtkNew<vtkIntArray> arr2010;
        arr2010->SetName("2010");
        table->AddColumn(arr2010);
    
        table->SetNumberOfRows(12);
        for (int i = 0; i < 12; i++)
        {
          table->SetValue(i, 0, i + 1);
          table->SetValue(i, 1, data_2008[i]);
          table->SetValue(i, 2, data_2009[i]);
          table->SetValue(i, 3, data_2010[i]);
        }
    
        //给表添加柱状图的元素  数据从table获取
         vtkPlot* line = 0;
    
        line = chart->AddPlot(vtkChart::BAR);
        line->SetInputData(table, 0, 1);
        auto rgba = colors->GetColor4ub("YellowGreen");
        line->SetColor(rgba[0], rgba[1], rgba[2], rgba[3]);
    
        line = chart->AddPlot(vtkChart::BAR);
        line->SetInputData(table, 0, 2);
        rgba = colors->GetColor4ub("Salmon");
        line->SetColor(rgba[0], rgba[1], rgba[2], rgba[3]);
    
        line = chart->AddPlot(vtkChart::BAR);
        line->SetInputData(table, 0, 3);
        rgba = colors->GetColor4ub("CornflowerBlue");
        line->SetColor(rgba[0], rgba[1], rgba[2], rgba[3]);
    
        //多重采样抗锯齿  对每个像素进行多次采样计算平均颜色，使渲染时边缘更加平滑。
        //较高的样本数产生更高的边缘 ，但增加计算成本。
        view->GetRenderWindow()->SetMultiSamples(0);
    
        view->SetInteractor(m_vtkWindow->interactor());
        m_vtkWindow->setRenderWindow(view->GetRenderWindow());
    }
    
    Widget::~Widget()
    {
        delete ui;
    }
    
    ```

    图示：

    ![image-20240313140758168](C:/Users/Aura/AppData/Roaming/Typora/typora-user-images/image-20240313140758168.png)

2. QImage和vtkImage的转换

    ①生成vtkImage（setDimensions AllocateScalars 指针赋值）

    ②初始化QImage，用QRgb从图像底层开始copy像素

    ```cpp
    
    namespace {
    vtkSmartPointer<vtkImageData> createDemoImageData()
    {
        vtkNew<vtkImageData> image;
        //设置图像50X50像素 1层
        image->SetDimensions(50,50,1);
        //为图像分配内存空间，每个像素设置3个分量（RGB）
        image->AllocateScalars(VTK_UNSIGNED_CHAR,3);
    
        //获取图像的高和宽
        int width=image->GetDimensions()[0];
        int height=image->GetDimensions()[1];
    
        //遍历像素
        for (int y = 0; y < height; y++)
         {
           for (int x = 0; x < width; x++)
           {
               //通过指针对图像的像素赋值，为绿图
             unsigned char* pixel =
                 static_cast<unsigned char*>(image->GetScalarPointer(x, y, 0));
             pixel[0] = x*2;
             pixel[1] = y*3;
             pixel[2] = x*2;
           }
         }
    
        return image;
    }
    QImage vtkImageDataToQImage(vtkSmartPointer<vtkImageData>imageData)
    {
        if (!imageData)
          {
            return QImage();
          }
    
          /// \todo retrieve just the UpdateExtent
          int width = imageData->GetDimensions()[0];
          int height = imageData->GetDimensions()[1];
    
          //格式设置为Format_RGB32 每个像素由32位颜色值表示
          QImage image(width, height, QImage::Format_RGB32);
    
          //指向QImage数据的指针， 被初始化为图像底部行开始
          QRgb* rgbPtr = reinterpret_cast<QRgb*>(image.bits()) + width * (height - 1);
    
          unsigned char* colorsPtr =
              reinterpret_cast<unsigned char*>(imageData->GetScalarPointer());
    
          // Loop over the vtkImageData contents.
          for (int row = 0; row < height; row++)
          {
            for (int col = 0; col < width; col++)
            {
              // Swap the vtkImageData RGB values with an equivalent QColor
              *(rgbPtr++) = QColor(colorsPtr[0], colorsPtr[1], colorsPtr[2]).rgb();
              colorsPtr += imageData->GetNumberOfScalarComponents();
            }
    
            rgbPtr -= width * 2;
          }
    
          return image;
    }
    } // namespace
    ```

    

3. 





### CMAKE

```cmake
cmake_minimum_required(VERSION 3.5)

project(vtkDemo_BarChartQt VERSION 0.1 LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(VTK_DIR "E:/thirdLibrary/vtk9.3/lib/cmake/vtk-9.3")


find_package(QT NAMES Qt6 Qt5 COMPONENTS Widgets REQUIRED)
find_package(Qt${QT_VERSION_MAJOR} COMPONENTS Widgets REQUIRED)

find_package(VTK)

if(VTK_NOT)
    message(STATUS "VTK NOT FOUND")
endif()


set(PROJECT_SOURCES
        main.cpp
        widget.cpp
        widget.h
        widget.ui
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(vtkDemo_BarChartQt
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET vtkDemo_BarChartQt APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(vtkDemo_BarChartQt SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(vtkDemo_BarChartQt
            ${PROJECT_SOURCES}
        )
    endif()
endif()

target_link_libraries(vtkDemo_BarChartQt PRIVATE Qt${QT_VERSION_MAJOR}::Widgets ${VTK_LIBRARIES})

set_target_properties(vtkDemo_BarChartQt PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(vtkDemo_BarChartQt)
endif()

vtk_module_autoinit(
  TARGETS vtkDemo_BarChartQt
  MODULES ${VTK_LIBRARIES}
)

```

