# numpy 学习笔记

## 介绍

numpy 包的核心是 ndarray 对象

ndarray 对象封装了 python 原生的通数据类型的 n 维数组。并为了保证性能，将相当多的操作进行了 AOT。

它们有一席重要的区别：

- ndarray 具有固定的大小。
- ndarray 数组中具有相同的类型。
- ndarray 具有高效的操作。
- 依赖包可以使用原生数组进行操作，但需要先转换成 ndarray。

## 安装

略

## 基础知识

在 numpy，维度称为轴，轴的数目为 rank。

### struct

- ndim 数组轴的个数
- shape 数组的维度，标识每个维度中数组的大小，对于有 n 行和 m 列的矩阵，shape 将是 (n,m) 因此， shape 元组的长度就是 rank 或者维度的个数 ndim；
- size 数组元素的总数
- dtype 一个描述数组中元素类型的对象，可以使用标准的 python 类型创建或者指定的 的type：numpy.int32、numpy.int16、numpy.float64
- itemsize 数组中每隔元素的字节大小 == dtype.itemsize
- data 该缓冲区包含数组的实际元素，通常不使用这个属性

### 创建方法

a = np.array(data:array|tuple,dtype=typeof(data))

### 常用工厂方法

- zeros(shape:tuple) 全0
- ones(shape:tuple) 全1
- empty(shape:tuple) 不初始化
- arange(start,end,step)
- linspace(start.end,size)
- reshape(...shape:iterable) 尽管作为实例方法被调用，但是由于返回的是对一个新的对象的应用，所以应当视为接受一个旧结果的工厂方法，后略
- fromfunction(func,shape,dtype)

**注意**：若数组太大无法打印，numpy 将跳过数组中心部分；若要禁止此跳过行为，可以使用 set_printoptions 更改打印选项

### 基本操作

- *：elementwise product
- dot：矩阵乘法
- +=，*=：同阶操作

不同类型操作时，进行向上转型。
可以通过指定轴 axis 的方式，改变实例方法 sum、min、max、cumsum 等的行为。

### 通用函数

numpy 提供了常见的数学函数，在数组上进行元素级别的操作。

### 索引、切片和迭代

一维数组可以被索引、切片和迭代。

**多维**：数组的每个轴可以有一个索引，这些索引在元组中以逗号分隔给出
当提供比数组的轴数更少的索引时，缺失的索引被认为是一个完整切片。

**迭代** 多维数组是相对于第一个轴完成的，使用 flat 将数组展平，可以对每个元素进行操作

## 形状操作

- ravel 返回一个展平后的数组
- reshape 返回一个改变轴后的数组
- T 返回一个转置后的数组

注意：reshape 是工厂方法，resize 是实例方法；如果将 reshape 操作中维度指定为 -1，会自动计算其它维度。

### 不同数组的堆叠

- vstack 仅限于二维数组，竖直方向堆叠
- hstack 仅限于二维数组，水平方向堆叠
- column_stack 将 1D 数组作为列堆叠到 2D 数组中，同理于 row_stack；concatenate 给出一个表示串接应该发生的轴的可选参数。

### 拆分数组

- hsplit 水平切割
- vsplit 竖直切割
- array_split 指定轴切割