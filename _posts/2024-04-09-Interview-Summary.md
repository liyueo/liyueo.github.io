---
layout:     post
title:      面试问题总结
subtitle:   面试总结-20240409
date:       2024-04-09
author:     BY
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - mianshi
    - summary
---


## 1.自我介绍

```
面试官您好，我叫李越，今年24岁，本科毕业于青岛大学，现在是大连理工大学计算机学院的一名研二学生，
我的研究方向是视频异常检测，，在科研之余我自学了c++、计算机网络、操作系统、数据库的相关知识，
并根据网上的教程学习做了Web服务器的项目，本次我想求职一份c++后端开发的岗位，感谢贵公司给了我一次面试的机会，
以上是我的个人基本情况
```

## 2.项目介绍

## 3.项目问题总结

### 1. ET LT 怎么读写

```水平触发
1. 对于读操作
只要缓冲内容不为空，LT模式返回读就绪。

2. 对于写操作
只要缓冲区还不满，LT模式会返回写就绪。

边缘触发

1. 对于读操作
（1）当缓冲区由不可读变为可读的时候，即缓冲区由空变为不空的时候。

（2）当有新数据到达时，即缓冲区中的待读数据变多的时候。

（3）当缓冲区有数据可读，且应用进程对相应的描述符进行EPOLL_CTL_MOD 修改EPOLLIN事件时。

2. 对于写操作
（1）当缓冲区由不可写变为可写时。

（2）当有旧数据被发送走，即缓冲区中的内容变少的时候。

（3）当缓冲区有空间可写，且应用进程对相应的描述符进行EPOLL_CTL_MOD 修改EPOLLOUT事件时。
```

## 4.c++问题总结

### 1.迭代器

```
# 5种迭代器
（1）输入迭代器
（2）输出迭代器
（3）前向迭代器
（4）双向迭代器
（5）随机访问迭代器

| 容器                               | 对应的迭代器类型 |
|------------------------------------|----------------|
| array                              | 随机访问迭代器  |
| vector                             | 随机访问迭代器  |
| deque                              | 随机访问迭代器  |
| list                               | 双向迭代器      |
| set / multiset                     | 双向迭代器      |
| map / multimap                     | 双向迭代器      |
| forward_list                       | 前向迭代器      |
| unordered_map / unordered_multimap | 前向迭代器      |
| unordered_set / unordered_multiset | 前向迭代器      |
| stack                              | 不支持迭代器    |
| queue                              | 不支持迭代器    |

```

## 5.网络问题总结

### 1.socket编程中没有accept，能建立tcp连接

```
可以建立，accept是发生在tcp连接之后，accept系统调用的过程
是从tcp连接队列中取出一个已经建立连接的socket
```

### 2.socket编程中没有listen可以建立tcp连接吗

```
服务端如果只 bind 了 IP 地址和端口，而没有调用 listen 的话，
然后客户端对服务端发起了连接建立，服务端会回 RST 报文。
但是...
tcp可以自己连接自己（tcp自连接）...8两个客户端也可以互相连接..
```

### 3.怎么利用UDP实现可靠连接
```

```

### 4.如何优化tcp
```

```

### 5.拥塞控制的过程
```

```

### 6.HTTP1.0 1.1 2.0 3.0
```

```

### 7.网络协议性能优化
```

```

### 8.DNS缓存时间与什么相关
```

```

### 9.什么是糊涂窗口综合征
> 糊涂窗口综合症就是接收方窗口变小，导致发送方每次发送的数据只有一个大大的头部，真正携带的数据很少。
>
> >可以从接收方和发送方分别进行处理：
>
> 发送方：
>
> •方案 1：只要窗口大小 < 某个值（内核缓冲区大小的一半）的时候，就直接将窗口大小设置为 0，防止发送方发送小数据；然后等到窗口大小 >= 内核缓冲区大小的一半 的时候，才打开窗口，通告发送方，告知其可以发送数据。这样就可以防止发送方发送小报文了
>
> •方案 2（延迟确认应答）：延迟确认应答其实是针对如果接受数据的主机在接收到报文的时候就立刻返回 ACK 应答的话，这时候返回的窗口可能比较小。如果接收方稍微等一会再应答，那么这个时候接收的数据已经被处理完了，从缓冲区中被清理出去了，这样的话，窗口就变大了，发送方能够发送的数据也就更多了
>
> 接收方：
>
> • Nagle 算法：指发送端即使还有应该发送的数据，但如果这部分数据很少的话，则进行延迟发送的一种处理机制。具体来说，当满足已发送的数据都已经收到确认应答时；或者可以发送的数据大小 >= 内核缓冲区大小的一半时，才可以发送数据。这样，从发送方的角度去防止了小报文的发送





## 6. Linux面试问题
### 1. cpu负载和cpu利用率
```
cpu负载 是指 当前系统正在运行的和处于等待运行的进程数之和
cpu利用率是指当前正在运行的进程实时占用CPU的百分比
```
### 2.那如果 CPU 负载很高，利用率却很低该怎么办？
```
CPU负载很高，利用率却很低，说明处于等待状态的任务很多，
通常这种情况是IO密集型的任务，大量请求在请求相同的IO，导致任务队列堆积。

同样，可以先通过top命令观察，假设发现现在确实是高负载低使用率。
top命令显示信息：
第1行是任务队列信息
第2、3行为进程和CPU的信息
第4、5行为内存信息
```
### 3. 那如果负载很低，利用率却很高呢？
```
这表示CPU的任务并不多，但是任务执行的时间很长，
通常是计算密集型任务，生成了大量耗时短的计算任务
```
### 4.常见的Linux命令
<span style="color:#008000">touch：创建文件</span>
<span style="color:#008011">cat：查看文件信息，一次性其全部显示</span>
<span style="color:#008900">more：more和cat有点区别，more用于分屏显示文件内容。</span>
<span style="color:#008670">less：和more类似，less用于分行显示</span>
<span style="color:#006700">tail:一般用户tail -fn 100 xx.log查看最后的100行内容</span>
