### 服务端准备连接的过程
- 创建套接字
  - int socket(int domain, int type, int protocol)
  - domain 就是指 PF_INET、PF_INET6 以及 PF_LOCAL 等，表示什么样的套接字。
  - type 可用的值是：SOCK_STREAM: 表示的是字节流，对应 TCP；SOCK_DGRAM： 表示的是数据报，对应 UDP；SOCK_RAW: 表示的是原始套接字。

- bind: 设定电话号码
  - bind(int fd, sockaddr * addr, socklen_t len)

- listen：接上电话线，一切准备就绪
  - int listen (int socketfd, int backlog)
- accept: 电话铃响起了……
  - int accept(int listensockfd, struct sockaddr *cliaddr, socklen_t *addrlen)

### 客户端发起连接的过程
 ![41](./image/41.png)
>  本质是信道不可靠, 通信双方需要就某个问题达成一致. 而要解决这个问题, 无论你在消息中包含什么信息, 三次通信是理论上的最小值. 所以三次握手不是TCP本身的要求, 而是为了满足「在不可靠信道上可靠地传输信息」这一需求所导致的