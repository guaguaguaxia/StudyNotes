- TCP 协议是基于字节流的传输层协议，其中不存在消息和数据包的概念；
- 应用层协议没有使用基于长度或者基于终结符的消息边界，导致多个消息的粘连；

> 当应用层协议使用 TCP 协议传输数据时，TCP 协议可能会将应用层发送的数据分成多个包依次发送，而数据的接收方收到的数据段可能有多个『应用层数据包』组成，所以当应用层从 TCP 缓冲区中读取数据时发现粘连的数据包时，需要对收到的数据进行拆分。