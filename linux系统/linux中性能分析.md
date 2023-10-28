linux上性能分析
https://mp.weixin.qq.com/s/OXPDYQmc_3EtptymAfVrhQ
https://www.cnblogs.com/niuben/p/12017242.html
https://mp.weixin.qq.com/s/Rg0w24oW_qHl9ncRBLmfeg

#top命令解释
- 命令: top

##cpu使用率
- CPU 使用率 = 1-CPU 空闲时间 / 总 CPU 时间
- 命令:top
- 字段说明  
https://img2018.cnblogs.com/i-beta/1303876/201912/1303876-20191210152709726-52408463.png
###第一行，任务队列信息，同 uptime 命令的执行结果
系统时间  运行时间  当前登录用户  负载均衡(uptime)load average(后面的三个数分别是1分钟、5分钟、15分钟的负载情况)
load average数据是每隔5秒钟检查一次活跃的进程数，然后按特定算法计算出的数值。如果这个数除以逻辑CPU的数量，结果高于5的时候就表明系统在超负荷运转了)

###第三行，cpu状态信息
- us【user space】— 用户空间占用CPU的百分比。
- sy【sysctl】— 内核空间占用CPU的百分比
- wa【wait】— IO等待占用CPU的百分比
-------
当 us 很高时，说明 CPU 时间主要消耗在用户代码上，可以从用户代码角度考虑优化性能；当 sy 很高时，说明 CPU 时间主要消耗在内核上，可以从是否系统调用频繁、CPU 进程或线程切换频繁角度考虑性能的优化；当 wa 很高时，说明有进程在进行频繁的 IO 操作，可能是磁盘 IO 或者网络 IO。一般情况下，如果 %us+%sy<=70%，我们可以认为系统的运行状态良好。


###第四行，内存信息
- 系统已用内存MemUsed  = MemTotal-MemFree
- 物理已用内存 -/+Used = MemTotal-MemFree-MemBuffers-MemCached
- 系统内存占用率 MemUsed% =（MemUsed/ MemTotal）*100%
- 物理内存占用率-/+Used% = (-/+Used/ MemTotal）*100%

