## 秒杀系统设计
- 客户端 90% 静态 HTML+10% 动态 JS；配合 CDN 做好缓存工作。
- 接入层专注于过滤和限流。
- 应用层利用缓存+队列+分布式处理好订单。
- 做好数据的预估，隔离，合并。
- 上线之前记得进行压力测试。

https://mp.weixin.qq.com/s/KWb3POodisbOEsQVblsoGw