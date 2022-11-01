常见 SQL 语句的加锁分析
https://blog.csdn.net/u012501054/article/details/95325745

mysql加锁处理分析
https://github.com/hedengcheng/tech/blob/master/database/MySQL/MySQL%20%E5%8A%A0%E9%94%81%E5%A4%84%E7%90%86%E5%88%86%E6%9E%90.pdf

https://blog.csdn.net/m1ssyAn/article/details/114672602

一条SQL加锁，可以分9种情况：
组合一：id列是主键，RC隔离级别
组合二：id列是二级唯一索引，RC隔离级别
组合三：id列是二级非唯一索引，RC隔离级别
组合四：id列上没有索引，RC隔离级别
组合五：id列是主键，RR隔离级别
组合六：id列是二级唯一索引，RR隔离级别
组合七：id列是二级非唯一索引，RR隔离级别
组合八：id列上没有索引，RR隔离级别
组合九：Serializable隔离级别

举例：
- select * from t1 where id = 10;