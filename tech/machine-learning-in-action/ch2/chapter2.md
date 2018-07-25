# k-近邻算法 kNN

## K-近邻算法概述

> `K-近邻算法`：采用测量不同特征值之间的距离的方法进行分类。
>
> 优点：精度高，对异常值不敏感，无数据输入假定。
> 
> 缺点：计算复杂度高、空间复杂度高。
>
> 使用数据范围：数值型和标称型

### 工作原理

存在一个样本数据集合，称作训练样本集，并且样本中的每个数据都存在标签 —— 我们知道样本集中每一个数据与所属分类的对应关系。输入没有标签的新数据后，将新数据的每个特征与样本集中的数据对应的特征进行比较，然后提取样本集中特征最相似数据（最近邻）的分类标签。

一般来说，只选取样本数据集中前k个最相似的数据，这就是k-近邻算法中k的出处，通常k是不大于20的整数，选取k中出现次数最多的分类，作为新数据的分类。

### 使用 python 导入数据

```python
from numpy import *
import operator

def createDataSet():
    group = array([[1.0,1.1],[1.0,1.0],[0,0],[0,0.1]])
    labels = ['A','A','B','B']
    return group,labels
```

```python
>>>import kNN
>>>group
array([[ 1. ,  1.1],
       [ 1. ,  1. ],
       [ 0. ,  0. ],
       [ 0. ,  0.1]])
>>>labels
['A', 'A', 'B', 'B']
```

### 实施 kNN 算法

伪代码：

1. 计算已知类别数据中的点与当前点之间的距离。
2. 按照距离递增次序排序。
3. 选取与当前距离最小的k个点。
4. 确定前k个点所在类别的出现频率。
5. 返回前k个点出现频率最高的类别作为当前点的预测分类。

```python
def classify0(inX,dataSet,labels,k):
    dataSetSize = dataSet.shape[0]
    diffMat = tile(inX,(dataSetSize,1)) - dataSet
    sqDiffMat = diffMat**2
    sqDistances = sqDiffMat.sum(axis=1)
    distances = sqDistances**0.5
    sortedDistIndicies = distances.argsort()
    classCount = {}
    for i in range(k):
        voteIlabel = labels[sortedDistIndicies[i]]
        classCount[voteIlabel]=classCount.get(voteIlabel,0)+1
    sortedClassCount = sorted(classCount.items(),
     key=operator.itemgetter(1),reverse=True)
    return sortedClassCount[0][0]
```

### 如何测试分类器

使用没有答案的测试集对分类器进行测试。