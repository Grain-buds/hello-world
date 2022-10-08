**1、为什么Mysql底层采用B+树做索引？**  
总结各种树解决的问题以及面临的新问题：  
二叉查找树(BST)：解决了排序的基本问题，但是由于无法保证平衡，可能退化为链表；  
平衡二叉树(AVL)：通过旋转解决了平衡的问题，但是旋转操作效率太低；  
红黑树：通过舍弃严格的平衡和引入红黑节点，解决了AVL旋转效率过低的问题，但是在磁盘等场景下，树仍然太高，IO次数太多；  
B树：通过将二叉树改为多路平衡查找树，解决了树过高的问题；  
B+树：在B树的基础上，将非叶节点改造为不存储数据的纯索引节点，进一步降低了树的高度；此外将叶节点使用指针连接成链表，范围查询更加高效。    
https://blog.csdn.net/qq_37933128/article/details/127179820  

**3.MySql的存储引擎有哪些?**
支持事务的引擎： InnoDB、BDB
不支持事务的引擎： MyISAM、MEMORY、MERGE、EXAMPLE、NDB Cluster、 ARCHIVE、CSV、BLACKHOLE、FEDERATED。

1.Myisam是Mysql的默认存储引擎，当create创建新表时，未指定新表的存储引擎时，默认使用Myisam。每个MyISAM 在磁盘上存储成三个文件。文件名都和表名相同，扩展名分别是 .frm (存储表定义) 、.MYD (MYData，存储数据)、.MYI (MYIndex，存储索引)。数据文件和索引文件可以放置在不同的目录，平均分布io，获得更快的速度。

2.InnoDB 存储引擎提供了具有提交、回滚和崩溃恢复能力的事务安全。但是对比 Myisam 的存储引擎，InnoDB 写的处理效率差一些并且会占用更多的磁盘空间以保留数据和索引。

3、ENGINE=xxx 设置引擎。

**6.选择合适的存储引擎？**

选择标准: 根据应用特点选择合适的存储引擎,对于复杂的应用系统可以根据实际情况选择 多种存储引擎进行组合. 下面是常用存储引擎的适用环境:

1. MyISAM: 默认的 MySQL 插件式存储引擎, 它是在 Web、 数据仓储和其他应用环境下最常使用的存储引擎之一。
2. InnoDB:用于事务处理应用程序，具有众多特性，包括 ACID 事务支持。
3. Memory: 将 所有数据保存在RAM 中， 在 需要快速查找引用和其他类似数据的环境下，可 提供极快的访问。
4. Merge:允许 MySQL DBA 或开发人员将一系列等同的 MyISAM 表以逻辑方式组合在一起,并作为 1 个对象引用它们。对于诸如数据仓储等 VLDB 环境十分适合。


7、InnoDB 存储引擎下char & varchar
建议使用 VARCHAR类型,对于InnoDB数据表，内部的行存储格式没有区分固定长度和可变长度列(所有数据行 都使用指向数据列值的头指针) ，因此在本质上，使用固定长度的CHAR列不一定比使 用可变长度VARCHAR列简单。 因而， 主要的性能因素是数据行使用的存储总量。 由于 CHAR 平均占用的空间多于VARCHAR，因此使用VARCHAR来最小化需要处理的数据行的存储总 量和磁盘I/O是比较好的

8、Mysql字符集
mysql服务器可以支持多种字符集 (可以用show character set命令查看所有mysql支持 的字符集) ，在同一台服务器、同一个数据库、甚至同一个表的不同字段都可以指定使用不 同的字符集。 
mysql的字符集包括字符集(CHARACTER)和校对规则(COLLATION)两个概念。  
保存汉字的字符集可以:utf8、gb2312、gbk，不能做出肯定答复的话最好选用 gbk。  
保存网络表情包：utf8utf8mb4  

9、索引类型是什么?设计原则？MySql有哪些索引？
联合索引
https://zhuanlan.zhihu.com/p/453658511
https://blog.csdn.net/weixin_44162368/article/details/113243686
https://blog.csdn.net/chuixue24/article/details/120266994
https://blog.csdn.net/qq_34486648/article/details/123096102