事务传播特性和事务隔离级别:
https://blog.csdn.net/jjavaboy/article/details/45243565

1、5种隔离级别，7种传播行为，后来才发现这是针对Spring的。  

2、对数据库来说隔离级别只有4种，Spring多了一个DEFAULT 这是一个PlatfromTransactionManager默认的隔离级别，使用数据库默认的事务隔离级别.

3、spring的事务是对数据库的事务的封装,最后本质的实现还是在数据库,假如数据库不支持事务的话,spring的事务是没有作用的  

4、数据库的事务说简单就只有开启,回滚和关闭,spring对数据库事务的包装。
原理就是拿一个数据连接,根据spring的事务配置,操作这个数据连接对数据库进行事务开启,回滚或关闭操作.
但是spring除了实现这些,还配合spring的传播行为对事务进行了更广泛的管理.其实这里还有个重要的点,
那就是事务中涉及的隔离级别,以及spring如何对数据库的隔离级别进行封装.事务与隔离级别放在一起理解会更好些.
原文链接：https://blog.csdn.net/weixin_39389888/article/details/95619097




https://zhuanlan.zhihu.com/p/128980187
https://www.cnblogs.com/yadongliang/p/9382173.html
https://blog.csdn.net/lgb105/article/details/80461556
