# Blender 学习笔记 - 基本操作、物体模式和编辑模式

## 1. 基本操作：

### 新建物件：shift + a

- 普通模式 - 相加
- 编辑模式 - 新建网格（在原始对象上新建）

### 多选：

- 鼠标左键：替代单选
- alt + 鼠标左键：圈选中
- shift + 以上操作：追加选中

### 视图快捷选单：~

### 删除：

- 无确认删除：del
- 有确认删除：x
	- 普通模式下：整体删除
	- 编辑模式下：可选择点/线/面，或者其组合删除

###  切换普通模式和编辑模式：tab

###  空格：space 

-  调出左侧菜单（需要在编辑 - 偏好 - 键位映射 中，将 Spacebar Action 选为工具）
- 此时，shift + space 变为录制 

###  全选：a；取消全选：ctrl + a

- 普通模式 - 全选
- 编辑模式 - 选择网格

###  在编辑模式下选中类型：

a. 点：1
b. 线：2
c. 面：3
d. shift + 上述操作：追加选择

###  细分：

a. 编辑模式全选物体后，右键调出菜单，选择细分
b. ![blender_basic_subdivision](https://blog.evernightfireworks.com/static/2020/02/blender_basic_subdivision.png)

## 2. 工具栏：

###  缩放 scale：s

a. +x，y，z：能够控制单轴移动
b. 缩放罩体（缩放空间）

###  移动 grab：g

###  变换 transform：t

###  旋转 rotate：r

a. +x，y，z：能够控制单轴移动

###   框选：

- 默认：space + b
- 循环切换（调整/框选/刷选/套索选择）：w

###  游标：

- 默认：space + space
- 重置到中心：shift + c

###  标注：

- space + d
- 在右上角小工具 - 视图 - 标注中删除

###  测量：

- space + m
- 属于一种标注，在右上角小工具 - 视图 - 标注中删除

###  挤出 exclusive：

- 挤出 e：
	- 操作：拉动 ![blender_basic_exclude_region](https://blog.evernightfireworks.com/static/2020/02/blender_basic_exclude_region.png)
- 沿法线挤出
	- 操作：拉动
	- ![blender_basic_exlude_along_normals](https://blog.evernightfireworks.com/static/2020/02/blender_basic_exlude_along_normals.png)
- 挤出各个面
	- 操作：拉动
	- ![blender_basic_exclude_individual](https://blog.evernightfireworks.com/static/2020/02/blender_basic_exclude_individual.png)
- 挤出至光标
	- 操作：点按
	- ![blender_basic_exclude_to_cursor](https://blog.evernightfireworks.com/static/2020/02/blender_basic_exclude_to_cursor.png)

### 内插面 insert：I

- 操作：拉动
- ![blender_basic_insert_faces](https://blog.evernightfireworks.com/static/2020/02/blender_basic_insert_faces.png)

###  倒角：ctrl + b

- 操作：
	- 拉动增大倒角访问
	- 滚轮增加倒角面数
- ![blender_basic_bevel](https://blog.evernightfireworks.com/static/2020/02/blender_basic_bevel.png)

###  环切：

- 环切
	- ctrl r
	- ![blender_basic_loop_cut](https://blog.evernightfireworks.com/static/2020/02/blender_basic_loop_cut.png)
- 偏移环切边
	- shift + ctrl + r
	- ![blender_basic_offset_edge_loop_cut](https://blog.evernightfireworks.com/static/2020/02/blender_basic_offset_edge_loop_cut.png)
	- 选中环切后拖动，默认为对称偏移环切，可设置两侧倍数因子

###  切割 knife：

- 切割（切割新的拓扑结构）：k
	- 建议使用快捷键模式，功能如下：
		a) ![blender_basic_knife_function_1](https://blog.evernightfireworks.com/static/2020/02/blender_basic_knife_function_1.png)
		b) ![blender_basic_knife_function_2](https://blog.evernightfireworks.com/static/2020/02/blender_basic_knife_function_2.png)
	- ![blender_basic_knife](https://blog.evernightfireworks.com/static/2020/02/blender_basic_knife.png)
- 切分（沿着平面切割几何体）：操作 点选/拉动/旋转

###  多边形：

- 直接拖动
	- ![blender_basic_polygon_build_select_drag](https://blog.evernightfireworks.com/static/2020/02/blender_basic_polygon_build_select_drag.png)
- ctrl 按住 - 点击：
	- ![blender_basic_polygon_build_ctrl_click](https://blog.evernightfireworks.com/static/2020/02/blender_basic_polygon_build_ctrl_click.png)
- ctrl 按住 - 点击 - 点击：
	- ![blender_basic_polygon_build_ctrl_click_click](https://blog.evernightfireworks.com/static/2020/02/blender_basic_polygon_build_ctrl_click_click.png)
	- 尽量使用四边形，便于环切等操作
- shift 点击拓扑顶点：消除该拓扑顶点

###  旋绕 spin：

- 旋绕（以当前 3D 游标为中心连续挤出弧形）：
	- 旋转操作：![blender_basic_spin](https://blog.evernightfireworks.com/static/2020/02/blender_basic_spin.png)
	- 旋转加拉动操作（继续挤出）：![blender_basic_spin_drag](https://blog.evernightfireworks.com/static/2020/02/blender_basic_spin_drag.png) 
- 旋绕复制
	- 与旋绕差别在复制，不连接而是复制面
	- ![blender_basic_spin_duplicates](https://blog.evernightfireworks.com/static/2020/02/blender_basic_spin_duplicates.png)

###  光滑：

- 光滑：
	- shift + G
	- 对细分后的物体进行
	- ![blender_basic_smooth](https://blog.evernightfireworks.com/static/2020/02/blender_basic_smooth.png)
- 随机化：
	- 沿着法线随机偏移
	- ![blender_basic_randomize](https://blog.evernightfireworks.com/static/2020/02/blender_basic_randomize.png)

###  滑移：g + g

- 滑移边线：
	- 让边沿着现有的面进行滑动
	- ![blender_basic_edge_slide](https://blog.evernightfireworks.com/static/2020/02/blender_basic_edge_slide.png)
- 滑移顶点：
	- ![blender_basic_vertex_slide](https://blog.evernightfireworks.com/static/2020/02/blender_basic_vertex_slide.png)

###  法向缩放：

- 法向缩放：
	- alt + s
	- ![blender_basic_shrink_flatten](https://blog.evernightfireworks.com/static/2020/02/blender_basic_shrink_flatten.png)
	- scale 是所有面放大，法向缩放是只在法向上缩放
	- 法向挤出是挤出后法向缩放
- 推拉：
	- 向轴心点方向移动
	- ![blender_basic_push_pull](https://blog.evernightfireworks.com/static/2020/02/blender_basic_push_pull.png)
	- 可用于制造水滴，叶片等

###  切变

- 切变
	- shift + ctrl + alt + s
	- 按面对体进行搓拉
	- ![blender_basic_shear](https://blog.evernightfireworks.com/static/2020/02/blender_basic_shear.png)
- 球形化：
	- shift + alt + s
	- ![blender_basic_to_sphere](https://blog.evernightfireworks.com/static/2020/02/blender_basic_to_sphere.png)
	- 使封闭体更接近球形

###  断离区域：

- 断离区域： v
	- ![blender_basic_rip_region](https://blog.evernightfireworks.com/static/2020/02/blender_basic_rip_region.png)
- 断离边线：alt + d
	- ![blender_basic_rip_edge](https://blog.evernightfireworks.com/static/2020/02/blender_basic_rip_edge.png)
	
## 3. 编辑模式其他操作：


###  网格 - 拆分：y

- ![blender_basic_mesh_split](https://blog.evernightfireworks.com/static/2020/02/blender_basic_mesh_split.png)

###  网格 - 投影切割：

- 操作：
	- 在普通模式
		a) 点选投影源
		b) shift 点选投影目标
	- 在编辑模式
		a) 点击投影切割
- ![blender_basic_mesh_knife_project](https://blog.evernightfireworks.com/static/2020/02/blender_basic_mesh_knife_project.png)

###  顶点 - 顶点倒角：

- shift + ctrl + b
- 操作：
	- 点选点
	- 触发顶点倒角
	- 拖动（无需点击）改变范围
	- 滚轮改变细分程度
- ![blender_basic_vertex_bevel_vertecies](https://blog.evernightfireworks.com/static/2020/02/blender_basic_vertex_bevel_vertecies.png)

###  顶点 - 从顶点创建边/面 fill： f

- 对边：
	- 选择要填充的边
	- f 填充面，若是四边形，一条边即可填充
- 对顶点：
	- 按照选中的顶点数量，填充直线/多边形
- 该模式一次触发只会新建一个多边形面
- ![blender_basic_vertex_fill](https://blog.evernightfireworks.com/static/2020/02/blender_basic_vertex_fill.png)

###  顶点 - 连接顶点路径 joint：j

- 连接顶点，交叉的连线，能够自动形成新的顶点
- ![blender_basic_vertex_joint](https://blog.evernightfireworks.com/static/2020/02/blender_basic_vertex_joint.png)

###  顶点 - 顶点合并：alt + m

- ![blender_basic_vertex_merge_vertecies](https://blog.evernightfireworks.com/static/2020/02/blender_basic_vertex_merge_vertecies.png)

###  边 - 桥接循环边：

- 对同一物体中的多个部分的循环边进行桥接
- ![blender_basic_edge_bridge_edge_loops](https://blog.evernightfireworks.com/static/2020/02/blender_basic_edge_bridge_edge_loops.png)
- ![blender_basic_edge_bridge_edge_loops_control](https://blog.evernightfireworks.com/static/2020/02/blender_basic_edge_bridge_edge_loops_control.png)
- ![blender_basic_edge_bridge_edge_loops_subdivision](https://blog.evernightfireworks.com/static/2020/02/blender_basic_edge_bridge_edge_loops_subdivision.png)

###  边 - 边线折痕：

- ![blender_basic_edge_edge_crease](https://blog.evernightfireworks.com/static/2020/02/blender_basic_edge_edge_crease.png)

###  边 - 标记缝合边：

- 注：
	- UV 展开将物体像纸盒一样展开，便于贴图
	- 使用标记缝合边可以让 blender 知道哪些区域是缝合边

###  边 - 标记锐边：

- 避免 平滑作用
	- 普通模式，右键物体 - 平滑着色
	- 右侧面板 - 物体数据属性 - 法向 - 自动光滑 - 角度值
- ![blender_basic_edge_mark_sharp](https://blog.evernightfireworks.com/static/2020/02/blender_basic_edge_mark_sharp.png)

###  边 - 标记 freestyle 边：

- 凸显 freestyle 渲染下

###  面 - 三角化：

- 三角化：
	- 将四边形变成三角形
	- ctrl + t
	- ![blender_basic_face_triangulate_faces](https://blog.evernightfireworks.com/static/2020/02/blender_basic_face_triangulate_faces.png)
- 三角面 - 四边面：
	- alt + j
	- 将三角面反置为四边面

###  面 - 完美建面

- 将复杂物体转换出的不规则网格设置规则化：
	- 物体模式 
	- 物体 - 转换为 - 曲线/曲面……转网格
- 操作：
	- 编辑模式
	- 面 - 完美建面
- 作用：
	- 拯救强迫症
	- 减少计算量，方便渲染

###  选择 - 选择相联元素 - 关联项：ctrl + l

- 选中与当前选中项关联的所有项
- ![blender_basic_select_select_linked](https://blog.evernightfireworks.com/static/2020/02/blender_basic_select_select_linked.png)

###  面 - 交集：

- 交集切割：
	- ![blender_basic_intersect_knife](https://blog.evernightfireworks.com/static/2020/02/blender_basic_intersect_knife.png)
- 交集布尔 - 差值
	- ![blender_basic_intersect_boolean_sub](https://blog.evernightfireworks.com/static/2020/02/blender_basic_intersect_boolean_sub.png)
- 交集布尔 - 并集
	- ![blender_basic_intersect_boolean_union](https://blog.evernightfireworks.com/static/2020/02/blender_basic_intersect_boolean_union.png)
- 交集布尔 - 交集
	- ![blender_basic_intersect_boolean_intersect](https://blog.evernightfireworks.com/static/2020/02/blender_basic_intersect_boolean_intersect.png)

## 4. 上方中置工具：


###  变换坐标系

- ![blender_basic_transform_orientation](https://blog.evernightfireworks.com/static/2020/02/blender_basic_transform_orientation.png)
- 分类：
	- 全局：世界坐标系
	- 局部：物体自身坐标系
	- 法向：一般用于面的法线
	- 万向：将每个坐标轴作为输入作用到欧拉旋转轴上
	- 视图：视图的平面坐标系，用于动画
	- 游标：相对于游标的坐标系

###  变换轴心点

- ![blender_basic_transform_pivot_point](https://blog.evernightfireworks.com/static/2020/02/blender_basic_transform_pivot_point.png)
- 分类：
	- 边界框中心：边界框的中心
	- 3D 游标：3D 游标
	- 各自的原点
	- 质心点：物体的密度 \* 位置向量的期望
	- 活动元素

###  吸附：

- ![blender_basic_snap_to](https://blog.evernightfireworks.com/static/2020/02/blender_basic_snap_to.png)
- shift + tab
- 分类：
	- 增量：吸附可视的单元背景网格
	- 体积：吸附到其他物体的某个基准点
	- 其他顾名思义，会有黄色点指示吸附点

###  衰减编辑：

- 快捷键：o
- ![blender_basic_proportional_editing](https://blog.evernightfireworks.com/static/2020/02/blender_basic_proportional_editing.png)
- 操作：
	- 点击拉动控制幅度
	- 鼠标滚轮控制范围
- 相联项可以控制是否影响无关因素

###  测量：

- 操作：
	- 编辑模式
	- ![blender_basic_viewport_overlays_measurements_config](https://blog.evernightfireworks.com/static/2020/02/blender_basic_viewport_overlays_measurements_config.png)
- ![blender_basic_viewport_overlays_measurements](https://blog.evernightfireworks.com/static/2020/02/blender_basic_viewport_overlays_measurements.png)

## 5. 第一次作业

教程中第一次练习的内容是对自己当前所用的笔记本电脑进行建模，由于还没看到曲线部分，所以建的并不是很完美。

![blender_basic_assignment_1](https://blog.evernightfireworks.com/static/2020/02/blender_basic_assignment_1.png)

此外，还初步尝试了一下材质和光影。

![blender_basic_assignment_2](https://blog.evernightfireworks.com/static/2020/02/blender_basic_assignment_2.png)

经过一些调整，最终效果如图。

![blender_basic_assignment_2_new](https://blog.evernightfireworks.com/static/2020/02/blender_basic_assignment_2_new.png)