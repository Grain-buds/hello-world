代理的三种类型：静态代理、jdk动态代理和GCLib代理
区别：
静态代理：代理对象的一个接口只服务于一种类型的对象
JDK动态代理:必须针对接口(java的单继承，动态生成的代理类已经继承了Proxy类的，就不能再继承其他的类，所以只能靠实现被代理类的接口的形式)
GCLib代理:底层的字节码技术，可以为一个类创建子类，在子类中采用方法拦截的技术拦截所有父类方法的调用并加入横切逻辑，可以代理接口和类，对于final方法，无法进行代理

https://blog.csdn.net/swadian2008/article/details/119053469
https://blog.csdn.net/qq_40277163/article/details/124682538
https://blog.csdn.net/cgj296645438/article/details/90054562

springboot自动配置的代理，默认使用Cglib
Spring容器通过proxyTargetClass属性值来判断使用Cglib还Jdk代理,默认使用Cglib（如果对于代理目标对象是接口，则还是用Jdk代理）
可以在配置文件设置spring.aop.proxy-target-class为false控制使用Jdk代理（因为@EnableAspectJAutoProxy(proxyTargetClass = false)）
https://blog.csdn.net/weixin_43901882/article/details/119742442