#linux java 安装
1、jdk需要放置的安装目录  
2、解压命令 tar zxvf   
3、配置环境变量  
```shell script
export JAVA_HOME=/home/jdk/jdk1.8.0_321
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=.:${JAVA_HOME}/bin:$PATH
```
4、source /etc/profile  
5、编辑 ~/.bashrc文件  
vim ~/.bashrc  
```shell script
if [ -f /etc/profile ]; then
        . /etc/profile
fi
```

#zookeeper
1、下载  
https://archive.apache.org/dist/zookeeper/   
2、解压安装  
tar -zxvf apache-zookeeper-3.8.0-bin.tar.gz  
ln -s apache-zookeeper-3.8.0-bin zookeeper  
cd /home/hyw/zookeeper/conf  
3、修改配置  
cp zoo_sample.cfg zoo.cfg  
vi  zoo.cfg  
```shell
#clientPort：这个端口就是客户端连接ZooKeeper服务器的端口，ZooKeeper会监听这个端口，接收客户端的请求
tickTime=2000 
dataDir=/home/hyw/zookeeper/tmp
clientPort=2181
```
4、修改环境变量  
vi /etc/profile  
```shell script
export JAVA_HOME=/usr/local/jdk1.8.0_221
export ZOOKEEPER_HOME=/home/hyw/zookeeper
export PATH=$JAVA_HOME/bin:$ZOOKEEPER_HOME/bin:$PATH
```
5、source /etc/profile  
6、启动zookeeper与查询  
cd /home/hyw/zookeeper/bin  
zkServer.sh start  
zkServer.sh status  

FAQ:
1、zookeeper启动报错org.apache.zookeeper.server.quorum.QuorumPeerMain
https://blog.csdn.net/fattershall/article/details/102842407


参考：  
https://blog.csdn.net/m0_46516834/article/details/114482482

#kafka
1、下载
http://archive.apache.org/dist/kafka/  
http://mirrors.aliyun.com/apache/kafka/2.8.1/?spm=a2c6h.25603864.0.0.3c7d126eerCYiT  

2、解压安装
tar -zxvf kafka_2.12-2.8.1.tgz  
ln -s kafka-2.8.0-src kafka_0  


3、修改环境变量
```shell script
export KAFKA_HOME=/home/hyw/kafka_0
export PATH=$JAVA_HOME/bin:$ZOOKEEPER_HOME/bin:$KAFKA_HOME/bin:$PATH
```
4、source /etc/profile  
5、修改配置 
mkdir kafka-logs logs  
6、修改属性 
vi config/server.properties
```shell script
log.dirs=/home/hyw/kafka_0/kafka-logs
zookeeper.connect=192.168.2.150:2181
listeners=PLAINTEXT://192.168.2.150:9092

```
kafka服务端口号，默认 9092  
单机：config/connect-standalone.properties-->
bootstrap.servers=localhost:9093  
集群：config/connect-distributed.properties-->
bootstrap.servers=localhost:9093  

//1.52.139.238:9092

https://blog.csdn.net/lizz861109/article/details/109093852

7、启动与停止  
启动：./bin/kafka-server-start.sh config/server.properties &  
停止：./bin/kafka-server-stop.sh config/server.properties &


8、常见命令
cd  kafka_0  
创建topic 
```shell script
./bin/kafka-topics.sh --create \
    --zookeeper 192.168.2.150:2181 \
    --replication-factor 1 \
    --partitions  1 \
    --topic kafkamonitor-simpleproducer
```

```sql
./bin/kafka-console-consumer.sh --bootstrap-server 192.168.2.150:9092 --topic kafkamonitor-simpleproducer --from-beginning
```


查询topic 
```sql
./bin/kafka-topics.sh --list --zookeeper 192.168.2.150:2181
```





https://blog.51cto.com/qiangsh/1746253  


#Kafaka 管理工具  ---有些版本不兼容
https://blog.csdn.net/qq_26838315/article/details/106882670?spm=1001.2101.3001.6650.4&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-4-106882670-blog-124859114.t5_download_50w&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-4-106882670-blog-124859114.t5_download_50w&utm_relevant_index=9

https://blog.csdn.net/weixin_43283487/article/details/106737765  
https://blog.csdn.net/yujia_666/article/details/108249009



##kafka-offset-monitor
下载： 
**https://github.com/Morningstar/kafka-offset-monitor/releases/tag/0.4.6**

1、 mkdir kafka-monitor  
2、上传包  
KafkaOffsetMonitor-assembly-0.4.6-SNAPSHOT.jar  
3、修改执行权限  
chmod 777 KafkaOffsetMonitor-assembly-0.4.6-SNAPSHOT.jar  
4、启动脚本  
vi start-monitor.sh  
chmod 777 start-monitor.sh  

```shell script
#!/bin/bash
#Java -cp 依赖jar或者是依赖jar库 测试类的全限定名
#解释下这条启动命令的含义，首先我们需要指明运行Web监控的类，然后需要用到ZooKeeper，所有要填写ZK集群信息，接着是Web运行端口，页面数据刷新的时间以及保留数据的时间值。
nohup java -cp KafkaOffsetMonitor-assembly-0.4.6-SNAPSHOT.jar com.quantifind.kafka.offsetapp.OffsetGetterWeb  --kafkaBrokers 192.168.2.150:9092 --zk 192.168.2.150:2181 --port 7788 --refresh 10.seconds --retain 3.days 1>mobile-logs/stdout.log 2>mobile-logs/stderr.log & 

exit 0
```
5、启动命令
./start-monitor.sh

6、界面访问http://192.168.2.150:7788 

FAQ:
解决KafkaOffsetMonitor页面不展示问题
http://www.manongjc.com/detail/51-rikqmbrmawptmej.html
修改kafka-offset-monitor/src/main/resources/offsetapp/index.html 修改三个angular地址
```sql
<script src="//cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
<script src="//cdn.static.runoob.com/libs/angular.js/1.4.6/angular-route.js"></script>
 <script src="//cdn.static.runoob.com/libs/angular.js/1.4.6/angular-resource.js"></script>
```
https://blog.csdn.net/u014635374/article/details/106223025/

##Kafka Manager
https://github.com/yahoo/CMAK/tree/1.3.3.22
https://blog.csdn.net/man_earth/article/details/87791306



##kafka-eagle-bin
https://github.com/smartloli/kafka-eagle-bin/

命令：
tar -zxf kafka-eagle-bin-1.2.9.tar.gz
cd kafka-eagle-bin-1.4.8
tar -zxf kafka-eagle-web-1.2.9-bin.tar.gz
vi /etc/profile
```sql
export KE_HOME=/home/hyw/kafka-eagle-bin-1.2.9/kafka-eagle-web-1.2.9
export PATH=$JAVA_HOME/bin:$KE_HOME/bin:$PATH
export PATH=$JAVA_HOME/bin:$ZOOKEEPER_HOME/bin:$KAFKA_HOME/bin:$KE_HOME/bin:$PATH
```
进入kafka-eagle的conf目录下修改配置文件 system-config.properties 
```sql
kafka.eagle.zk.cluster.alias=cluster1
cluster1.zk.list=192.168.2.150:2181
kafka.eagle.webui.port=8048
kafka.eagle.url=jdbc:sqlite:/home/hyw/kafka-eagle-bin-1.2.9/kafka-eagle-web-1.2.9/db/ke.db
```

chmod  777 ke.sh  

nohup ke.sh start out.txt 2>&1  
ke.sh [start|status|stop|restart|stats]
访问 http://192.168.2.150:8048/ke  




