## MYSQL FAQ

- 事务
- 索引
- 性能
- 表结构设计
- 数据库基础


## 事务
#### 你们生产环境的 MySQL 中使用了什么事务隔离级别？为什么？

## 索引
#### 为什么 MySQL 索引用的是 B+ 树而不是红黑树？

#### MySQL 三层 B+ 树能存多少数据？



## 性能
#### MySQL 数据库的性能优化方法有哪些？



## 数据库基础

#### 什么是 Write-Ahead Logging (WAL) 技术？它的优点是什么？MySQL 中是否用到了 WAL？


## 表结构设计







为什么阿里巴巴的 Java 手册不推荐使用存储过程？

如何实现数据库的不停服迁移？




MySQL 中 InnoDB 存储引擎与 MyISAM 存储引擎的区别是什么？

MySQL 的查询优化器如何选择执行计划？

什么是数据库的逻辑删除？数据库的物理删除和逻辑删除有什么区别？

什么是数据库的逻辑外键？数据库的物理外键和逻辑外键各有什么优缺点？

MySQL 事务的二阶段提交是什么？

MySQL 三层 B+ 树能存多少数据？

MySQL 在设计表（建表）时需要注意什么？

MySQL 插入一条 SQL 语句，redo log 记录的是什么？

SQL 中 select、from、join、where、group by、having、order by、limit 的执行顺序是什么？

为什么 MySQL 索引用的是 B+ 树而不是红黑树？

MySQL一张表最多可以有多少列？



MySQL 中的 Log Buffer 是什么？它有什么作用？

为什么在 MySQL 中不推荐使用多表 JOIN？

MySQL 中如何解决深度分页的问题？


如何在 MySQL 中监控和优化慢 SQL？

MySQL 中 DELETE、DROP 和 TRUNCATE 的区别是什么？

MySQL 中 INNER JOIN、LEFT JOIN 和 RIGHT JOIN 的区别是什么？

MySQL 中 `LIMIT 100000000, 10` 和 `LIMIT 10` 的执行速度是否相同？

MySQL 中 DATETIME 和 TIMESTAMP 类型的区别是什么？

数据库的三大范式是什么？

在 MySQL 中，你使用过哪些函数？

MySQL 中 TEXT 类型最大可以存储多长的文本？

MySQL 中 AUTO_INCREMENT 列达到最大值时会发生什么？

在 MySQL 中存储金额数据，应该使用什么数据类型？

什么是数据库的视图？

什么是数据库的游标？

为什么不推荐在 MySQL 中直接存储图片、音频、视频等大容量内容？

相比于 Oracle，MySQL 的优势有哪些？

MySQL 中 VARCHAR(100) 和 VARCHAR(10) 的区别是什么？

在什么情况下，不推荐为数据库建立索引？

MySQL 中 EXISTS 和 IN 的区别是什么？



MySQL 中的事务隔离级别有哪些？

MySQL 默认的事务隔离级别是什么？为什么选择这个级别？

数据库的脏读、不可重复读和幻读分别是什么？

MySQL 中有哪些锁类型？

MySQL 的乐观锁和悲观锁是什么？

MySQL 中如果发生死锁应该如何解决？

如何使用 MySQL 的 EXPLAIN 语句进行查询分析？

MySQL 中 count(*)、count(1) 和 count(字段名) 有什么区别？

MySQL 中 int(11) 的 11 表示什么？

MySQL 中 varchar 和 char 有什么区别？

MySQL 中如何进行 SQL 调优？


如何在 MySQL 中避免单点故障？

如何在 MySQL 中实现读写分离？

什么是 MySQL 的主从同步机制？它是如何实现的？

如何处理 MySQL 的主从同步延迟？

什么是分库分表？分库分表有哪些类型（或策略）？

如果组长要求你主导项目中的分库分表，大致的实施流程是？

对数据库进行分库分表可能会引发哪些问题？

从 MySQL 获取数据，是从磁盘读取的吗？（buffer pool）

MySQL 的 Doublewrite Buffer 是什么？它有什么作用？




MySQL 中的数据排序是怎么实现的？

MySQL 的 Change Buffer 是什么？它有什么作用？

详细描述一条 SQL 语句在 MySQL 中的执行过程。

MySQL 的存储引擎有哪些？它们之间有什么区别？

MySQL 的索引类型有哪些？

MySQL InnoDB 引擎中的聚簇索引和非聚簇索引有什么区别？

MySQL 中的回表是什么？

MySQL 索引的最左前缀匹配原则是什么？

MySQL 的覆盖索引是什么？

MySQL 的索引下推是什么？

在 MySQL 中建索引时需要注意哪些事项？

MySQL 中使用索引一定有效吗？如何排查索引效果？


MySQL 中的索引数量是否越多越好？为什么？

请详细描述 MySQL 的 B+ 树中查询数据的全过程	困难

为什么 MySQL 选择使用 B+ 树作为索引结构？

MySQL 是如何实现事务的？	困难

MySQL 中长事务可能会导致哪些问题？

MySQL 中的 MVCC 是什么？	困难

MySQL 二级索引有 MVCC 快照吗？

如果 MySQL 中没有 MVCC，会有什么影响？



MySQL 中的事务隔离级别有哪些？

MySQL 默认的事务隔离级别是什么？为什么选择这个级别？

数据库的脏读、不可重复读和幻读分别是什么？

MySQL 中有哪些锁类型？

MySQL 的乐观锁和悲观锁是什么？

MySQL 中如果发生死锁应该如何解决？

如何使用 MySQL 的 EXPLAIN 语句进行查询分析？

MySQL 中 count(*)、count(1) 和 count(字段名) 有什么区别？

MySQL 中 int(11) 的 11 表示什么？

MySQL 中 varchar 和 char 有什么区别？

MySQL 中如何进行 SQL 调优？


如何在 MySQL 中避免单点故障？

如何在 MySQL 中实现读写分离？

什么是 MySQL 的主从同步机制？它是如何实现的？

如何处理 MySQL 的主从同步延迟？

什么是分库分表？分库分表有哪些类型（或策略）？

如果组长要求你主导项目中的分库分表，大致的实施流程是？

对数据库进行分库分表可能会引发哪些问题？

从 MySQL 获取数据，是从磁盘读取的吗？（buffer pool）

MySQL 的 Doublewrite Buffer 是什么？它有什么作用？


