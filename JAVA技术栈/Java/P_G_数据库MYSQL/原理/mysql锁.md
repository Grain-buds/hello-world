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
1、select * from t1 where id = 10;

结论:
- 前8种组合下，select操作均不加锁，采用的是快照读
- 组合9,select * from t1 where id = 10; 这条SQL，在在Serializable隔离级别，SQL1会加读锁，也就是说快照读不复存在，MVCC并发控制降级为Lock-Based CC

2、delete from t1 where id = 10;
结论:
- 组合1，加x锁。id是主键时，此SQL只需要在id=10这条记录上加X锁即可
- 组合2，加两个X锁。若id列是unique列，其上有unique索引。那么SQL需要加两个X锁，一个对应于id unique索引上的id = 10的记录，另一把锁对应于聚簇索引上的[name='d',id=10]的记录
- 组合3，加两个X锁。若id列上有非唯一索引，那么对应的所有满足SQL查询条件的记录，都会被加x锁。同时，这些记录在主键索引上的记录，也会被加x锁
- 组合4，加表锁，全扫描。若id列上没有索引，SQL会走聚簇索引的全扫描进行过滤，由于过滤是由MySQL Server层面进行的。因此每条记录，无论是否满足条件，都会被加上X锁。但是，为了效率考量，MySQL做了优化，对于不满足条件的记录，会在判断后放锁，最终持有的，是满足条件的记录上的锁，但是不满足条件的记录上的加锁/放锁动作不会省略。同时，优化也违背了2PL的约束
- 组合5，delete from t1 where id = 10; 这条SQL，加锁与组合1一样
- 组合6，与组合2一样
- 组合7，加x锁和GAP锁。Repeatable Read隔离级别下，id列上有一个非唯一索引，对应SQL：delete from t1 where id = 10; 首先，通过id索引定位到第一条满足查询条件的记录，加记录上的X锁，加GAP上的GAP锁，然后加主键聚簇索引上的记录X锁，然后返回；然后读取下一条，重复进行。直至进行到第一条不满足条件的记录[11,f]，此时，不需要加记录X锁，但是仍旧需要加GAP锁，最后返回结束。
- 组合8，加表锁，全扫描。在Repeatable Read隔离级别下，如果进行全表扫描的当前读，那么会锁上表中的所有记录，同时会锁上聚簇索引内的所有GAP，杜绝所有的并发 更新/删除/插入 操作。当然，也可以通过触发semi-consistent read，来缓解加锁开销与并发影响，但是semi-consistent read本身也会带来其他问题，不建议使用。
- 组合9，和组合8一样。

