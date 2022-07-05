1、explain执行计划详解
参考：
https://blog.csdn.net/wuseyukui/article/details/71512793

2、explain案例
https://blog.csdn.net/Dreamhai/article/details/104558854

总结如下：

1、使用Explain关键字可以模拟优化器执行SQL语句，分析查询语句或是结构的性能瓶颈

2、 Explain 字段详解

id
    select查询的序列号，包含一组数字，表示查询中执行select子句或操作表的顺序
	
select_type	
    查询类型
    
tab	
    正在访问哪个表
     
partitions	
    匹配的分区
     
type	
    访问的类型
     
possible_keys	
    显示可能应用在这张表中的索引，一个或多个，但不一定实际使用到
     
key	
    实际使用到的索引，如果为NULL，则没有使用索引

key_len	
    表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度
ref	
    显示索引的哪一列被使用了，如果可能的话，是一个常数，哪些列或常量被用于查找索引列上的值
rows	
    根据表统计信息及索引选用情况，大致估算出找到所需的记录所需读取的行数
filtered	
    查询的表行占表的百分比
Extra	
    包含不适合在其它列中显示但十分重要的额外信息
    
    
    




