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

## 复制和视图

### 简单赋值

不会创建数组对象或者其数据的拷贝

### 视图或者浅复制

view 方法创建一个新的数组对象，它和原对象共享相同的数据

```python
c = a.view()
assert c is not a
assert c.base is a
assert !c.flags.owndata
```

## 深拷贝

copy 方法生成数组和其数据的完整拷贝

```python
d = a.copy()
assert d is not a
assert d.base is not a
```

## Less 基础

### 广播 Broadcasting 规则

Broadcasting 它允许通用函数以有意义方式处理具有不完全相同形状的输入。

- 如果输入数组具有不同数量的维度，则 1 将被重复地添加到较小数组的 shape，直到所有数组具有相同数量的维度。
- 确保沿着特定维度具有大小为1的数组表现得好像它们具有沿着该维度具有最大形状的数组的大小。

### 花式索引和索引技巧

#### 使用索引数组索引

当被索引的数组 a 是一个多维数组的时候，单个所用数组指的是它的第一维度。

也可以给出多个维度的索引，但是每个维度索引数组必须有相同的形状。

#### 使用布尔值作为数组索引

1. 通过给予布尔值确定数组中哪些是想要的
2. 对于每个维度，给出想要的布尔数组

#### ix_() 函数

该函数用来组合不同的向量来获得每个 n-uplet 结果；直观的结果是，可以将每个它们安排到不同的位置上去，然后直接运用矩阵方法。

## 线性代数

- transpose() 转置
- np.linalg.inv(a) 求逆
- np.trace() 求迹
- np.linalg.solve(a,y) 求解
- np.linalg.eig(j) 求

## 技巧和提示

### 自动重定义数组形状

要更改数组的大小，可以省略其中的一个 size 因为它可以被推导出来。

## 数据类型

| 数据类型 | 描述 |
|-----|----|
| bool_ | 布尔（True或False），存储为一个字节 |
| int_ | 默认整数类型（与Clong相同；通常是int64或int32） |
| INTC | 与Cint（通常为int32或int64）相同 |
| INTP | 用于索引的整数（与Cssize_t相同；通常是int32或int64） |
| INT8 | 字节（-128至127） |
| INT16 | 整数（-32768至32767） |
| INT32 | 整数（-2147483648至2147483647） |
| INT64 | 的	整数（-9223372036854775808至9223372036854775807） |
| UINT8 | 无符号整数（0到255） |
| UINT16 | 无符号整数（0到65535） |
| UINT32 | 无符号整数（0到4294967295） |
| UINT64 | 无符号整数（0到18446744073709551615） |
| float_ | float64的简写。 |
| float16 | 半精度浮点：符号位，5位指数，10位尾数 |
| FLOAT32 | 单精度浮点数：符号位，8位指数，23位尾数 |
| float64 | 双精度浮点：符号位，11位指数，52位尾数 |
| complex_ | complex128的简写。 |
| complex64 | 复数，由两个32位浮点数（实部和虚部） |
| complex128 | 复数，由两个64位浮点数（实部和虚部） |

使用字符串方式来访问 dtype 的方式已经被淘汰

## 数组创建

### 将 python array_like 对象转换为 numpy 数组

array() 函数可以将 array-like 结构转为数组。

一些创建方法在前面已经提到过，此处省略。

- indices 将低维度的数组拓展为更高维度的数组

### 从磁盘读取数组

略

## NumPy IO 操作

### 使用 genfromtxt 数据

提供了两个玄幻，第一个循环以字符串序列转换文件的每一行，第二个循环将字符串转换为适当的数据类型，这种机制尽管比较慢，但是提供了更好的灵活性，而 loadtxt 等简单方法就不能做到这一点。

**约定如下**:

```python
import numpy as np
from io import BytesIO
```

#### 定义输入

genfromtxt的唯一强制参数是数据的来源。它可以是一个字符串，一串字符串或一个生成器。如果提供了单个字符串，则假定它是本地或远程文件的名称，或者带有read方法的开放文件类对象，例如文件或StringIO.StringIO对象。如果提供了字符串列表或生成器返回字符串，则每个字符串在文件中被视为一行。当传递远程文件的URL时，该文件将自动下载到当前目录并打开

#### 将行拆分为列

`delimiter` 参数

delimiter 参数用于表示字符标记列之间的分割，字符为直接表示标记，数字定义为给定数量的字符

`autostrip` 参数

使用 autostrip 参数可以帮助覆盖默认行为，以剥离前导和尾随的空格。

`comments` 参数

定义标记开始的和结束的字符串

#### 跳过直线并选择列

`skip_header` 和 `skip_footer` 参数

默认 =0 为不跳过

`usecols`

选取其中的某些行

#### 选择数据的某些行

控制我们从文件中读取的字符串序列如何转换为其他类型的主要方法是设置dtype参数。这个参数的可接受值是：

  -  单一类型，如dtype=float。除非使用names参数将名称与每个列关联（见下文），否则输出将是给定dtype的2D格式。请注意，dtype=float是genfromtxt的默认值。
  - 一系列类型，如dtype =（int， float， float）。
  - 逗号分隔的字符串，例如dtype="i4,f8,|S3"。
  - 一个包含两个键'names'和'formats'的字典。
  - a sequence of tuples(name, type), such as dtype=[('A', int), ('B', float)].
  - 现有的numpy.dtype对象。
  - 特殊值None。在这种情况下，列的类型将根据数据本身确定（见下文）。

在所有情况下，除了第一种情况，输出将是一个带有结构化dtype的一维数组。这个dtype与序列中的项目一样多。字段名称由names关键字定义。

当dtype=None时，每列的类型由其数据迭代确定。我们首先检查一个字符串是否可以转换为布尔值（也就是说，如果字符串在小写字母中匹配true或false）；然后是否可以将其转换为整数，然后转换为浮点数，然后转换为复数并最终转换为字符串。通过修改StringConverter类的默认映射器可以更改此行为。

为方便起见，提供了dtype=None选项。但是，它明显比显式设置dtype要慢。


#### 设置名称

`names` 参数

1. 在给予 dtype 时一起给予。
2. 或者将`names`关键字与一系列字符串或逗号分隔的字符串一起使用。

`defaultfmt` 参数

如果我们没有提供足够的名称来匹配dtype的长度，缺少的名称将使用此默认模板进行定义：

`deletechars`，`excludelist`，`case_senstitve` 参数帮助更好的控制名称

#### 调整转换

`converters` 定义字符串序列如何转换

`missing_values` 和 `filling_values` 用于处理缺失值。

`usemask` 用来跟踪丢失数据的发生。

## 索引

如前述，略

### 处理程序中可变数量的索引

在其它地方构造的时候，注意切片使用 `slice` 函数，省略号使用 `Ellipsis` 对象代替。

## 广播

如前述，略

## 字节交换

### 字节排序和 ndarrays 简介

数据类型 `>i2` 其中 `>` 表示大端，`i2` 表示带符号的 2 字节整数。

### 更改字节顺序

- 更改数组 dtype 中的字节顺序信息，以便它将未确定的数字的字节顺序和它正在查看的底层内存之间的关系，使用`arr.newbyteorder()`
- 改变底层数据的字节顺序，保持原来的 的type解释，使用 `arr.byteswap()`。

## 结构换数组

### 介绍，如前述，略

### 结构化数据类型

结构化数据类型可以被认为是一定长度的字节序列，它被解释为一个字段集合，每个字段在结构中有一个名称，一个数据类型和一个字节偏移量。

可以使：

1. 元组列表，每个字段一个元组；
2. 一串用逗号分隔的 的type 规范；
3. 字段参数数组的字典；
4. 字段名称的字典

#### 操作和显示结构化数据类型

可以在 dtype 对象的 `names` 属性中找到了结构化数据类型的字段名称列表；或者可以在`fields`属性找到包含每个字段的dtype和字段偏移量的元组。

#### 自动字节偏移和对齐

默认 `align = True`，字段将被打包在一起；否则将以 C 方式填充，以使得 itemsize 是最大字段对齐的倍数（最小公倍数？）

如果在基于字典的 dtype 规范中使用可选的 offset 键制定了偏移量，则将检测偏移量是否为最大字段大小的倍数，否则将引发异常。

便捷函数numpy.lib.recfunctions.repack_fields将对齐的dtype或数组转换为已打包的dtype或数组，反之亦然。它接受dtype或结构化ndarray作为参数，并返回一个带有重新打包的字段的副本，无论是否有填充字节。

#### 字段标题

字段标题用作附加说明或者备用名称，可以用于索引数组，当使用 list-of-tuple 时，可以将字段名称指定为两个字符串的元组，当使用第一种形式基于字典的规范时，通过使用 3 元素元组来提供标题 `（数据类型，偏移量，标题）`；

#### 联合类型

使用 `dtype` 规范 `（base_dtype，dtype）` 即可，默认情况下，base_dtype 为 `numpy.void`

### 结构化数组的索引和分配

#### 将数据分配给结构化数组

1. 使用python元组
2. 通过标量赋值
3. 来自其他结构化数组的赋值
4. 子阵列的赋值

#### 索引结构化数组

1. 通过字段名称索引数组来访问和修改结构化数组的各个字段
2. 访问多个字段，此时索引的结果是原始数组的视图（1.15+），若想使用副本，需要使用 `numpy.lib.recFunctions.repack_field`。

#### 查看包含对象的结构化数组

为了防止在`numpy.Object`类型的字段中阻塞对象指针，numpy目前不允许包含对象的结构化数组的视图。

结构比较：先按 dtype 比较，再按照内容比较，返回原始维度的布尔数组；
当两个dtype不同的数组比较时，返回类型比较，不推荐这种方式。

### 记录数组

作为一个可选的方便的选项，numpy提供了一个ndarray子类 numpy.recarray，以及 numpy.rec子模块中的相关帮助函数，它允许通过属性访问结构化数组的字段，而不仅仅是通过索引。记录数组还使用一种特殊的数据类型 numpy.Record，它允许通过属性对从数组获得的结构化标量进行字段访问。

类似 js 中的 a['bar'] 和 a.bar

## 子类化 ndarray

### ndarrays 和对象创建

ndarrays和对象创建

ndarray的子类化很复杂，因为ndarray类的新实例可以以三种不同的方式出现。 这些是：

1. 显式构造函数调用 - 如MySubClass（params）。 这是创建Python实例的常用途径。
2. 查看转换 - 将现有的ndarray转换为给定的子类。
3. 从模板创建新实例-从模板实例创建新实例。示例包括从子类数组返回片、从uFuncs创建返回类型和复制数组。有关更多详细信息，请参见从模板创建。

最后两个是ndarrays的特征 - 为了支持数组切片之类的东西。 子类化ndarray的复杂性是由于numpy必须支持后两种实例创建路径的机制。

### 视图投影

视图投影是标准的ndarray机制，通过它您可以获取任何子类的ndarray，并将该数组的视图作为另一个（指定的）子类返回

```python
>>> import numpy as np
>>> # create a completely useless ndarray subclass
>>> class C(np.ndarray): pass
>>> # create a standard ndarray
>>> arr = np.zeros((3,))
>>> # take a view of it, as our useless subclass
>>> c_arr = arr.view(C)
>>> type(c_arr)
<class 'C'>
```

### 从模板创建

当numpy发现它需要从模板实例创建新实例时，ndarray子类的新实例也可以通过与视图转换非常相似的机制来实现。

### 视图投影和从模版创建的关系

View转换意味着您已从ndarray的任何潜在子类创建了数组类型的新实例。从模板创建新意味着您已从预先存在的实例创建了类的新实例。

### 子类化的含义

调用子类的 `__new__` 方法来返回 subtype，跳过 `__init__`,并使用 `__array_finalize__` 进行析构

```python
import numpy as np

class RealisticInfoArray(np.ndarray):

    def __new__(cls, input_array, info=None):
        # Input array is an already formed ndarray instance
        # We first cast to be our class type
        obj = np.asarray(input_array).view(cls)
        # add the new attribute to the created instance
        obj.info = info
        # Finally, we must return the newly created object:
        return obj

    def __array_finalize__(self, obj):
        # see InfoArray.__array_finalize__ for comments
        if obj is None: return
        self.info = getattr(obj, 'info', None)
```
### ufuncs 的 `__array_ufunc__`

子类可以通过重写默认的ndarray.__arrayufunc_方法来重写在其上执行numpy uFunc函数时的行为。此方法将代替ufunc执行，如果未实现所请求的操作，则应返回操作结果或NotImplemented`。

```python
 def __array_ufunc__(ufunc, method, *inputs, **kwargs):
```

- *ufunc* 是被调用的ufunc对象。
- *method* 是一个字符串，指示如何调用Ufunc。“__call__” 表示它是直接调用的，或者是下面的其中一个：`methods <ufuncs.methods>`：“reduce”，“accumulate”，“reduceat”，“outer” 或 “at” 等属性。
- *inputs* 是 “ufunc” 的类型为元组的输入参数。
- *kwargs* A包含传递给函数的任何可选或关键字参数。 这包括任何``out``参数，并且它们总是包含在元组中。

### 额外的内容是高级属性，具体参阅文档