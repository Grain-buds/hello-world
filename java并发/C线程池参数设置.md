https://www.cnblogs.com/waytobestcoder/p/5323130.html


https://blog.csdn.net/sinat_34976604/article/details/88125707






线程池的饱和策略:

1、核心业务，不允许失败，AbortPolicy：直接抛出异常，拒绝执行。（慎用默认 AbortPolicy）
2、任务重要，不能丢弃，CallerRunsPolicy，调用者承担压力。
3、日志/监控/非关键任务，DiscardPolicy，静默丢弃，不影响主流程
4、实时性强，新任务更重，DiscardOldestPolicy，丢弃旧任务，保留新任务
5、需要异步重试或持久化，自定义策略

监控：饱和策略、监控拒绝次数；监控活跃线程数、队列长度、拒绝次数等指标。