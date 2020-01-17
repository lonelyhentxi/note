# 分布式系统 - 物理时钟

## 1. 为什么在系统中需要时间戳？

- 性能表现的精确测量
- 确保最新或者最近的数据
- 并发过程产生事件的时间（temporal）顺序
- 在发送方和接收方之间同步消息
- 协调联合活动
- 对共享对象并发访问的串行化

## 2. 物理时间：

- 太阳时间
- 格林尼治时间
- 协作标准时间（UTC，Coordinated Universal Time）
    - 国际原子时间 TAI
        - 准确性：六百万年一秒以内
        - 问题：原子时间和太阳时间不能对应
    - 通用协调时间：
        - 基于原子时间，从1972年1月1日开始
        - 当太阳时间和TAI差距大于1秒时，有时候会插入或者删除一秒保持同步

## 3. 电脑时间：

### 3.1 概述：

- 互补式氧化半导体（CMOS）时钟电路被石英（quartz）振荡器（oscillator）驱动
- 当电池用尽时备用电池可以顶上去

### 3.2 原理：

电路有一个计数器和一个寄存器。计数器每次震荡减一，当它到达0的时候，寄存器中的值会被读入计数器中，并发出一个中断。

### 3.3 时钟漂移（drift）和时钟偏斜（skew）

#### 3.3.1 概念

时钟漂移（drift）：

- 时钟滴答速率不同
- 会在感知时间中扩大差距

时钟偏斜（skew/offset）：两个时钟对于同一个时间点不同

### 3.3.2 完美的时间

![perfect_time](https://blog.evernightfireworks.com/static/2019/11/perfect_time.png)
![skew_time](https://blog.evernightfireworks.com/static/2019/11/skew_time.png)

### 3.3.3 处理漂移

方法：

- 倒退时间没有好处，向后移动的错觉（illusion）可能会混淆消息排序和软件开发环境
- 如果快了，让其变慢；如果满了，让其变快

线性补偿函数 Linear compensating function

重新同步 Resynchronization

- 当到达同步周期后，重新同步，或者偏斜会超出阈值
- 通过 unix 函数 adjtime 来调整时间

### 3.3.4 得到UTC：

**从顶级源头得到时间**

方式：

- GPS
- WWV
- GOES

对每台设备使用不现实：

- 成本
- 尺寸
- 便利性
- 环境

**为客户端电脑得到时间：**

- 外部时间同步
- 使用 RPC 同步时间
    - 使得 RPC 包含时间
    - 用服务器时间来设置本地时间
    - 没有计算延迟

**克里斯汀（Cristian）算法**

原理：

- ![cristian_alg](https://blog.evernightfireworks.com/static/2019/11/cristian_alg.png)
- T_client = T_server + (T_1 - T_0)/2

准确度： +-round_trip_time/2

错误边界：

- ![error_bound](https://blog.evernightfireworks.com/static/2019/11/error_bound.png)
- T_min: 最小消息传播时间

问题：

- 服务器可能失败
- 受到(subject to)恶意(malicious)干扰(interference)

**伯克利（Berkeley）算法**

- 历史：由 Gusella 和 Zatti 在1989 年在 BSD 版本的 unix 上提出
- 目的：尽可能使一组计算机的时钟同步
- 假设：没有机器有准确的时间源
- 方法：从参与的计算机中获取平均值，将所有计算机同步到平均
- 算法：
    - 一个节点被选为主节点，其它作为从节点
    - 主节点定期轮询（periodically poll）所有的从节点，询问时间，此处可以使用克里斯丁算法
    - 计算平均值
    - 向每个从站发送需要调整的时钟的偏移量
    - 通过发送偏移而不是时间戳来避免网络延迟问题
- 特性：
    - 有规定（provisions）忽略偏移时间过大的节点
        - 任何从节点可以在主节点失败的时候接管主节点

**Network Time Protocol（NTP）**


概要：是最常用的因特网时间协议，并且提供了最佳的准确性（RFC 1305），计算机通常在 OS 中包含 NTP 软件，在端口 123 侦听 NTP 请求，并以 NTP 格式答复 UDP/IP 数据包。

注意：
- 只能从单台服务器获取同步数据叫做 SNTP（简单网络时间协议，RFC2030）；NTP 可以从多台服务器获得平均时间。
- 答复数据包是自 1900 年 1 月 1日以来以 UTC 秒为单位的 64 位时间戳

NTP 同步子网络：

![ntp_sync_net](https://blog.evernightfireworks.com/static/2019/11/ntp_sync_net.png)

NTP 目标：

- 允许客户端通过网络变得准确地同步时间尽管存在延迟，使用静态技术过滤数据以提升结果质量。
- 提供可靠的服务：
    - 避免长时间的连接中断
    - 冗余路径
    - 冗余服务器
- 允许客户端经常的地同步
    - 通过使用偏移量来调整时钟
- 从干扰中保护：认证数据源

NTP 同步模式（UDP）

- 多播（以太网，低延迟）：服务器周期性在子网中向客户端多播
- 远程过程调用（中等准确率）：
    - 类似克里斯汀算法
    - 服务器向客户端回复真实的时间戳
- 对称模式（symmetric，高准确率）：在时间服务器之间点到点传输
    - ![symmetric_diagram](https://blog.evernightfireworks.com/static/2019/11/symmetric_diagram.png) 
    - ![symmetric_delay](https://blog.evernightfireworks.com/static/2019/11/symmetric_delay.png)
    - ![symmetric_cal](https://blog.evernightfireworks.com/static/2019/11/symmetric_cal.png)
- 提升准确率：
    - 来自单源的数据过滤
        - 保留（Retain）多源的最近对 <o_i,d_i>
        - 过滤器色散（dispersion）：选择对应最小d_j的o_j
    - 对等选择：
        - 同步最小地层（stratum）服务器
        - 最小地层数字，最低同步散射
    - 服务器的地层会动态改变，依赖于同步服务器

SNTP：

- 单播
- 多播，广播结果
- 任播，广播请求，采用第一个回复
