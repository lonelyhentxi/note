# GAMES101 - 现代图形学入门笔记4：变换进阶

该课程是 GAMES 开设的现代计算机图形学课程，系统而全面的介绍：光栅化、几何表示、光的传播理论、动画与模拟。每个方面都从基础原理出发讲解到实际应用，并介绍前沿的理论研究。本文是其 4 章（进阶变换）的学习笔记。

<!--katex-->

## 1. 反向旋转

已知二维图形旋转 $\theta$ 其变换矩阵为：

$$ 
R_{\theta} = \left(
\begin{matrix} \cos{\theta} & -\sin{\theta} \\ \sin{\theta} & \cos{\theta} \end{matrix} \right) 
\tag{1}
 $$
 
 使用同样的求法可知反向旋转相同角度时：
 
 $$
 R_{\theta} = \left(
    \begin{matrix} \cos{\theta} & \sin{\theta} \\ -\sin{\theta} & \cos{\theta} \end{matrix} \right) \tag{2}
$$

实际上，从定义上说正向和反向旋转时互逆的过程，而对于该类矩阵实际上其逆即为转置；转置等于其逆的矩阵在数学上称为正交矩阵（Orthogonal Matrix）。

## 2. 3D Transformations

### 3d 仿射变换

同样使用其次坐标定义：

- 3d point $ = (x, y, z, 1)^T $
- 3d vector $ = (x, y, z, 0)^T $

同样，对于 $(x, y, z, w) (w != 0)$ 其值相当于 $(x/w, y/w, z/w)$，有：

$$
\left( \begin{matrix} x’ \\ y’ \\ z’ \\ 1 \end{matrix} \right) = 
\left( \begin{matrix} a & b & c & t_x \\ d & e & f  & t_y \\ g & h & i & t_z \\ 0 & 0 & 0 & 1 \end{matrix} \right) \cdot
\left( \begin{matrix} x \\ y \\ z \\ 1 \end{matrix} \right)
\tag{3}
$$

也是先应用线性变换，再运用平移变换
 
### 缩放
 
 $$
 S( s_x , s_y, s_z ) =
 \left( \begin{matrix} s_x & 0 & 0 & 0 \\ 0 & s_y & 0 & 0 \\ 0 & 0 & s_z & 0 \\0 & 0 & 0 & 1 \end{matrix} \right) \tag{4}
 $$

### 平移

$$
T(t_x, t_y, t_z) = 
\left( \begin{matrix} 1 & 0 & 0 & t_x \\ 0 & 1 & 0 & t_y \\ 0 & 0 & 1 & t_z \\ 0 & 0 & 0 & 1 \end{matrix} \right) \tag{5}
$$ 

### 旋转

#### 指定轴的旋转表示

对于 x 轴：

$$
R_x(\alpha) = 
\left( \begin{matrix} 1 & 0 & 0 & 0 \\ 0 & \cos{\alpha} & -\sin{\alpha} & 0 \\ 0 & \sin{\alpha} & \cos{\alpha} & 0 \\ 0 & 0 & 0 & 1 \end{matrix} \right) \tag{6}
$$

对于 y 轴：

$$
R_y(\beta) = 
\left(\begin{matrix}
\cos{\beta} & 0 & \sin{\beta} & 0 \\
0 & 1 & 0 & 0 \\
-\sin{\beta} & 0 & \cos{\beta} & 0 \\
0 & 0 & 0 & 1
\end{matrix} \right) \tag{7}
$$


对于 z 轴：

$$
R_z(\gamma) = 
\left(\begin{matrix}
    \cos{\gamma} & -\sin{\gamma} & 0 & 0 \\
    \sin{\gamma} & \cos{\gamma} & 0 & 0 \\
    0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 1
\end{matrix} \right)
\tag{8}
$$
 
 其中由于循环对称，使用右手定则容易注意到：
 
 - for x-axis, y and z -> x
 - for y-axis, z and x-> -y
 - for z-axis, x and y-> z

 故而存在对 y 轴的旋转形式与其他轴旋转情况不同的结果。
 
#### 旋转的组合
 
 对于任意 3D 旋转可以拆分成多个不同轴的旋转的组合，称为欧拉角（Euler angles）。假设飞机航向为 y 轴正方向，常见称呼为：
 
 - pitch 倾角，沿 x 轴旋转
 - roll 翻滚，沿 y 轴旋转
 - yaw 转向，沿 z 轴旋转

 其表示为：
 
 $$
 R_{xyz}(\alpha, \beta, \gamma) = R_x(\alpha)R_y(\beta)R_z(\gamma) \tag{9}
 $$
 
#### Rodrigues’ 旋转公式

任意旋转可以表示使用角度 $\alpha$ 和轴 $n$ 表示：

$$ 
R(n, \alpha) = 
    \cos(\alpha) I + 
    (1 - \cos(\alpha) )nn^T + 
    \sin(\alpha)\left(\begin{matrix}0 & -n_z & n_y \\ n_z & 0 & -n_x \\ -n_y & n_x & 0 \end{matrix} \right)
\tag{10}
$$

- 对于不过原点的轴， 相当于将原点移动到轴上，旋转后，移动回去
- 式 10 的最右矩阵实际上是相当于使用轴进行叉乘

四元数的引入更多地为计算旋转和旋转之间的差值

## 视图/摄影机变换

### 什么是视图变换？

考虑如何拍照：

1. 寻找一个地点并且准备被摄物体（模型变换）
2. 发现一个好的角度并且设置一个摄像机（视图变换）
3. 拍照（投影变换）

### 如何进行视图变换？

相机的定义：

- 位置 Position: $\vec{e}$
- 朝向 Look-at/gaze direction: $\vec{g}$
- 朝上 Up direction: $\vec{t}$

关键的观察：当相机和所有物体一起移动时，照片一样；可视为相机固定于原点，Y 为上，-Z 为朝向，所有物体相对于相机移动，减少了复杂度。

视图变换方法：

1. 变换到原点
2. g 旋转到 -Z
3. t 旋转到 Y
4. g 叉乘 t 旋转到 X

数学上的定义：

因此，视图变换可以分解为视图位移和视图旋转：

$$
M_{view} = T_{view} R_{view}
\tag{11}
$$

其中，视图位移易得：

$$
T_{view} = \left( \begin{matrix}
1 & 0 & 0 & -x_e \\
0 & 1 & 0 & -y_e \\
0 & 0 & 1 & -z_e \\
0 & 0 & 0.& 1 \end{matrix} \right)
\tag{12}
$$

若考虑将世界轴向摄像机轴移动，则有：

$$
R_{view}^{-1} = \left( \begin{matrix}
    x_{\vec{g} \times \vec{t} } & x_t & x_{-g} & 0 \\
    y_{\vec{g} \times \vec{t} } & y_t & y_{-g} & 0 \\
    z_{\vec{g} \times \vec{t} } & z_t & z_{-g} & 0 \\
    0 & 0 & 0 & 1 \end{matrix} \right)
\tag{13} 
$$

由式 1 和式 2 得，摄像机轴向世界轴的移动为式 13 的非拓展部分转置，有：

$$
R_{view} = \left( \begin{matrix}
    x_{\vec{g} \times \vec{t} } & y_{\vec{g} \times \vec{t} } &  z_{\vec{g} \times \vec{t} } & 0 \\
    x_t & y_t & z_t & 0 \\
    x_{-g} & y_{-g} & z_{-g} & 0 \\
    0 & 0 & 0 & 1 \end{matrix} \right)
\tag{14}
$$

模型变换和视图变换可以结合到一起，称为模型视图变换。

## 投影变换

### 图形学中的投影

- 3D 到 2D 的变换
- 正交投影（Orthographic Projection）：
    - 平行线的延长仍然平行，用于工程制图
    - 假设相机离得无限远，远近切割平面大小相等
- 透视投影（Perspective Projection）：
    - 平行线的延长会相交，人眼的视角
    - 认为摄像机本身是一个点，将远处的较大切割平面投射到近处较小的切割平面


### 正交投影

简单的理解：

- 摄像机进行视图变换
- 移除 Z 轴（对于前后区分，若无透明度，可以最小正距离为准；否则以透明度公式计算）
- 移动并缩放结果到 -1 到 1 的矩形上（约定俗成，方便后续计算）

正式：试图映射一个 $[l,r] \times [b, t] \times [f, n] $ 立方体（cuboid）到正则（规范、标准？canonical）立方体 $[-1,1]^3$

$$
M_{ortho} = 
\left( \begin{matrix}
    \frac{2}{r-l} & 0 & 0 & 0 \\
    0 & \frac{2}{t-b} & 0 & 0 \\
    0 & 0 & \frac{2}{n-f} & 0 \\
    0 & 0 & 0 & 1 \end{matrix} \right)
\left( \begin{matrix}
    1 & 0 & 0 & -\frac{r+l}{2} \\
    0 & 1 & 0 & -\frac{t+b}{2} \\
    0 & 0 & 1 & -\frac{n+f}{2} \\
    0 & 0 & 0 & 1 \end{matrix} \right)    
\tag{15}
$$

该方法可以便于裁剪（OPENGL 使用左手系）

### 透视投影

#### 透视投影概述

- 计算机图形学中最广泛使用的
- 近大远小

回顾：齐次坐标中，点的每个维度乘以任何一个非 0 相同值仍然表示同一个点

#### 如何进行透视投影？

对于透视投影，设置远近平面，称为 frustum；对于正交投影，设置远近平面，称为 cuboid。有基本思路：

- 将 frustum 挤压（squish）成为 cuboid
- 进行正交投影

规定：

- 近平面永远不变
- z 轴不变
- 远平面中心点不变

挤压变换，根据相似三角形得：

$$
y^{’} = \frac{n}{z} y
\tag{16}
$$ 

由齐次坐标同乘不变得：

$$
\left( \begin{matrix} x \\ y \\ z \\ 1 \end{matrix} \right) \Rightarrow
\left( \begin{matrix} nx/z \\ ny/z \\ unknown \\ 1 \end{matrix} \right) \equiv
\left( \begin{matrix} nx \\ ny \\ unknown \\ z \end{matrix} \right)
\tag{17}
$$

则有转换矩阵为：

$$
M_{persp \rightarrow ortho} = 
\left( \begin{matrix}
n & 0 & 0 & 0 \\
0 & n & 0 & 0 \\
? & ? & ? & ? \\
0 & 0 & 1 & 0 \\
\end{matrix} \right)
\tag{18}
$$

存在以下两个观察：

- 任何一个点在近平面上不会变化
- 任何点在远的平面上 z 不会变化

由以上观察 1 得：

$$
M_{persp \rightarrow ortho}^{(4 \times 4)} \left( \begin{matrix} x \\ y \\ z \\ 1 \end{matrix} \right) \equiv \left( \begin{matrix} nx \\ ny \\ unknown \\ z \end{matrix} \right)
\tag{19}
$$

替换 z 为 n 时，有：

$$
\left( \begin{matrix} x \\ y \\ z \\ 1 \end{matrix} \right) \Rightarrow
\left( \begin{matrix} x \\ y \\ n \\ 1 \end{matrix} \right) \equiv
\left( \begin{matrix} nx \\ ny \\ n^2 \\ n \end{matrix} \right)
\tag{20}
$$

因此第三行必须有以下形式：

$$
\left( \begin{matrix} 0 & 0 & A & B \end{matrix} \right)  \left( \begin{matrix} x \\ y \\ n \\ 1 \end{matrix} \right) = n^2 \Rightarrow An + B = n^2
\tag{21}
$$

由观察 2 得：

$$
\left( \begin{matrix} 0 & 0 & f & 1\end{matrix} \right) \equiv \left( \begin{matrix} 0 & 0 & f^2 & f \end{matrix} \right) \Rightarrow Af + B = f^2
\tag{22}
$$

由式 21 式 22 得：

$$
\begin{aligned}
An + B &= n^2 \\ 
Af + B &= f^2
\end{aligned}
\tag{23}
$$

得：

$$
\begin{aligned}
A &= n + f \\
B &= -nf
\end{aligned}
\tag{24}
$$