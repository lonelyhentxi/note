套接字通讯：Socket Communication，直接通讯

## 1. 客户端和服务器：

- 一个分布式系统可以被泛用的建模为客户端和服务器端。
- 客户端和服务器端通常位于不同的设备上，通过消息传输来交互。
- 为提供服务，服务器必须得到传输层地址 - 位置是良定义的。（方法：监听端口号）

## 2. 传输地址：

- 服务器需要传输地址，客户端才能和其沟通：
    - IP address 可以辨识机器
    - 传输层使用端口号来辨识进程
        - `machine address` -> `building address`
        - `transport address` -> `apartment number`
- 客户端包含地址通过：
    - 硬编码
    - 数据库（`/etc/services`, directory server）

## 3. 传输层协议：

- 传输层支持在进程之间沟通
- 两种协议：
    - 面向连接的
    - 无连接协议

## 4. 面向连接协议：

**过程**：

1. 建立连接
1. 协商（negotiate）协议
1. 交换数据
1. 中断（terminate）连接

**虚拟电路（stream）服务**

1. 提供专用（dedicated）电路的错觉（illusion）
1. 消息到达顺序的保证
1. 应用消息使用连接ID，而不是地址。

## 5. 无连接协议：

**过程**：

- 没有呼叫的启动
- 发送/接收信息（每个包都被标址了）
- 不终止

**数据报服务**：

- 终端不确定消息被接收
- 服务器和客户端不维护状态
- 相较于虚电路服务更廉价但是不可靠

## 6. 套接字（Sockets）：传输层交流

对传输层沟通的流行抽象：有伯克利在1982年开发的

**目标**：

- 在进程之间的交流不应该取决于是否在同一台设备上：统一所以的数据交换为文件访问
- 应用可以选择某种方式来交流：虚电路、数据报、基于消息的、有序传送
- 支持多种协议和命名惯例。

## 7. 编码操作：

### 7.1 客户端：

1. 创建套接字
1. 构造服务器地址
1. 连接服务器地址
1. 沟通
1. 关闭连接

### 7.2 服务端：

1. 创建套接字
1. 绑定套接字到地址上
1. 设置端口为监听模式
1. 接收连接
1. 通信
1. 关闭连接

### 7.3 操作：

- `socket`
- `bind`
- `listen`, `accept`, `connect`
- `read/write`, `send/recv`, `sento/recvfrom`, `sendmsg/recvmsg`
- `close/shutdown`

### 7.3.1 详情：

- 创建套接字
    - 是系统调用
        - `int s = socket(domain, type, protocol)`
    - 参数：
        - `domain` 标识地址组
            - `AF_INET`: IPC on the Internet
            - `AF_UNIX`: IPC within a computer
            - `AF_NS`: IPC on Xeroxs Network Systems
        - `type` 应用需要服务类型
            - `SOCK_STREAM` 虚电路
            - `SOCK_DGRAM` 数据报
            - `SOCK_RAW` 原始IP访问
        - `protocol` 声明协议，为支持用户自定义的协议，默认值为0
    - 返回：
        - 返回 `socket` 的文件描述符代号
- 绑定 `socket` 到地址：
    - `bind` system call:
        - `int error = bind(s,addr,addrlen);`
    - 参数：
        - `s`: 端口号的描述符
        - `addr`： 地址的结构体（`struct sockaddr *`）
        - `addrlen`: 地址结构体的长度
    - 返回：
        - 错误代码
    - 用例：
        - 绑定文件名到 `UNIX` 套接字上：
            - 
            - 
        - 绑定网络地址：
            - 
            - 
- 服务器：设置套接字来监听：
    - `listen` system call
        - `int error = listen(s, backlog);`
    - 参数：
        - `s`： socket的文件描述符
        - `backlog`：待定连接的队列长度
    - 返回：
        - 错误码
- 服务器：接收连接
    - `accept` system call
        - `int snew = accept(s, clntaddr, addrlen);`
    - 参数：
        - `s`：套接字文件描述符
        - `clntaddr`：包含客户端 `id` 的 `sockaddr` 结构体指针
        - `addrlen`：包含客户端地址结构体长度的int的指针
    - 返回：
        - 一个新的套接字用来本次沟通会话
- 客户端：连接
    - `connect` system call：
        - `int error = connect(s, svraddr, addrlen);`
    - 参数：
        - `s`：连接的文件描述符
        - `svraddr`：包含服务器地址的sockaddr结构体指针
        - `addrlen`：服务器地址结构体长度
    - 返回值：
        - 错误码
- 交换数据和关闭套接字：
    - 通过套接字交换数据和文件访问一样
        - `read(s,buf,length);`
        - `write(s,buf,length);`
    - 关闭套接字于：
        - 系统调用：
            - `close(s);`
            - 或者：`shutdown(int s, int how);`
        - 参数：
            - `s`： 套接字描述符
            - `how`：
                - `0`：进一步接收不允许
                - `1`：进一步发送不允许
                - `2`：进一步发送和接收都不允许

### 7.3.2 服务端和客户端的同步：

![]()

### 7.3.3 套接字操作的数据结构和实现：

**数据结构**：

- `Client only sends data to {machine,port};`
- 如何跟踪对同一服务器进程的同时会话
- 系统维护一个叫做 `Protocol Control Block` 的结构：每个 `PCB` 条目包含：
    - 本地地址
    - 本地端口
    - 远程地址
    - 远程端口
    - 套接字监听标记
    - 对套接字文件描述符的引用

**实现**：

- `socket()`
    - 作用：在 `PCB` 表中申请新的空条目
    - 
- `bind()`
    - 作用：给本地地址和端口赋值
    - 
- `listen()`
    - 作用：设置 `socket` 接收连接
    - 
- `connect()`
    - 作用：发送连接请求给服务器
    - 
- `accept()`
    - 作用：发送确认给客户端
    - 
- 通过套接字的消息交换：
    - 每条来自客户端的消息被标记为 `data` 或者 `control mesg(connect)`
    - 如果是 `data`，搜索 `PCB` 表中目的地址和目的端口符合收到的消息并且 `listen` 没有被设置的
    - 如果是 `control`，搜索表中本地端口符合目的端口并且listen被设置的

### 7.3.4 数据报套接字：

**操作**： 

`create`，`bind`，`close`，同样的流操作，但是不监听、接收和连接

**定义**：

- `sendto/recvfrom`
    - `int sendto(int s, void *msg, int len, int flags, struct sockaddr *to,int tolen);`
    - `int recvfrom(int s, void *buf, int len, int flags, struct sock);`
- `sendmsg/recvmsg`
    - `int sendmsg(int s,struct msghdr *msg, int flags);`
    - `int recvmsg(int s,struct msghdr *msg, int flags);`
