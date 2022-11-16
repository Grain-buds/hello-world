## Redis Cluster为什么有16384个槽
1、在发送心跳包时使用char进行bitmap压缩后是2k（16384÷8÷1024=2kb），也就是说使用2k的空间创建了16k的槽数
2、如果槽位为65536，发送心跳信息的消息头达8k，发送的心跳包过于庞大，浪费带宽。
3、redis的集群主节点数量基本不可能超过1000个
4、槽位越小，节点少的情况下，压缩率高。Redis主节点的配置信息中，它所负责的哈希槽是通过一张bitmap的形式来保存的，在传输过程中，会对bitmap进行压缩，但是如果bitmap的填充率slots / N很高的话(N表示节点数)，bitmap的压缩率就很低

https://blog.csdn.net/weixin_51095543/article/details/125287604  
redis的集群：
https://blog.csdn.net/truelove12358/article/details/79612954

## 分布式锁
基于数据库的分布式锁
https://blog.csdn.net/qq_45473377/article/details/122986872
https://blog.csdn.net/lovexiaotaozi/article/details/83819916

## redis的分布锁
https://blog.csdn.net/ZYJ95959595/article/details/105527454

## RedLock
https://blog.csdn.net/qq_44833552/article/details/123987308

## jedis的源码总结



## jedis的线程安全
https://blog.csdn.net/weixin_40237626/article/details/117736457
https://leehao.blog.csdn.net/article/details/46830553

## jedis的线程池