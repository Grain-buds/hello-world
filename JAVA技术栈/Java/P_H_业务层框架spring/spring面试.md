1. Spring基础
1.1 什么是 spring?
1.2 Spring框架的设计目标，设计理念，和核心是什么？
1.3 Spring Framework 中有多少个模块，它们分别是什么？
1.4 Spring的优缺点是什么？
1.5 Spring 框架中都用到了哪些设计模式？
1.6 spring context应用上下文有什么作用？
1.7 Spring 应用程序有哪些不同组件？
1.8 使用 Spring 有哪些方式？
1.9 Spring框架中有哪些不同类型的事件？
2. Spring IoC
2.1 什么是 Spring IOC 容器？
2.2 控制反转(IoC)有什么作用？
2.3 IoC 的好处有哪些？
2.4 Spring 的 IoC支持哪些功能?
2.5 Spring IoC 的实现机制
2.6 Spring 如何设计容器的，BeanFactory和ApplicationContext的关系详解?
2.7 BeanFactory 和 ApplicationContext有什么区别?
2.8 ApplicationContext通常的实现是什么？
2.9 什么是Spring的依赖注入？
2.10 依赖注入的基本原则是什么？
2.11 依赖注入有什么优势？
2.12 可以通过多少种方式完成依赖注入？
2.13 构造器依赖注入和 Setter方法注入有什么区别？
3. Spring Beans
3.1 什么是 spring bean？
3.2 spring 提供了哪些配置方式？
3.3 spring 支持几种 bean scope？
3.4 Spring配置文件包含了哪些信息？
3.5 Spring基于xml注入bean的几种方式
3.6 Spring框架中的单例bean是线程安全的吗？
3.7 Spring如何处理线程并发问题？
3.8 spring bean 容器的生命周期是什么样的？
3.9 bean生命周期方法有哪些？ 你能重载它们吗？
3.10 在 Spring中如何注入一个java集合？
3.11 什么是 spring 的内部 bean？
3.12 什么是bean装配？什么是bean的自动装配？
3.13 自动装配有哪些方式？
3.14 使用@Autowired注解自动装配的过程是怎样的？
3.15 自动装配有什么局限？
3.16 你可以在Spring中注入一个null 和一个空字符串吗？
4. Spring注解
4.1 什么是基于Java的Spring注解配置?
4.2 怎样开启注解装配？
4.3 @Component, @Controller, @Repository, @Service 有何区别？
4.4 @Required 注解有什么作用？
4.5 @Autowired 注解有什么作用？
4.6 @Autowired和@Resource有什么区别？
4.7 @Qualifier注解有什么作用？
4.8 @RequestMapping注解有什么用？
5. Spring数据访问
5.1 解释对象/关系映射集成模块
5.2 在Spring框架中如何更有效地使用JDBC？
5.3 解释JDBC抽象和DAO模块的作用是什么？
5.4 Spring DAO 有什么用？
5.5 Spring JDBC API 中存在哪些类？
5.6 JdbcTemplate是什么？
5.7 Spring通过什么方式访问Hibernate？
5.8 如何通过HibernateDaoSupport将Spring和Hibernate结合起来？
5.9 Spring支持的事务管理类型是什么？spring 事务实现方式有哪些？
5.10 Spring事务的实现方式和实现原理是什么？
5.11 Spring的事务传播行为有那些？
5.12 说一下 Spring 的事务隔离？
5.13 Spring框架的事务管理有哪些优点？
5.14 你更倾向用那种事务管理类型？
6. Spring AOP
6.1 什么是AOP？
6.2 Spring通知有哪些类型？
6.3 什么是基于XML Schema方式的切面实现？
6.4 什么是切面 Aspect？
6.5 Spring AOP and AspectJ AOP 有什么区别？AOP 有哪些实现方式？
6.6 什么是基于注解的切面实现？
6.7 如何理解 Spring 中的代理？
6.8 JDK动态代理和CGLIB动态代理的区别是什么？
6.9 有几种不同类型的自动代理？
6.10 请解释一下Spring AOP核心的名称分别是什么意思？
6.11 为什么Spring只支持方法级别的连接点？
6.12 Spring AOP 中，关注点和横切关注的区别是什么？

---------------------------------

# 1. Spring基础
## 1.1 什么是 spring?
Spring是一个轻量级Java开发框架，它是一个分层的轻量级开源框架，为开发Java应用程序提供全面的基础架构支持。Spring负责基础架构，因此Java开发者可以专注于应用程序的开发。它的两个核心特性，也就是依赖注入（dependency injection，DI）和面向切面编程（aspect-oriented programming，AOP）。

Spring最根本的使命是简化Java开发。为了降低Java开发的复杂性，Spring采取了以下4种关键策略

基于POJO的轻量级和最小侵入性编程；
通过依赖注入和面向接口实现松耦合；
基于切面和惯例进行声明式编程；
通过切面和模板减少样板式代码。
## 1.2 Spring框架的设计目标，设计理念，和核心是什么？
Spring设计目标：
Spring为开发者提供一个一站式轻量级应用开发平台；


## 1.3 Spring Framework 中有多少个模块，它们分别是什么？
Spring 总共大约有 20 个模块。 而这些组件被分别整合在核心容器（Core Container） 、 AOP（Aspect Oriented Programming）和设备支持（Instrmentation） 、数据访问与集成（Data Access/Integeration）等 几 个模块中。 以下是 Spring 5 的模块结构图：



Spring 核心容器
提供了框架的基本组成部分，包括控制反转（Inversion of Control，IOC）和依赖注入（Dependency Injection，DI）功能。 

## 1.5 Spring 框架中都用到了哪些设计模式？
工厂模式：BeanFactory就是简单工厂模式的体现，用来创建对象的实例；
单例模式：Bean默认为单例模式。
代理模式：Spring的AOP功能用到了JDK的动态代理和CGLIB字节码生成技术；
模板方法：用来解决代码重复的问题。比如. RestTemplate
观察者模式：定义对象键一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都会得到通知被制动更新，如Spring中listener的实现–ApplicationListener。

## 1.6 spring context应用上下文有什么作用？
这是基本的Spring模块，提供spring 框架的基础功能，BeanFactory 是 任何以spring为基础的应用的核心。Spring 框架建立在此模块之上，它使Spring成为一个容器。用于获取注入的bean。



# 2. Spring IoC
## 2.1 什么是 Spring IOC 容器？
控制反转即IoC (Inversion of Control)，它把传统上由程序代码直接操控的对象的调用权交给容器，通过容器来实现对象组件的装配和管理。所谓的“控制反转”概念就是对组件对象控制权的转移，从程序代码本身转移到了外部容器。

Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期


2.2 控制反转(IoC)有什么作用？
管理对象的创建和依赖关系的维护。对象的创建并不是一件简单的事，在对象关系比较复杂时，如果依赖关系需要程序猿来维护的话，那是相当头疼的
解耦，由容器去维护具体的对象
托管了类的产生过程，比如我们需要在类的产生过程中做一些处理，最直接的例子就是代理，如果有容器程序可以把这部分处理交给容器，应用程序则无需去关心类是如何完成代理的
2.3 IoC 的好处有哪些？
它将最小化应用程序中的代码量。
它将使您的应用程序易于测试，因为它不需要单元测试用例中的任何单例
或JNDI 查找机制。
它以最小的影响和最少的侵入机制促进松耦合。
它支持即时的实例化和延迟加载服务。

2.4 Spring 的 IoC支持哪些功能?
Spring 的 IoC 设计支持以下功能：

依赖注入
依赖检查
自动装配
支持集合
指定初始化方法和销毁方法
支持回调某些方法（但是需要实现 Spring 接口，略有侵入）
其中，最重要的就是依赖注入，从 XML 的配置上说，即 ref 标签。对应 Spring RuntimeBeanReference 对象。

对于 IoC 来说，最重要的就是容器。容器管理着 Bean 的生命周期，控制着 Bean 的依赖注入。

2.5 Spring IoC 的实现机制
Spring 中的 IoC 的实现原理就是工厂模式加反射机制 。
示例：

interface Fruit {
  public abstract void eat();
}

class Apple implements Fruit {
  public void eat(){
    System.out.println("Apple");
  }
}
class Orange implements Fruit {
  public void eat(){
    System.out.println("Orange");
  }
}
class Factory {
  public static Fruit getInstance(String ClassName) { 
    Fruit f=null;
    try {
      f=(Fruit)Class.forName(ClassName).newInstance(); } catch (Exception e) {
    e.printStackTrace();
    }
    return f;
  }
}

class Client {
  public static void main(String[] a) {
    Fruit f=Factory.getInstance("com.xxx.xxx.Apple");
    if(f!=null){
      f.eat();
    } 
  }
}



## 2.7 BeanFactory 和 ApplicationContext有什么区别?
BeanFactory和ApplicationContext是Spring的两大核心接口，都可以当做Spring的容器。其中ApplicationContext是BeanFactory的子接口。

依赖关系
BeanFactory：是Spring里面最底层的接口，包含了各种Bean的定义，读取bean配置文档，管理bean的加载、实例化，控制bean的生命周期，维护bean之间的依赖关系。
ApplicationContext接口作为BeanFactory的派生，除了提供BeanFactory所具有的功能外，还提供了更完整的框架功能：




## 2.9 什么是Spring的依赖注入？
控制反转IoC是一个很大的概念，可以用不同的方式来实现。其主要实现方式有两种：依赖注入和依赖查找

依赖注入：相对于IoC而言，依赖注入(DI)更加准确地描述了IoC的设计理念。所谓依赖注入（Dependency Injection），即组件之间的依赖关系由容器在应用系统运行期来决定，也就是由容器动态地将某种依赖关系的目标对象实例注入到应用系统中的各个关联的组件之中。组件不做定位查询，只提供普通的Java方法让容器去决定依赖关系。

## 2.10 依赖注入的基本原则是什么？
依赖注入的基本原则是：应用组件不应该负责查找资源或者其他依赖的协作对象。配置对象的工作应该由IoC容器负责，“查找资源”的逻辑应该从应用组件的代码中抽取出来，交给IoC容器负责。容器全权负责组件的装配，它会把符合依赖关系的对象通过属性（JavaBean中的setter）或者是构造器传递给需要的对象。

## 2.11 依赖注入有什么优势？
依赖注入之所以更流行是因为它是一种更可取的方式：让容器全权负责依赖查询，受管组件只需要暴露JavaBean的setter方法或者带参数的构造器或者接口，使容器可以在初始化时组装对象的依赖关系。其与依赖查找方式相比，主要优势为：

查找定位操作与应用代码完全无关。
不依赖于容器的API，可以很容易地在任何容器以外使用应用对象。
不需要特殊的接口，绝大多数对象可以做到完全不必依赖容器。
## 2.12 可以通过多少种方式完成依赖注入？
通常，依赖注入可以通过三种方式完成，即：

构造函数注入

setter 注入

接口注入（由于在灵活性和易用性比较差，现在从Spring4开始已被废弃）

## 2.13 构造器依赖注入和 Setter方法注入有什么区别？
构造函数注入	setter 注入
没有部分注入	有部分注入
不会覆盖 setter 属性	会覆盖 setter 属性
任意修改都会创建一个新实例	任意修改不会创建一个新实例
适用于设置很多属性	适用于设置少量属性
两种依赖方式都可以使用，构造器注入和Setter方法注入。最好的解决方案是用构造器参数实现强制依赖，setter方法实现可选依赖。

# 3. Spring Beans
## 3.1 什么是 spring bean？
它们是构成用户应用程序主干的对象。
Bean 由 Spring IoC 容器管理。
它们由 Spring IoC 容器实例化，配置，装配和管理。
Bean 是基于用户提供给容器的配置元数据创建。
## 3.2 spring 提供了哪些配置方式？
基于xml配置
bean 所需的依赖项和服务在 XML 格式的配置文件中指定 。 这些配置文件通常包含许多 bean 定义和特定于应用程序的配置选项。 它们通常以 bean 标签开头。例如：

<bean id="studentbean" class="org.edureka.firstSpring.StudentBean"> 
  <property name="name" value="Edureka"></property>
</bean>

基于注解配置
您可以通过在相关的类，方法或字段声明上使用注解，将 bean 配置为组件类本身，而不是使用 XML 来描述 bean 装配。 默认情况下， Spring 容器中未打开注解装配。 因此，您需要在使用它之前在 Spring 配置文件中启用它。 例如：

<beans>
<context:annotation-config/>
<!-- bean definitions go here -->
</beans>

基于 Java API 配置
Spring的Java配置是通过使用@Bean和@Configuration来实现 。

@Bean
注解扮演与元素相同的角色 。
@Configuration 类
允许通过简单地调用同一个类中的其他@Bean方法来定义bean间依赖关系。
例如：

@Configuration
public class StudentConfig {
  @Bean
  public StudentBean myStudent() {
    return new StudentBean();
  }
}


## 3.3 spring 支持几种 bean scope？
Spring bean 支持5种scope：

Singleton
每个 Spring IoC 容器仅有一个单实例 。
Prototype
每次请求都会产生一个新的实例。
Request
每一次 HTTP 请求都会产生一个新的实例，并且该 bean 仅在当前 HTTP 请求内有效。
Session
每一次 HTTP 请求都会产生一个新的 bean，同时该 bean 仅在当前 HTTP session 内有效。
Global-session
类似于标准的 HTTP Session 作用域，不过它仅仅在基于 portlet 的 web 应用中才有意义 。 Portlet 规范定义了全局 Session 的概念，它被所有构成某个 portlet web 应用的各种不同的 portlet 所共享。 在 global session 作用域中定义的 bean 被限定于全局 portlet Session 的生命周期范围内。 如果你在 web 中使用 global session 作用域来标识 bean，那么 web 会自动当成 session 类型来使用。
仅当用户使用支持Web的ApplicationContext时，最后三个才可用 。

3.4 Spring配置文件包含了哪些信息？
Spring配置文件是个XML 文件，这个文件包含了类信息，描述了如何配置它们，以及如何相互调用。


## 3.6 Spring框架中的单例bean是线程安全的吗？
不是，Spring框架中的单例bean不是线程安全的。

spring 中的 bean 默认是单例模式，spring 框架并没有对单例 bean 进行多线程的封装处理。

实际上大部分时候 spring bean 无状态的（比如 dao 类），所有某种程度上来说 bean 也是安全的，但如果 bean 有状态的话（比如 view model 对象），那就要开发者自己去保证线程安全了，最简单的就是改变 bean 的作用域，把“singleton”变更为“prototype”，这样请求 bean 相当于 new Bean()了，所以就可以保证线程安全了。

有状态就是有数据存储功能。
无状态就是不会保存数据。
## 3.7 Spring如何处理线程并发问题？
在一般情况下，只有无状态的Bean才可以在多线程环境下共享，在Spring中，绝大部分Bean都可以声明为singleton作用域，因为Spring对一些Bean中非线程安全状态采用ThreadLocal进行处理，解决线程安全问题。

ThreadLocal和线程同步机制都是为了解决多线程中相同变量的访问冲突问题。同步机制采用了“时间换空间”的方式，仅提供一份变量，不同的线程在访问前需要获取锁，没获得锁的线程则需要排队。而ThreadLocal采用了“空间换时间”的方式。

ThreadLocal会为每一个线程提供一个独立的变量副本，从而隔离了多个线程对数据的访问冲突。因为每一个线程都拥有自己的变量副本，从而也就没有必要对该变量进行同步了。ThreadLocal提供了线程安全的共享对象，在编写多线程代码时，可以把不安全的变量封装进ThreadLocal。

## 3.8 spring bean 容器的生命周期是什么样的？
spring bean 容器的生命周期流程如下：

1、Spring 容器根据配置中的 bean 定义中实例化 bean。
2、Spring 使用依赖注入填充所有属性，如 bean 中所定义的配置。
3、如果 bean 实现 BeanNameAware 接口，则工厂通过传递 bean 的 ID 来调用setBeanName()。
4、如果 bean 实现 BeanFactoryAware 接口，工厂通过传递自身的实例来调用 setBeanFactory()。
5、如果存在与 bean 关联的任何BeanPostProcessors，则调用 preProcessBeforeInitialization() 方法 。
6、如果为 bean 指定了 init 方法（ 的 init-method 属性），那么将调用它。
7、最后，如果存在与 bean 关联的任何 BeanPostProcessors，则将调用 postProcessAfterInitialization() 方法。
8、如果 bean 实现 DisposableBean 接口，当 spring 容器关闭时，会调用 destory()。
9、如果为 bean 指定了 destroy 方法（ 的 destroy-method 属性），那么将调用它。


## 3.9 bean生命周期方法有哪些？ 你能重载它们吗？
有两个重要的bean 生命周期方法：

第一个是setup ：
它是在容器加载bean的时候被调用。
第二个方法是 teardown ：
它是在容器卸载类的时候被调用。
bean 标签有两个重要的属性（init-method和destroy-method）。用它们你可以自己定制初始化和注销方法。它们也有相应的注解（@PostConstruct和**@PreDestroy**）。

## 3.12 什么是bean装配？什么是bean的自动装配？
装配，或bean装配是指在Spring容器中把bean组装到一起，前提是容器需要知道bean的依赖关系，如何通过依赖注入来把它们装配到一起。

在Spring框架中，在配置文件中设定bean的依赖关系是一个很好的机制，Spring 容器能够自动装配相互合作的bean，这意味着容器不需要和配置，能通过Bean工厂自动处理bean之间的协作。这意味着 Spring可以通过向Bean Factory中注入的方式自动搞定bean之间的依赖关系。自动装配可以设置在每个bean上，也可以设定在特定的bean上。

## 3.13 自动装配有哪些方式？
Spring容器能够自动装配bean。也就是说，可以通过检查BeanFactory的内容让Spring自动解析bean的协作者。

自动装配的不同模式：

no
这是默认设置，表示没有自动装配 。 应使用显式 bean 引用进行装配 。
byName
它根据 bean 的名称注入对象依赖项 。 它匹配并装配其属性与 XML 文件中由相同名称定义的 bean。
byType
它根据类型注入对象依赖项。如 果属性的类型与 XML 文件中的一个 bean 名称匹配，则匹配并装配属性。
构造函数
它通过调用类的构造函数来注入依赖项 。它有大量的参数 。
autodetect
首先容器尝试通过构造函数使用 autowire 装配，如果不能，则尝试通过 byType 自动装配。
## 3.14 使用@Autowired注解自动装配的过程是怎样的？
使用@Autowired注解来自动装配指定的bean。在使用@Autowired注解之前需要在Spring配置文件进行配置，<context:annotation-config />。

在启动spring IoC时，容器自动装载了一个AutowiredAnnotationBeanPostProcessor后置处理器，当容器扫描到@Autowied、@Resource或@Inject时，就会在IoC容器自动查找需要的bean，并装配给该对象的属性。在使用@Autowired时，首先在容器中查询对应类型的bean：

如果查询结果刚好为一个，就将该bean装配给@Autowired指定的数据；
如果查询的结果不止一个，那么@Autowired会根据名称来查找；
如果上述查找的结果为空，那么会抛出异常。解决方法时，使用required=false。
## 3.15 自动装配有什么局限？
覆盖的可能性
您始终可以使用 和 设置指定依赖项，这将覆盖自动装配。
基本元数据类型
简单属性（如原数据类型，字符串和类）无法自动装配。
模糊特性
自动装配不如显式装配精确，如果有可能，建议使用显式装配。


## 4.3 @Component, @Controller, @Repository, @Service 有何区别？
@Component：
这将 java 类标记为 bean。它是任何 Spring 管理组件的通用构造型。spring 的组件扫描机制现在可以将其拾取并将其拉入应用程序环境中。

@Controller：
这将一个类标记为 Spring Web MVC 控制器。标有它的 Bean 会自动导入到 IoC 容器中。

@Service：
此注解是组件注解的特化。它不会对 @Component 注解提供任何其他行为。您可以在服务层类中使用 @Service 而不是 @Component，因为它以更好的方式指定了意图。

@Repository：
这个注解是具有类似用途和功能的 @Component 注解的特化。它为 DAO 提供了额外的好处。它将 DAO 导入 IoC 容器，并使未经检查的异常有资格转换为 Spring DataAccessException。

## 4.4 @Required 注解有什么作用？
这个注解表明bean的属性必须在配置的时候设置，通过一个bean定义的显式的属性值或通过自动装配，若@Required注解的bean属性未被设置，容器将抛出BeanInitializationException。示例：

## 4.5 @Autowired 注解有什么作用？
@Autowired默认是按照类型装配注入的，默认情况下它要求依赖对象必须存在（可以设置它required属性为false）。@Autowired 注解提供了更细粒度的控制，包括在何处以及如何完成自动装配。它的用法和@Required一样，修饰setter方法、构造器、属性或者具有任意名称和/或多个参数的PN方法。

## 4.7 @Qualifier注解有什么作用？
当您创建多个相同类型的 bean 并希望仅使用属性装配其中一个 bean 时，您可以使用@Qualifier 注解和 @Autowired 通过指定应该装配哪个确切的 bean 来消除歧义。

## 4.8 @RequestMapping注解有什么用？
@RequestMapping 注解用于将特定 HTTP 请求方法映射到将处理相应请求的控制器中的特定类/方法。此注释可应用于两个级别：

类级别：
映射请求的 URL
方法级别：
映射 URL
HTTP 请求方法

# 5. Spring数据访问

## 5.5 Spring JDBC API 中存在哪些类？
JdbcTemplate
SimpleJdbcTemplate
NamedParameterJdbcTemplate
SimpleJdbcInsert
SimpleJdbcCall
5.6 JdbcTemplate是什么？
JdbcTemplate 类提供了很多便利的方法解决诸如把数据库数据转变成基本数据类型或对象，执行写好的或可调用的数据库操作语句，提供自定义的数据错误处理。


## 5.9 Spring支持的事务管理类型是什么？spring 事务实现方式有哪些？
Spring支持两种类型的事务管理：

编程式事务管理：
这意味你通过编程的方式管理事务，给你带来极大的灵活性，但是难维护。

声明式事务管理：
这意味着你可以将业务代码和事务管理分离，你只需用注解和XML配置来管理事务。

## 5.10 Spring事务的实现方式和实现原理是什么？
Spring事务的本质其实就是数据库对事务的支持，没有数据库的事务支持，spring是无法提供事务功能的。真正的数据库层的事务提交和回滚是通过binlog或者redo log实现的。

## 5.11 Spring的事务传播行为有那些？
Spring事务的传播行为说的是，当多个事务同时存在的时候，spring如何处理这些事务的行为。

PROPAGATION_REQUIRED：
如果当前没有事务，就创建一个新事务，如果当前存在事务，就加入该事务，该设置是最常用的设置。
PROPAGATION_SUPPORTS：
支持当前事务，如果当前存在事务，就加入该事务，如果当前不存在事务，就以非事务执行。
PROPAGATION_MANDATORY：
支持当前事务，如果当前存在事务，就加入该事务，如果当前不存在事务，就抛出异常。
PROPAGATION_REQUIRES_NEW：
创建新事务，无论当前存不存在事务，都创建新事务。
PROPAGATION_NOT_SUPPORTED：
以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
PROPAGATION_NEVER：
以非事务方式执行，如果当前存在事务，则抛出异常。
PROPAGATION_NESTED：
如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则按REQUIRED属性执行。
## 5.12 说一下 Spring 的事务隔离？
spring 有五大隔离级别，默认值为 ISOLATION_DEFAULT（使用数据库的设置），其他四个隔离级别和数据库的隔离级别一致：

ISOLATION_DEFAULT：用底层数据库的设置隔离级别，数据库设置的是什么我就用什么；
ISOLATION_READ_UNCOMMITTED：未提交读，最低隔离级别、事务未提交前，就可被其他事务读取（会出现幻读、脏读、不可重复读）；
ISOLATION_READ_COMMITTED：提交读，一个事务提交后才能被其他事务读取到（会造成幻读、不可重复读），SQL server 的默认级别；
ISOLATION_REPEATABLE_READ：可重复读，保证多次读取同一个数据时，其值都和事务开始时候的内容是一致，禁止读取到别的事务未提交的数据（会造成幻读），MySQL 的默认级别；
ISOLATION_SERIALIZABLE：序列化，代价最高最可靠的隔离级别，该隔离级别能防止脏读、不可重复读、幻读。
脏读 ：表示一个事务能够读取另一个事务中还未提交的数据。比如，某个事务尝试插入记录 A，此时该事务还未提交，然后另一个事务尝试读取到了记录 A。

不可重复读 ：是指在一个事务内，多次读同一数据。

幻读 ：指同一个事务内多次查询返回的结果集不一样。比如同一个事务 A 第一次查询时候有 n 条记录，但是第二次同等条件下查询却有 n+1 条记录，这就好像产生了幻觉。发生幻读的原因也是另外一个事务新增或者删除或者修改了第一个事务结果集里面的数据，同一个记录的数据内容被修改了，所有数据行的记录就变多或者变少了。

## 5.13 Spring框架的事务管理有哪些优点？
为不同的事务API 如 JTA，JDBC，Hibernate，JPA 和JDO，提供一个不变的编程模式。
为编程式事务管理提供了一套简单的API而不是一些复杂的事务API
支持声明式事务管理。
和Spring各种数据访问抽象层很好得集成。
## 5.14 你更倾向用那种事务管理类型？
大多数Spring框架的用户选择声明式事务管理，因为它对应用代码的影响最小，因此更符合一个无侵入的轻量级容器的思想。声明式事务管理要优于编程式事务管理，虽然比编程式事务管理（这种方式允许你通过代码控制事务）少了一点灵活性。唯一不足地方是，最细粒度只能作用到方法级别，无法做到像编程式事务那样可以作用到代码块级别。

# 6. Spring AOP
## 6.1 什么是AOP？
OOP(Object-Oriented Programming)面向对象编程，允许开发者定义纵向的关系，但并适用于定义横向的关系，导致了大量代码的重复，而不利于各个模块的重用。

AOP(Aspect-Oriented Programming)，一般称为面向切面编程，作为面向对象的一种补充，用于将那些与业务无关，但却对多个对象产生影响的公共行为和逻辑，抽取并封装为一个可重用的模块，这个模块被命名为“切面”（Aspect），减少系统中的重复代码，降低了模块间的耦合度，同时提高了系统的可维护性。可用于权限认证、日志、事务处理等。

## 6.2 Spring通知有哪些类型？
在AOP术语中，切面的工作被称为通知，实际上是程序执行时要通过SpringAOP框架触发的代码段。

Spring切面可以应用5种类型的通知：

前置通知（Before）：在目标方法被调用之前调用通知功能；
后置通知（After）：在目标方法完成之后调用通知，此时不会关心方法的输出是什么；
返回通知（After-returning ）：在目标方法成功执行之后调用通知；
异常通知（After-throwing）：在目标方法抛出异常后调用通知；
环绕通知（Around）：通知包裹了被通知的方法，在被通知的方法调用之前和调用之后执行自定义的行为。
同一个aspect，不同advice的执行顺序：

没有异常情况下的执行顺序：
around before advice
before advice
target method 执行
around after advice
after advice
afterReturning
有异常情况下的执行顺序：
around before advice
before advice
target method 执行
around after advice
after advice
afterThrowing:异常发生
java.lang.RuntimeException: 异常发生
6.3 什么是基于XML Schema方式的切面实现？
在这种情况下，切面由常规类以及基于XML的配置实现。

6.4 什么是切面 Aspect？
aspect 由 pointcount 和 advice 组成，切面是通知和切点的结合。 它既包含了横切逻辑的定义, 也包括了连接点的定义. Spring AOP 就是负责实施切面的框架, 它将切面所定义的横切逻辑编织到切面所指定的连接点中.
AOP 的工作重心在于如何将增强编织目标对象的连接点上, 这里包含两个工作:

如何通过 pointcut 和 advice 定位到特定的 joinpoint 上
如何在 advice 中编写切面代码.
可以简单地认为, 使用 @Aspect 注解的类就是切面.


## 6.5 Spring AOP and AspectJ AOP 有什么区别？AOP 有哪些实现方式？
AOP实现的关键在于 代理模式，AOP代理主要分为静态代理和动态代理。静态代理的代表为AspectJ；动态代理则以Spring AOP为代表。

1. AspectJ是静态代理的增强，所谓静态代理，就是AOP框架会在编译阶段生成AOP代理类，因此也称为编译时增强，他会在编译阶段将AspectJ(切面)织入到Java字节码中，运行的时候就是增强之后的AOP对象。

2. Spring AOP使用的动态代理，所谓的动态代理就是说AOP框架不会去修改字节码，而是每次运行时在内存中临时为方法生成一个AOP对象，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。

6.6 什么是基于注解的切面实现？
在这种情况下(基于@AspectJ的实现)，涉及到的切面声明的风格与带有java5标注的普通java类一致。

6.7 如何理解 Spring 中的代理？
将 Advice 应用于目标对象后创建的对象称为代理。在客户端对象的情况下，目标对象和代理对象是相同的。

Advice + Target Object = Proxy

6.8 JDK动态代理和CGLIB动态代理的区别是什么？
Spring AOP中的动态代理主要有两种方式，JDK动态代理和CGLIB动态代理：

JDK动态代理只提供接口的代理，不支持类的代理。核心InvocationHandler接口和Proxy类，InvocationHandler 通过invoke()方法反射来调用目标类中的代码，动态地将横切逻辑和业务编织在一起；接着，Proxy利用 InvocationHandler动态创建一个符合某一接口的的实例, 生成目标类的代理对象。
如果代理类没有实现 InvocationHandler 接口，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成指定类的一个子类对象，并覆盖其中特定方法并添加增强代码，从而实现AOP。CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。
静态代理与动态代理区别在于生成AOP代理对象的时机不同，相对来说AspectJ的静态代理方式具有更好的性能，但是AspectJ需要特定的编译器进行处理，而Spring AOP则无需特定的编译器处理。

InvocationHandler 的 invoke(Object proxy,Method method,Object[] args)：proxy是最终生成的代理实例; method 是被代理目标实例的某个具体方法; args 是被代理目标实例某个方法的具体入参, 在方法反射调用时使用。

6.9 有几种不同类型的自动代理？
BeanNameAutoProxyCreator
DefaultAdvisorAutoProxyCreator
Metadata autoproxying
6.10 请解释一下Spring AOP核心的名称分别是什么意思？
切面（Aspect）：
切面是通知和切点的结合。通知和切点共同定义了切面的全部内容。 在Spring AOP中，切面可以使用通用类（基于模式的风格） 或者在普通类中以 @AspectJ 注解来实现。
连接点（Join point）：
指方法，在Spring AOP中，一个连接点 总是 代表一个方法的执行。 应用可能有数以千计的时机应用通知。这些时机被称为连接点。连接点是在应用执行过程中能够插入切面的一个点。这个点可以是调用方法时、抛出异常时、甚至修改一个字段时。切面代码可以利用这些点插入到应用的正常流程之中，并添加新的行为。
通知（Advice）：
在AOP术语中，切面的工作被称为通知。
切入点（Pointcut）：
切点的定义会匹配通知所要织入的一个或多个连接点。我们通常使用明确的类和方法名称，或是利用正则表达式定义所匹配的类和方法名称来指定这些切点。
引入（Introduction）：
引入允许我们向现有类添加新方法或属性。
目标对象（Target Object）：
被一个或者多个切面（aspect）所通知（advise）的对象。它通常是一个代理对象。也有人把它叫做 被通知（adviced） 对象。 既然Spring AOP是通过运行时代理实现的，这个对象永远是一个 被代理（proxied） 对象。
织入（Weaving）：
织入是把切面应用到目标对象并创建新的代理对象的过程。在目标对象的生命周期里有多少个点可以进行织入：
编译期：切面在目标类编译时被织入。AspectJ的织入编译器是以这种方式织入切面的。
类加载期：切面在目标类加载到JVM时被织入。需要特殊的类加载器，它可以在目标类被引入应用之前增强该目标类的字节码。AspectJ5的加载时织入就支持以这种方式织入切面。
运行期：切面在应用运行的某个时刻被织入。一般情况下，在织入切面时，AOP容器会为目标对象动态地创建一个代理对象。SpringAOP就是以这种方式织入切面。
6.11 为什么Spring只支持方法级别的连接点？
因为Spring基于动态代理，所以Spring只支持方法连接点。Spring缺少对字段连接点的支持，而且它不支持构造器连接点。方法之外的连接点拦截功能，我们可以利用Aspect来补充。

6.12 Spring AOP 中，关注点和横切关注的区别是什么？
关注点（concern）是应用中一个模块的行为，一个关注点可能会被定义成一个我们想实现的一个功能。

横切关注点（cross-cutting concern）是一个关注点，此关注点是整个应用都会使用的功能，并影响整个应用，比如日志，安全和数据传输，几乎应用的每个模块都需要的功能。因此这些都属于横切关注点。



原文链接：https://blog.csdn.net/lupengfei1009/article/details/113417060