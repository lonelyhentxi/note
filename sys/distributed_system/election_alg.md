# 分布式系统 - 选举算法

## 1. 选举 election

- 没有进程需要扮演协调者（coordinator）
- 进程之间无可区分的特征
- 每个进程之间都有可以表示自己的唯一ID

## 2. 霸道（Bully）选举算法

### 2.1 算法：

1. 选择一个最大ID的进程作为协调者
1. 当进程P侦测到一个死协调者时：发送选举消息给所有更高ID的进程
1. 如果没有人响应，P赢了并接管
1. 如果有任何响应，P的工作完成
1. 额外：让所有较低ID的节点知道选举发生了
1. 如果进程收到选举消息：发送OK回去，主持选举（除非已经选举了一个）
1. 一个进程通过向所有进程发送消息告诉他们新的协调者来宣告（announce）胜利（victory)
1. 如果一个死进程恢复，它维持一次选举并发现协调者

### 2.3 示意图：

1. ![bully_1](https://blog.evernightfireworks.com/static/2019/11/bully_1.png)
1. ![bully_2](https://blog.evernightfireworks.com/static/2019/11/bully_2.png)
1. ![bully_3](https://blog.evernightfireworks.com/static/2019/11/bully_3.png)
1. ![bully_4](https://blog.evernightfireworks.com/static/2019/11/bully_4.png)

## 3. 环选举算法

### 3.1 算法：

- 进程的环安排（arrangement）
- 如果任何线程侦测到协调者的失败：
    - 构造携带进程ID的选举消息并发送到下一个线程
    - 如果后继者完蛋了，跳过
    - 重复该过程，直到有个运行中的节点
- 当接收到一条选举信息的时候，进程转发这条消息，并附加上自己的ID
- 最终消息返回到始祖（originator）
    - 进程查看ID列表
    - 流通（circulate），或者多播（multicast）一个协调者消息，宣告协调者（例如，最低数值节点）

### 3.2 示意图：

- ![ring_1](https://blog.evernightfireworks.com/static/2019/11/ring_1.png)
- ![ring_2](https://blog.evernightfireworks.com/static/2019/11/ring_2.png)
- ![ring_3](https://blog.evernightfireworks.com/static/2019/11/ring_3.png)
- ![ring_4](https://blog.evernightfireworks.com/static/2019/11/ring_4.png)
- ![ring_5](https://blog.evernightfireworks.com/static/2019/11/ring_5.png)
- ![ring_6](https://blog.evernightfireworks.com/static/2019/11/ring_6.png)
- $P_2$ 接收倒初始化后的选举信息，并选出一个领袖（例如，序号最小的）
- ![ring_7](https://blog.evernightfireworks.com/static/2019/11/ring_7.png)

## 4. 常（Chang）&罗伯特（Robert）环算法

### 4.1 提升：

优化环

### 4.2 算法：

1. 消息总是包含一个进程ID
1. 避免多次循环选举
1. 如果一个进程发送消息，将其状态标记为参与者（participant）
1. 当接收到消息时：
    - 如果 PID(message) > PID(process)，转发消息，更高的 ID 战胜更低的
    - 如果 PID(message) < PID(process)，消息会被进程 ID 取代
    - 如果 PID(message) < PID(process) 并且进程是参与者，丢弃（discard）这个消息，我们已经传播了 ID
    - 如果相等，该进程已经是领袖，传播自己是领袖的消息

## 5. 网络分裂（Network Partitioning）：脑裂（Split Brain）

### 5.1 网络分部（partitioning/segmentation）

- 脑裂
- 多节点决定领袖

### 5.2 处理分裂：

- 坚持多数（majority），如果没有多数，系统将无法运行
- 依靠替代沟通机制（mechanism）来验证失败

冗余网络，分享磁盘，串行线，SCSI
