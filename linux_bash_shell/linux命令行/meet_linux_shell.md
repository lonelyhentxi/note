# 初识 linux shell

---

## 1.1 什么是 linux

linux可以分为以下四部分:
- Linux 内核
- GNU 工具
- 图形化桌面环境
- 应用软件

### 1.1.1 深入探究 linux 内核

**概述**

- 内核是 Linux 系统的核心
- 控制计算机系统上所有的软件和硬件
- 必要时分配硬件 , 根据需要执行软件

**历程与维护**

诞生历程参见 Linus Torvalds 所著自传 *Just For Fun* , 历史上内核的维护经历三个阶段
- 个人维护
- 网友发布代码 , Linus 审核
- 开发小组维护

**主要功能**

- 系统内存管理
- 软件程序管理
- 硬件设备管理
- 文件系统管理

**系统内存管理**

1. 管理机器上可用的物理内存
2. 创建和管理虚拟内存
   - 虚拟内存通过硬盘的储存空间实现 , 称为 swap space
3. 内存分块 (page) 储存 , 内核维护内存页面表 : 记录正在使用的内存 , 将长时间未使用的放入虚拟内存 (换出,swapping out) , 当要访问页面时 , 需要调入内存

**软件程序管理**

1. linux 将系统中正在运行的程序称作进程 , 可以在前台 , 后台运行
2. 内核会创建一个 init 进程来启动系统上所有其他的进程
   - 内核启动时 , init 进程会放入虚拟内存中
   - 内核在启动任何进程时 , 都会在虚拟内存中开设一块来储存该进程用到的数据和代码
   - 一些 linux 发行版使用在 /etc/inittab 中的一个表来管理开机启动的进程
   - 另一些系统 (如ubuntu) 储存在 /etc/init.d 中 , 将开机时启动或停止某个应用的脚本放在这个目录下 , 这些脚本通过 /etc/rcX.d 的符号链接使用
3. linux 系统的 init 系统采用了 5 个不同的运行级
   - 只启动基本的系统进程和一个控制台终端进程 -- 单用户模式 -- 用来在系统有问题时紧急维护 , 通常只有管理员能够登录
   - 标准的启动运行级是 3 , 大多数应用软件 , 比如网络支持程序 , 都会启动.
   - 5 级允许用户通过图形化窗口 (X window) 控制

**硬件管理程序**

任何 Linux 系统需要与之通信的设备 , 都需要在内核代码中加入其驱动代码 ,  他是应用程序和硬件设备之间的中间人

以下是两种插入设备代码的方式:
- 编译进入内核的设备驱动代码
  - 必须重新编译内核
- 可插入内核的设备驱动代码模块
  - 允许将驱动代码热插入内核 , 不再使用可以移走

Linux 系统将硬件当成特殊的文件 , 称为设备文件 , 有三种分类
1. 字符型设备文件
    - 每次只能处理一个字符的设备 : 大多数调制解调器和终端
2. 块设备文件
    - 每次可以处理大块数据的设备 : 硬盘
3. 网络设备文件
    - 采用数据包发送和接收数据的设备 : 包括各种网卡和一个特殊的回环设备 (允许 Linux 使用常见的网络编程协议同自身通信)

Linux 为每个设备创建一种称为节点的特殊文件 , 与设备的通信都通过节点完成 , 每个节点都有唯一的数值对供内核标识 : 包括主设备号和次设备号 , 前者指类似的设备 , 后者标识特定的设备

**文件系统管理**

linux 支持不同的文件格式从硬盘中读取数据 : 包括自身和其他系统的格式 , 内核必须在编译时就加入对这些格式的支持

<table>
<tr>
<th>文件系统</th>
<th>描述</th>
</tr>
<tr>
<td>ext</td>
<td>linux拓展文件系统 , 最早的linux文件系统</td>
</tr>
<tr>
<td>ext2</td>
<td>在以上拓展更多功能</td>
</tr>
<tr>
<td>ext3</td>
<td>支持日志</td>
</tr>
<tr>
<td>ext4</td>
<td>支持高级日志</td>
</tr>
<tr>
<td>hpts</td>
<td>os/2高性能文件系统</td>
</tr>
<tr>
<td>jfs</td>
<td>IBM日志文件系统</td>
</tr>
<tr>
<td>iso9660</td>
<td>CD-ROM的iso 9660文件系统</td>
</tr>
<tr>
<td>msdos</td>
<td>微软的FAT16</td>
</tr>
<tr>
<td>minix</td>
<td>MINIX文件系统</td>
</tr>
<tr>
<td>ncp</td>
<td>netware文件系统</td>
</tr>
<tr>
<td>nfs</td>
<td>网络文件系统</td>
</tr>
<tr>
<td>ntfs</td>
<td>支持nt内核的文件系统</td>
</tr>
<tr>
<td>proc</td>
<td>访问系统信息</td>
</tr>
<tr>
<td>ReiserFS</td>
<td>高级Linux文件系统,提供更好的性能和硬盘恢复功能</td>
</tr>
<tr>
<td>smb</td>
<td>支持网络访问的samba smb文件系统</td>
</tr>
<tr>
<td>sysv</td>
<td>较早期的Unix文件系统</td>
</tr>
<tr>
<td>ufs</td>
<td>BSD文件系统</td>
</tr>
<tr>
<td>umdos</td>
<td>建立在msdos上的类unix文件系统</td>
</tr>
<tr>
<td>vfat</td>
<td>FAT32</td>
</tr>
<tr>
<td>XFS</td>
<td>高性能64位日志文件系统</td>
</tr>
</table>

linux系统所访问的硬盘必须格式化成为列表所列出来的其中一种

linux内核采用虚拟文件系统 (Virtual File System,VFS) 作为每个文件系统和系统交互的接口 , 每当文件系统被挂载和使用时 , VFS信息将会被存在内存中

### 1.1.2 GNU 工具

GNU , linux 除内核之外 , 用来实现一些标准功能的工具 :

ps:
- GNU (GNU's Not Unix) 在名为开源软件的软件理念下 , 开发了一套完整的 Unix 工具 
- 开源软件理念允许程序员开发软件 , 并将其免费发布 : 任何人都可以使用 , 修改该软件 , 或者集成进入自己的系统

**核心 GNU 工具**

GNU coreutils 软件包:
1. 用以文件处理的工具
2. 用以操作文本的工具
3. 用以进程管理的工具

**shell**

- GNU/Linux shell 是一种特殊的交互式工具 : 提供了启动程序 ,  管理文件系统中的文件和运行的进程的途径
- shell 的核心是命令提示符: 命令提示符是 shell 负责交互的部分 : 允许输入文本命令 解释命令 在内核中执行
- shell 包含了一组内部命令
- 将多个 shell 命令放入文件中作为程序执行 , 称为 shell 脚本
- linux 上 , 有多个不同特性的 shell 
    - 默认shell -- bash shell

<table>
<tr>
<th>shell</th>
<th>描述</th>
</tr>
<tr>
<td>ash</td>
<td>运行在内存受限环境中的与bash shell完全兼容的轻量级shell</td>
</tr>
<tr>
<td>korn</td>
<td>与unix shell(Bourne shell)兼容的shell,支持诸如数组和浮点等高级程序特性</td>
</tr>
<tr>
<td>tcsh</td>
<td>将c语言中一些元素引入shell脚本的shell</td>
</tr>
<tr>
<td>zsh</td>
<td>结合bash/korn/tcsh特性,提供高级编程特性,共享历史文件和主题化提示符的高级shell</td>
</tr>
</table>

### 1.1.3 Linux 桌面环境

Linux 提供多种图形化界面:

**X window**

- X window 是直接和显卡和显示器打交道的底层程序
- Linux 并不是唯一使用该图形系统的操作系统
- X window 的实现不止一种
- X.org 提供了 X windows 的开源实现 , 支持市面上的新显卡 , 因此最为流行
- 核心的 X window 提供了独立程序运行的图形化显示界面 , 但没有桌面环境
- Fedora 采用了实验性的 Wayland , 以及 Ubuntu 的 Mir

**KDE 桌面(openSUSE : KDE 4)**

K Desktop Environment 在1996年作为开源项目发布 , 会产生类似于 windows 的图形化桌面环境

**GNOME 桌面**

the GNU Network Object Model Environment 与1999年发布 , 最为流行 , 成为以 Red Hat , CentOS 为首的许多发行版的首选

尽管不再沿用 windows 的标准观感 , 但仍然继承许多 windows 的优秀功能

**Unity 桌面**

unity 用于 ubuntu , 得名于其目的 , 为工作站 , 平板电脑 , 移动设备提供一套一致的桌面体验

**其他桌面**

一些轻量级图形化桌面
<table>
<tr>
<th>桌面</th>
<th>描述</th>
</tr>
<tr>
<td>Fluxbox</td>
<td>没有面板的轻型桌面 , 仅有一个可用来启动程序的弹出式菜单</td>
</tr>
<tr>
<td>Xfee</td>
<td>类KDE的轻量级系统</td>
</tr>
<tr>
<td>JWM</td>
<td>joe的窗口管理器,适用于低内存低硬盘的超轻型桌面</td>
</tr>
<tr>
<td>Fvwm</td>
<td>支持诸如虚拟桌面和面板等高级功能,但可以在低内存环境运行</td>
</tr>
<tr>
<td>fvwm95</td>
<td>fvwm衍生出的观感类似windows95桌面</td>
</tr>
</table>

---

## 1.2 Linux 发行版

完整的 linxu 系统包被称为发行版,不同的发行版常常归为3类
- 完整的核心linux发行版
- 特定用途的发行版
- LiveCD 测试发行版

### 1.2.1 核心 Linux 发行版

核心 Linux 发行版含有内核 , 一个或多个图形化桌面环境以及预编译好的几乎所有能够见到的 Linux 应用 , 提供了一站式的完整 Linux 安装

<table>
<tr>
<th>发行版</th>
<th>描述</th>
</tr>
<tr>
<td>Stackware</td>
<td>最早发行版中的一员,在linux极客中比较流行</td>
</tr>
<tr>
<td>Red Hat</td>
<td>用于web服务器</td>
</tr>
<tr>
<td>Fedora</td>
<td>从RH分离出的家用发行版</td>
</tr>
<tr>
<td>Gentoo</td>
<td>为高级用户设计的发行版,仅包含Linux源代码</td>
</tr>
<tr>
<td>openSUSE</td>
<td>用于商用和家用的发行版</td>
</tr>
<tr>
<td>Debian</td>
<td>在linux专家和Linux商用产品中流行的发行版</td>
</tr>
</table>

### 1.2.2 特定用途的 Linux 发行版

1. 基于某个主流发行版 , 仅包含主流发行版中一部分用于特定用途的应用程序
2. 尝试通过自动检测和自动配置常见硬件引导安装

<table>
<tr>
<th>发行版</th>
<th>描述</th>
</tr>
<tr>
<td>CentOS</td>
<td>基于RH企业版Linux源代码构建的免费发行版</td>
</tr>
<tr>
<td>Ubuntu</td>
<td>用于学校和家庭的免费发行版</td>
</tr>
<tr>
<td>PCLinuxOS</td>
<td>用于家庭和办公的免费发行版</td>
</tr>
<tr>
<td>Mint</td>
<td>用于家庭娱乐的免费发行版</td>
</tr>
<tr>
<td>dyne:bolic</td>
<td>用于音频和MIDI应用的免费发行版</td>
</tr>
<tr>
<td>Puppy Linux</td>
<td>用于老旧PC的小型免费发行版</td>
</tr>
</table>

**Linux LiveCD**

略 , 类似于 windows to go

取决于基质和系统 , 对诸如读写性有诸多的限制


