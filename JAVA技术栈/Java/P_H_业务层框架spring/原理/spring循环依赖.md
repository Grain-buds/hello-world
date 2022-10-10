一、什么是循环依赖？  
二、容器启动过程中有多少种循环依赖？  
三、容器是如何解决循环依赖的  
- 容器如何解决循环依赖问题？  
- 三级缓存在哪里  




循环依赖类型
A和B的依赖关系有两种
1 set型依赖
2 构造器型依赖
在容器中bean的类型主要分两种
1 单例bean
2 多例bean
按上边的条件一共有四种组合
spring只解决了单例set型依赖，其他3种情况都是通过抛异常来处理的
https://blog.csdn.net/u011523825/article/details/125318118


如何解决循环依赖
https://zwq666666.blog.csdn.net/article/details/125619901
https://louyuting.blog.csdn.net/article/details/77940767
https://blog.csdn.net/weixin_36755535/article/details/123376176

Spring 循环依赖及三级缓存
https://blog.csdn.net/u012098021/article/details/107352463