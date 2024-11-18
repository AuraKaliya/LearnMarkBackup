### Triplet

Eigen::Triple< Scalar >  三元组，用于记录一个非零元素的行、列、值。

当与Eigen::SpareMatrix交互时，将一组Triplet放在vector中，传入即可生成稀疏矩阵。

```
std::vector<Eigen::Triplet<double> > tripleVector;
Eigen::SparseMatrix<double> matrix;
while(flag)
{
	...
	tripleVector.push_back({rowIndex,colIndex,value});
	...
}
matrix.setFromTriplets(tripleVector.begin(), tripleVector.end());
```



### SparseMatrix

Eigen::SparseMatrix < _Scalar , _Option , _StorageIndex > 是Eigen中的稀疏矩阵主要存储类。_

* _Scalar :系数（值）类型，如int、double等。
* _Option ：控制位，默认为-1 Eigen::ColMajor 列主序，可设置为 1 Eigen::RowMajor 行主序。
* _StorageIndex ：索引的类型，默认为int。

主要包含以下四个数组(行主序CSR、列主序CSC)：

* values: 稀疏矩阵中所有非0元素的值。--Data
* InnerIndices: 非0元素的索引（列主序为行索引，行主序为列索引）。 --Indices
* OuterStarts: 存储了矩阵中对应行/列中非0元素在values中的索引。 --IndexPointer
* InnerNNZs: 矩阵中对应行/列的非0元素的个数（列主序为每列，行主序为每行）。

成员函数

```C++
SparseMatrix<double> SpMat;
SpMat.nonZeros(); // 非0元素的个数
SpMat.outerSize(); // 列主序存储的列数，行主序存储的行数
SpMat.innerSize(); // 列主序存储的行数，行主序存储的列数
SpMat.innerIndexPtr(); // 指针，指向第一个非0元素的行索引（列主序）
SpMat.outerIndexPtr(); // 指针，指向第一个每列最开始非0元素在value的索引（列主序）
```



### CSR格式

前置：COO格式：将矩阵中非0元素以坐标方式存储< row,col,value >。

CSR是对COO格式的改进格式，矩阵按行顺序存储（行主序）。

* Data ：元素值。
* Indices ：Data中元素对应的列序数。
* IndexPointer ：第i个元素是第i行的第一个非0元素在Data中的序列（位置），第i+1个元素是第i行的最后一个非0元素在Data中的序列（位置）。 

其他：CSC格式：按列存储方式（列主序）。

一个示例：

#### Data

```
Size 7,5
IndexPointers 0,2,3,3,3,6,6,7
Indices 0,2,2,2,3,4,3
Data 8,2,5,7,1,2,9
-------------------------------------------------
(8, 0, 2, 0, 0)
(0, 0, 5, 0, 0)
(0, 0, 0, 0, 0)
(0, 0, 0, 0, 0)
(0, 0, 7, 1, 2)
(0, 0, 0, 0, 0)
(0, 0, 0, 9, 0)
```

#### solution

```C++
 //1 构建全0矩阵
    for(int i=0;i<m_size.first;++i)
    {
        QVector<double> tmpVector;
        for(int j=0;j<m_size.second;++j)
        {
            tmpVector.append(0);
        }
        m_matrix.append(tmpVector);
    }

    //2. 解析数据 进行填充

    auto iterStart=m_indexPointers.begin();
    auto iterEnd=m_indexPointers.begin();
    int index=0;
    iterEnd++;

    while(iterEnd!=m_indexPointers.end())
    {

        //qDebug()<<"NOW: "<<*iterStart<<*iterEnd;
        for(int i=*iterStart;i<*iterEnd;++i)
        {
            m_matrix[index][m_indices[i] ] =m_data[i];
        }
        index++;
        iterStart++;
        iterEnd++;
    }

```

