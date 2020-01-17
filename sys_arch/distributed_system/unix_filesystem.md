# 分布式系统 - Unix 文件系统

## 1. 文件系统为共建任何信息系统提供了基础服务

**作用**：文件存储、管理、检索（retrieve）服务

着眼于**两个文件操作**：

- `fp = open("cs4274/exam", r, 0)`
- `r = read(fp, n, buf)`

## 2. 硬盘结构：

**头，柱面（`cylinder`），磁道（`tracks`）扇区**：

- 数据首先存储在相同的柱面以减少磁头的移动
- 磁盘操作以扇区作为基本单位

**数据块和块数字**：

- 文件系统使用块（`block`）作为磁盘操作的数据单位
    - 通常一个块大于`1K`（>=扇区大小）
    - 磁盘块通常以其块`#`指代
- 块 `#` 到物理地址的翻译
    - `sectors  = blk_num * (#sects_per_blk)`
    - `cyld_num = sectors / (#sects_per_cyld)`
    - `head_num = (sectors % (#sects_per_cyld))/(#sects_per_track)`
    - `sect_num = sectors % (#sects_per_track)`

## 3. 内部文件结构：

### 3.1 每个文件对应一个 i-node: 文件属性和其数据索引：

- 文件属性
    - 所有者、访问权限（access permissions）、访问时间（access times）、文件尺寸
    - `12 bytes`
- 数据索引：
    - `13 indices： 10 direct indices、1 indirect index、1 double indirect index、1 triple indirect index`
    - `13 * 4 = 52 bytes`
- 一个 `inode` 的尺寸，`64bytes`
- 所有的 `inode` 被其 `inode #` 索引

### 3.2 Unix目录（Directory）文件：

- 目录文件是含有名到 `i-node#` 的映射项的通常文件：
- 在 BSD4.3 之后，文件名可以是变长的（最长255个字节）。每个项中包含项长度、名称和 `inode#`

### 3.3 目录结构和名称分辨力：

- 目录是树状结构
- 路径名称递归地解析到 `i-node#`，从根组件到组件的解析到相对目录

### 3.4 磁盘管理：

**`inode#` 到 `i-nodes`**

**超级块**：

- 每个文件系统拥有一个超级块，其包含：
    - 文件系统的尺寸
    - 系统中空闲块的数量
    - 一个缓存空闲块的数组
    - 下一个空闲块序号的索引
    - 到 `i-node` 区域开始的指针
    - 系统中空闲 `i-node` 的数量
    - 缓存的空闲 `i-node` 的列表
    - 下一个空闲 `i-node` 的索引
- 超级块（通常是第一个磁盘块）是文件系统的根，在系统启动时必须载入
- 通过 `inode` 序号计算 `inode` 地址：
    - `blk_num = (inode_num -1) / #inodes_per_blk + st_addr_inodes`
    - `byte_offset = ((inode_num - 1) mod (#inodes_per_blk)) x inode_size`
    - N.B. In UNIX, `inode_size = 64`, `#inodes_per_blk = 16`
- 地址翻译的总结：
    - `pathname` -> `inode#` -> `inode address (block # & offset)` -> `inode` -> `data block #` -> `data address` -> `data`

## 4. 全局文件组织

### 4.1 磁盘分区（partitions）

- 磁盘单元被分区为段
- 每个段被设置成文件系统
- `inode` 号和磁盘块号被文件系统验证

### 4.2 挂载和解挂操作

- 操作：
    - `mount(dev_name, dir_name, options);` 
    - `umount(dev_name)`
- 在 `incore inode table` 中标记为 `mount point`
- 在 `mount table` 中挂载到 `inode` 上
- 跨 `mount point` 的名称解析：
    - 当内核跨挂载点解析路径名的时候，通过 `inode` 知晓其挂载点
    - 首先查询挂载表项，其包含超级块的地址和一个指向根 `inode` 的指针
    - 其接着访问挂载文件系统的根节点，接下来的名称解析在挂载文件系统内部进行解析。

## 5. 链接文件

- 操作：
    - `link(sour_fname,dest_fname)`
- 文件系统首先查找源文件的 inode#
- 搜索父目录来找到目的名
- 向父目录添加目的文件的项，添加目的文件和inode#的对
- 注意：链接不能够跨文件系统（软链接可以）
