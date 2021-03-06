# Libevent

Libevent 是一个用C语言编写的、轻量级的开源高性能事件通知库（网络库），是一个高并发高性能的事件处理框架

- 是一个事件驱动（ event-driven），高性能（事件驱动—回调机制）;

- 事件驱动（ event-driven），高性能（事件驱动—回调机制）;
- 轻量级，专注于网络
- 源代码相当精炼、易读；跨平台，支持 Windows、 Linux、 *BSD 和 Mac Os；
- 支持多种 I/O 多路复用技术， epoll、 poll、 dev/poll、 select 和 kqueue 等；
- 支持 I/O，定时器和信号等事件；注册事件优先级





## 高性能高并发

- 要实现高性能，必须保证“non-blocking IO + IO multiplexing”结合
- 单线程server没有线程切换和加锁开销
- 劣势是不能充分利用CPU多核优势（可以通过多进程解决）





## 适用场景

编写server时，想要避免线程切换和加锁开销获得高性能高并发的server，可以采用事件驱动的Libevent搭建单线程server



## 使用流程

- 创建事件处理框架event_base
  -  每个event_base结构体有一个事件集合,可以检测以确定哪个事件是激活的，epoll基于红黑树实现，相当于epoll红黑树的根
  - 每个event_base都有一种用于检测那种事件已经就绪的"方法",或者说后端
- 创建事件
- 事件添加到处理框架
- 开始事件循环
- 释放资源



## Libcurl

- libevent主要实现服务器，包含了select、epoll等高并发的实现
- libcurl实现了curl命令的API封装，主要作为客户端
- *cURL*是一个利用URL语法在命令行下工作的文件传输工具



## 项目中适用

## 

项目中主要适用Libevent+Libcurl搭建了https服务器，一个应用服务器，一个存储服务器









## HTTPS

- HTTPS： 采用 对称加密 和 非对称加密 结合的方式来保护浏览器和服务端之间的通信安全
  非对称加密算法交换密钥 +数字证书验证身份+对称加密算法加密数据![image-20200708125321967](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200708125321967.png)

![img](https://img2018.cnblogs.com/blog/1249620/201811/1249620-20181108111555375-1370286956.png)

非对称加密过程

- 客户端尝试建立安全连接，服务器返回CA证书和服务器公钥
- 客户端验证证书和公钥有效性，有效则生成对称一对客户端公私钥，并用服务端公钥将客户端公私钥加密发送到服务端
- 服务端适用服务端私钥解密拿到客户端公私钥，并生成一个服务端的会话密钥并用客户端公钥加密发送给客户端
- 客户端用客户端公钥解密拿到服务端会话密钥
- 至此SSL加密建立

对称加密过程

- 客户端用客户端公钥加密请求数据发送给客户端
- 服务端用客户端私钥解密后再用服务端会话密钥加密回复数据返回客户端
- 客户端用服务器会话密钥解密