https://dubbo.apache.org/zh/docs/v2.7/user/examples/local-stub/

一、实践
非直连
- Zookeeper安装与启动
- dubbo-api，存放公共接口
- dubbo-provider，提供远程服务，使用dubbo包的@service注入
- dubbo-consumer，调用远程服务，使用远程注入@Reference


直连
https://blog.csdn.net/qq_45896330/article/details/125794947
https://blog.csdn.net/wang_luwei/article/details/123778370
https://blog.csdn.net/noaman_wgs/article/details/70214612?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166468602216782395316530%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166468602216782395316530&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-4-70214612-null-null.142^v51^pc_rank_34_1,201^v3^control_1&utm_term=dubbo%E4%BD%BF%E7%94%A8&spm=1018.2226.3001.4187


FAQ:
一、Dubbo服务整体的注册调用流程
1. 服务容器负责启动，加载，运行服务提供者。
2. 服务提供者在启动时，向注册中心注册自己提供的服务。
3. 服务消费者在启动时，向注册中心订阅自己所需的服务。
4. 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
5. 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
6. 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

点涉及：服务端暴露，消息端的引入，序列化协议，SPI,容错和负载均衡，配置规则的优先级，核心配置(服务端，消费端)，通信框架

1、服务端的服务暴露过程


2、消费端的引入过程


https://blog.csdn.net/xDroid_linzhuo/article/details/117296666


3、序列化协议的选择


1、接口的远程调用的实现方式https://blog.csdn.net/weixin_41605937/article/details/115371013



2、dubbo


1、消费者在使用暴露的服务时候，依赖注入是如何获取的。消费者这边没有写sprig的注解，使用的是bean的方式
- dubbo-provider，提供远程服务，使用dubbo包的@service注入
- dubbo-consumer，调用远程服务，使用远程注入
@Reference支持远程注入
@Autowired@Resource只支持本地注入


2、一个标准的 URL 格式至多可以包含如下的几个部分
dubbo://0.0.0.0:9090/com.alibaba.dubbo.registry.RegistryService?application=hca-001-consumer&default.check=false&dubbo=2.6.2&interface=com.hca.dcommon.open.UserService&methods=getUserLabel&organization=dubbox&owner=programmer&pid=10892&register.ip=169.254.234.16&side=consumer&timestamp=1664704375535


描述一个 dubbo 协议的服务
dubbo://192.168.1.6:20880/moe.cnkirito.sample.HelloService?timeout=3000
 
描述一个 zookeeper 注册中心
zookeeper://127.0.0.1:2181/org.apache.dubbo.registry.RegistryService?application=demo-consumer&dubbo=2.0.2&interface=org.apache.dubbo.registry.RegistryService&pid=1214&qos.port=33333&timestamp=1545721981946
 
描述一个消费者
consumer://30.5.120.217/org.apache.dubbo.demo.DemoService?application=demo-consumer&category=consumers&check=false&dubbo=2.0.2&interface=org.apache.dubbo.demo.DemoService&methods=sayHello&pid=1209&qos.port=33333&side=consumer&timestamp=1545721827784


启动过程
http://t.zoukankan.com/eric-lin-p-4959638.html

3、Dubbo 中常用标签。
分为三个类别：公用标签，服务提供者标签，服务消费者标签。
公用标签：配置应用信息 <dubbo:application/> 和  配置注册中心 <dubbo:registry/>
服务提供者标签：配置暴露的服务 <dubbo:service interface=”服务接口名” ref=”服务实现对象 bean”>
服务消费者标签：引用远程服务 <dubbo:reference id=”服务引用 bean 的 id” interface=”服务接口名”/>
https://blog.csdn.net/weixin_43823808/article/details/117335995
