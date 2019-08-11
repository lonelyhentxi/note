# 分布式系统 - 远程过程调用

## 1. 概述

历史：

- 1984 Birrell & Nelson
- 调用其他机器上过程的机制（Mechanism）
- 位于机器A上的进程可以调用位于机器B上的过程：
    1. A被暂停
    2. 执行在B上继续
    3. 当B返回时，控制在A上继续

目标：使得远程过程调用看起来和本地程序调用相似

## 2. RPC的实现

### 2.1 概要：

1. RPC编译器自动生成存根(stub)/骨骼程序（skeleton routines）来使得一个远程过程调用到用户就像本地一样
2. ![rpc_impl](https://blog.evernightfireworks.com/static/2019/11/rpc_impl.png)
3. 编组和解编参数和返回值
4. 在不同的机器上处理不同的数据格式
5. 侦测和处理服务器和客户端过程中和网络的失败

### 2.2 案例：

SUN RPC中的编译：

![rpc_compile](https://blog.evernightfireworks.com/static/2019/11/rpc_compile.png)

Demo of RPC

![demo_of_rpc](https://blog.evernightfireworks.com/static/2019/11/demo_of_rpc.png)

### 2.3 RPC的流程

1. 客户端呼叫存根（向栈中压入参数）
![rpc_proc_1](https://blog.evernightfireworks.com/static/2019/11/rpc_proc_1.png)
2. Clnt_stub 编组（marshals）参数到消息中，并进行一个系统调用（客户端阻塞）
![rpc_proc_2](https://blog.evernightfireworks.com/static/2019/11/rpc_proc_2.png)
3. 网络传输到服务器
![rpc_proc_3](https://blog.evernightfireworks.com/static/2019/11/rpc_proc_3.png)
4. 传送到服务器的存根，服务器恢复
 ![rpc_proc_4](https://blog.evernightfireworks.com/static/2019/11/rpc_proc_4.png)
5. 服务器存根解编码参数并调用服务器过程（local call）
![rpc_proc_5](https://blog.evernightfireworks.com/static/2019/11/rpc_proc_5.png)
6. 返回存根
![rpc_proc_6](https://blog.evernightfireworks.com/static/2019/11/rpc_proc_6.png)

### 2.4 编写程序：

程序需要被写为两部分

- 客户端程序
    - 声明服务器位置
    - 参数和返回值都是指针
- 服务器例程
    - 生成相同的本地过程，除了参数和参数类型
