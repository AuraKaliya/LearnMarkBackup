## Qt 模型视图(Model-View)

视图是显示和编辑数据的界面组件，模型是视图与原始数据的操作接口。

Qt中所有的数据模型(Model)都是基于`QAbstractItemModel`类。



## Model

### QAbstractItemModel

模型类的基类，定义了视图组件和代理存取数据的接口。数据无需存储在数据模型里，可以存放在其它类、文件或数据库中。

基于该类派生出几个抽象类`QAbstractListModel`、`QAbstractProxyModel`、`QAbstractTableModel`、`QStandardItemModel`、`QFileSystemModel`;

| Model 类                 | 用途                                                       |
| ------------------------ | ---------------------------------------------------------- |
| QStringListModel         | 用于处理字符串列表数据的数据模型类                         |
| QStandardltemModel       | 标准的基于项数据的数据模型类，每个项数据可以是任何数据类型 |
| QFileSystemModel         | 计算机上文件系统的数据模型类                               |
| QSortFilterProxyModel    | 与其他数据模型结合，提供排序和过滤功能的数据模型类         |
| QSqlQueryModel           | 用于数据库SQL查询结果的数据模型类                          |
| QSqlTableModel           | 用于数据库的一个数据表的数据模型类                         |
| QSqlRelationalTableModel | 用于关系型数据表的数据模型类                               |



所有的数据模型都由该类继承而来，无论底层的数据结构如何表示，其索引都按该类提供的表格层次结构来表示数据。

数据模型中存储数据的基本单元都是项(Item)，每个项都有一个行号、列号和父项。通过数据模型存储的每个数据项都由一个模型索引，视图和代理组件通过模型索引来获取数据。

`QModelIndex`是表示模型索引的类，提供数据存储的一个临时指针，该指针随模型内部组织的数据结构改变而改变。若需要使用持久型模型索引，则使用`QPersistentModelIndex`类。

在为一个数据模型的项设置数据时，可以赋予其不同项的角色的数据。`QStandardltemModel`项数据为`QStandardItem`；

```cpp
void QStandardItem::setData(const QVariant &value, int role= Qt::UserRole + 1);
QVariant QStandardItem::data(int role = Qt::UserRole + 1) const;
```



## View

视图组件是显示数据模型的数据的界面，Qt提供的视图组件有`QListView`、`QTreeView`、`QTableView`、`QColumnView`、`QHeaderView`；

视图组件显示数据是，需要调用视图类的`setModel()`函数，关联视图组件和数据模型。



## Delegate

代理是在视图组件上为编辑数据提供编辑器。负责从数据模型处获取相应数据，然后显示在编辑器中；在编辑数据后，又将其保存在数据模型中。

### QAbstractItemDelegate

所有代理类的基类。其子类`QStyledItemDelegate`是Qt视图组件的缺省使用的代理类。

