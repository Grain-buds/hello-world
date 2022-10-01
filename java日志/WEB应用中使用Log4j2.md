官网log4j2:
http://logging.apache.org/log4j/2.x/

一、log4j2支持的配置文件格式
log4j2支持XML、JSON、YAML和properties四种配置文件格式，当然官网上还介绍了通过代码来进行配置的方法，这里不重点介绍。

log4j2最方便的在于自动加载配置，log4j2自动加载配置的优先级如下
```text
1. log4j2在程序启动时会检查系统配置项“log4j2.configurationFile”，如果此配置项被设置，则优先采用此配置项指定的配置文件去解析加载；
2. 如果1中的系统配置项没有被指定，则按下面优先级顺序加载。首先寻找classpath下的log4j2-test.properties文件
3. 寻找classpath下的 log4j2-test.yaml 或者 log4j2-test.yml 文件
4. 寻找classpath下的 log4j2-test.json 或者 log4j2-test.jsn文件
5. 寻找classpath下的 log4j2-test.xml 文件
6. 如果上述test文件没有找到，则尝试寻找classpath下的 log4j2.yaml 或者 log4j2.yml 文件
7. 寻找classpath下的 log4j2.json或者 log4j2.jsn文件
8. 寻找classpath下的 log4j2.xml 文件
9. 如果上述配置文件都无法找到，则日志直接输出到控制台
```


log4j2.xml配置项详解
```text
<!--表明下面时log4j的配置项-->
<Configuration>
	<!--日志输出的方式-->
	<Appenders>
		<Appender>
		</Appender>
	</Appenders>
	<!--指定日志，每个logger与Appender关联-->
	<Loggers>
		<Logger>
		</Logger>
	</Loggers>
</Configuration>
```

Configuration log4j2的配置项
```text
name 指定log4j2的配置项的名字
status 指定log4j2自身的打印日志的级别
monitorInterval 指定log4j2自动重新配置的监测时间间隔，单位是s，最小是5s
Appenders 指定日志输出方式的列表，以appender等指定一个日志的输出方式，目前支持的Appender主要有Console、File、RollingFile、Async、Routing等
```

Console 将日志打印到控制台:

- name：指定Console的名称
- target：SYSTEM_OUT或SYSTEM_ERR，一般只设置默认SYSTEM_OUT
- PatternLayout pattern指定输出格式，不设置默认为:%m%n，下面介绍pattern的格式:
 
```text
%d{yyyy-MM-dd HH:mm:ss.SSS} %-5level %class{36} %L %M -- %msg%xEx%n

%d{yyyy-MM-dd HH:mm:ss, SSS} : 日志生产时间
%p : 日志输出格式
%c : logger的名称
%m : 日志内容，即 logger.info("message")
%n : 换行符
%C : Java类名
%L : 日志输出所在行数
%M : 日志输出所在方法名
hostName : 本地机器名
hostAddress : 本地ip地址
```   

File 将日志打印到文件
- name 指定Appender的名字
- filename 指定输出日志的目的文件带全路径的文件名
- PatternLayout pattern指定输出格式，不设置默认为:%m%n。详细设置参考上面。

RollingFile 将日志打印到文件，文件可以回滚保存

- name 指定Appender的名字
- filename 指定输出日志的目的文件带全路径的文件名
- filepattern 指定新建日志文件的名称格式
- filePermissions 指定日志文件权限
- PatternLayout pattern指定输出格式，不设置默认为:%m%n。详细设置参考上面。
- Policies 指定滚动日志的策略，就是什么时候进行新建日志文件输出日志
  - TimeBasedTriggeringPolicy 基于时间的滚动策略
interval 指定多久滚动一次，默认是1 hour
modulate modulate=true用来调整时间：比如现在是早上3am，interval是4，那么第一次滚动是在4am，接着是8am，12am…而不是7am.
  - SizeBasedTriggeringPolicy基于指定文件大小的滚动策略
size 超过size日志文件的大小就回滚.
  - DefaultRolloverStrategy 用来指定同一个文件夹下最多有几个日志文件时开始删除最旧的，创建新的max 文件夹下最多日志文件数量
  
Routing 指定日志路由，可以指定规则与Appender进行绑定   
- name 指定Appender的名字
- Routes

Loggers 指定logger,loggers与appeders进行关联，将loggers中的日志输出到appenders，由appenders实现日志的控制台输出或者文件记录。
- Root 用来指定项目的根日志，如果没有单独指定Logger，那么就会默认使用该Root日志输出
    - level 日志输出级别，共有8个级别，按照从低到高为：All < Trace < Debug < Info < Warn < Error < Fatal < OFF.
    - AppenderRef用来指定该日志输出到哪个Appender.。
- Logger 自定义的子日志
    - level 日志输出级别，共有8个级别，按照从低到高为：All < Trace < Debug < Info < Warn < Error < Fatal < OFF。
    - name 用来指定该Logger所适用的类或者类所在的包全路径,继承自Root节点。
    - additivity 日志是否在父Logger中输出。   
    - AppenderRef用来指定该日志输出到哪个Appender,如果没有指定，就会默认继承自Root.如果指定了，那么会在指定的这个Appender和Root的Appender中都会输出，此时我们可以设置Logger的additivity="false"只在自定义的Appender中进行输出。



日志系列之WEB应用中使用Log4j2
https://luruipeng.blog.csdn.net/article/details/78802644

配置log4j2.xml
https://blog.csdn.net/Huozhiwu_11/article/details/89636618
https://blog.csdn.net/thekenofDIS/article/details/80439776
https://blog.csdn.net/kellyfun/article/details/71470797
https://blog.csdn.net/a842699897/article/details/80836132

Mybatis 的Log4j日志输出问题
https://liuzh.blog.csdn.net/article/details/22931341


springboot配置log4j
https://zzq23.blog.csdn.net/article/details/87629782
https://blog.csdn.net/Numb_ZL/article/details/119256582

log4j2实现日志跟踪
https://blog.csdn.net/qq_39529562/article/details/107943739


Akka
https://blog.csdn.net/crazymakercircle/article/details/109432688

https://blog.csdn.net/thekenofDIS/article/details/80439776