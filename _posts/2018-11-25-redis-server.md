---
layout: default-blog-md
title: redis流程简述
category: markdown
---

## 代码版：
#### initServer

- aeCreateEventLoop
  - aeApiCreate
    - kqueue
- aeCreateTimeEvent
- aeCreateFileEvent
  - aeApiAddEvent(acceptTcpHandler)
    - kevent

#### aeMain

- beforeSleep
  - WriteToClient(write)
- aeProcessEvents
  - aeApiPoll
    - kevent
  - aeFileEvent *fe
  - fe->rfileProc
  - fe->wfileProc
  - processTimeEvents

#### acceptTcpHandler

- anetTcpAccept(accept)
- acceptCommonHandler
  - createClient
    - aeCreateFileEvent(readQueryFromClient)

#### readQueryFromClient

- read
- processInputBuffer
- processCommand
  - lookupCommand(redisCommandTable)
  - call
    - addReply
    - memcpy

## 注释版：
#### 初始化

- 创建异步网络IO
- 创建定时器事件
- 创建描述符事件

#### 主循环

- 进入阻塞等待之前，调用write写输出到客户端
- 处理被唤醒的事件
  - 调用注册的事件处理函数
  - 处理定时器时间

#### tcp处理函数

- 调用accept接收客户端请求
- 注册客户端请求处理函数readQueryFromClient

#### readQueryFromClient

- 调用read读取客户端请求
- 解析报文格式
- 处理客户端命令
  - 从redisCommandTable里查找对应命令的处理函数
  - 调用处理函数，把回应报文拷贝到输出缓冲区里