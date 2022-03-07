## Spring简述

Spring是一种轻量级开发框架，旨在提高开发人员的开发效率以及系统的可维护性。

框架中有许多模块：核心容器、数据访问/集成、Web、AOP(面向切面编程)、工具、消息和测试模块。

Spring六个特征

1. 核心技术：依赖注入，AOP，事件，资源，i18n，验证，数据绑定，类型转换，SpEL
2. 测试：模拟对象，TestContext框架，Spring MVC测试， WebTestClient
3. 数据访问：事务，DAO支持，JDBC，ORM，编组XML
4. Web支持：Spring MVC和Spring WebFlux Web框架
5. 集成：远程处理， JMS， JCA， JMX， 电子邮件，任务，调度，缓存
6. 语言：Kotlin， Groovy， 动态语言

### Spring一些重要模块

* Spring Core：基础，Spring其他所有功能都需要依赖于该类库。主要提供IOC依赖注入功能
* Spring As额从天上：为与AspectJ的集成提供支持
* Spring AOP：提供了面向切面编程的实现
* Spring JDBC：Java数据库连接
* Spring JMS：Java消息服务
* Spring ORM：用于支持Hibernate等ORM工具
* Spring Web：为创建Web应用程序提供支持
* Spring Test：提供了对JUnit和TestNG测试的支持

## @RestController和@Controller

Controller返回一个页面

如果单独使用@Controller不加@ResponseBody的话，一般使用在要返回一个视图的情况，传统的Spring MVC应用，对应于前后端不分离的情况

**@RestController返回JSON或XML形式的数据**

只返回对象，对象数据直接以JSON或者XML形式写入HTTP相应中，属于TESTful Web服务，前后端分离

@RestController = @Controller + @ResponseBody

## Spring IOC和AOP

### IOC(Inverse of Control:控制反转)

一种设计思想，将原本在程序中手动创建对象的控制权，交给Spring框架来管理。IOC容器是Spring用来实现IOC的载体，实际上是一个Map，存放的是各种对象。

IOC容易就像是工厂，创建一个对象的时候，只需要配置好相关文件和注解即可，完全不用考虑对象是如何被创建出来的。Spring时代通过XML文件来配置Bean，后来SpringBoot注解配置开始流行。

Spring IOC初始化过程

XML---读取--->Resource---解析--->BeanDefinition---注册--->BeanFactory

### AOP(Aspect-Oriented Programming面向切面编程)

AOP能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块间的耦合度，有利于未来的可拓展性和可维护性。

Spring AOP是基于动态代理的。如果代理的对象实现了某个接口，Spring AOP会使用JDK Proxy去创建代理对象；如果没有实现接口，Spring AOP会使用Cglib生成一个被代理对象的子类来作为处理。

除此之外，可以使用AspectJ，是Java省泰中最完整的AOP框架。

使用AOP可以把一些功能抽象出来，在需要用到的地方直接使用，大大简化了代码量，新增功能时也更方便。

### Spring AOP和AspectJ AOP区别

Spring AOP属于运行时增强，而AspectJ AOP是编译时增强。

Spring AOP基于代理，而AspectJ AOP基于字节码操作。

Spring AOP继承了AspectJ AOP，AspectJ AOP功能更强大，而Spring AOP相对来说更简单。

如果切面比较少，那么二者差异不大；如果切面太多，选择AspectJ AOP会被Spring AOP快很多。

## Spring Bean

### Spring中bean的作用域

* singleton：唯一bean实例，默认bean都是单例的
* protorype：每次请求都会创建一个新的bean实例
* request：每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效
* session：每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP session内有效
* global-session：全局session作用域，仅仅在基于portlet的web应用中才有意义，Spring5已经没有了。

### Spring中单例bean的线程安全问题

单例bean存在线程安全问题，主要是因为当多个线程操作同一个对象的时候，对这个对象的非静态成员变量的写操作会出现安全问题。

解决方案：

1. 在bean对象中尽量避免定义可变成员变量（不太现实）
2. 在类中定义一个ThreadLocal成员变量，将需要的可变成员变量保存在ThreadLocal中

### @Bean和@Component

1. @Bean注解作用于方法，@Component作用于类
2. @Component通常是通过类路径 扫描来自动侦测以及自动装配到Spring容器中；@Bean通常是通过我们在标有该注解的方法中定义产生这个bean，@Bean告诉了Spring这是某个类的示例，当我需要用它的时候还给我
3. @Bean注解比@Component自定义性更强，而且很多地方只能通过@Bean来注册bean。比如当引入第三方库的类需要装配到Spring容器时，只能用@Bean来实现

### 将一个类声明成Spring的bean有哪些注解

一般使用@Autowired注解自动装配bean。要想把类标识成可用于@Autowired注解自动装配的bean类，可以采用以下注解

1. @Component：可以标注任意类为Spring组件。
2. @Respository：对应持久层即DAO层，主要用于数据库相关操作
3. @Service：对应服务层，主要涉及一些复杂的逻辑，需要用到DAO层
4. @Controller：对应Spring MVC控制层，主要用户接受用户请求并调用Service层返回数据给前端页面

### bean的生命周期

1. Bean容器找到配置文件中Spring Bean的定义
2. Bean容器利用Java Reflection API创建一个Bean的实例
3. 如果涉及到一些属性值利用`set()`方法设置
4. 如果Bean实现了`BeanNameAware`接口，调用`setBeanName()`方法传入Bean名字
5. 如果Bean实现了`BeanClassLoaderAware`接口，调用`setBeanClassLoader()`方法啊，传入`ClassLoader`对象实例
6. 如果Bean还实现了其他`*Aware`接口，就调用对应的方法
7. 如果有和加载这个Bean的Spring容器相关的`BeanPostProcessor`对象，执行`postProcessBeforeInitialization()`方法
8. 如果Bean实现了`InitializingBean`接口，只想`afterPropertiesSet()`方法
9. 如果Bean在配置文件的定义中包含了init-method属性，执行指定的方法
10. 如果有和加载这个Bean的Spring容器相关的`BeanPostProcessor`对象，执行`postProcessAfterInitialization()`方法
11. 当要销毁Bean时，如果Bean实现了`DisposableBean`接口，执行`destroy()`方法
12. 当要销毁Bean时，如果Bean在配置文件中定义了destory-method属性，执行指定的方法

## Spring MVC

Model1时代：整个Web应用几乎全部用JSP页面组成，只使用少量的JavaBean来处理数据库连接、访问等操作。

Model2时代：Java Bean(Model) + JSP(View) + Servlet(Controller)

Spring MVC是当前最优秀的MVC框架。

Spring MVC可以帮助我们进行更简洁的Web层的开发，并且它天生与Spring框架集成。Spring MVC下我们一般把后端项目分为Service层（处理业务M）、DAO层（数据库操作M）、Entity层（实体层M）、Controller层（控制层C，返回数据给前端页面）

### Spring MVC的工作原理

1. 客户端（浏览器）发送请求，直接请求到DispatcherServlet
2. DispatcherSevlet根据请求信息调用HandlerMapping，解析请求对应的Handler
3. 解析对应的Handler（即Controller控制器）后，开始由HandlerAdapter适配器处理
4. HandlerAdapter会根据Handler来调用真正的处理器开始处理请求，并处理相应的业务逻辑
5. 处理器处理完业务后，会返回一个ModelAndView对象，Model是返回的数据对象，View是逻辑上的View
6. ViewResolver会本剧View查找实际的View
7. DispatcherServlet把返回的Model传给View（视图渲染）
8. 把View返回给请求者（浏览器）

## Spring框架中的设计模式

1. 工厂设计模式：Spring使用工厂模式通过BeanFactory，ApplicationContext创建bean对象
2. 代理设计模式：Spring AOP功能的实现
3. 单例设计模式：Spring中的Bean默认都是单例的
4. 包装器设计模式：项目连接多个数据库，而且不同的客户在每次访问中根据需要会访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源
5. 观察者模式：Spring事件驱动模型就是观察者模式很经典的一个应用
6. 适配器模式：Spring AOP的增强或通知使用到了适配器模式，MVC中也使用到了适配器模式适配Controller

## Spring事务

### Spring事务的分类

1. 编程式事务，在代码中硬编码（不推荐）
2. 声明式事务，在配置文件中配置
   * 基于XML的声明式事务
   * 基于注解的声明式事务

### Spring事务的隔离级别

TransactionDefinition接口定义了五个隔离级别常量

1. TransactionDefinition.ISOLATION_DEFAULT:使用后端数据库默认的隔离级别，MySQL默认采用REPEATABLE_READ级别
2. TransactionDefinition.ISOLATION_READ_UNCOMMITTED:最低隔离级别，允许读取尚未提交的数据变成，可能导致脏读、幻读、不可重复读
3. TransactionDefinition.ISOLATION_READ_COMMITTED:允许读取并发事务已经提交的数据，可以阻止脏读，但是会发生幻读和不可重复读
4. TransactionDefinition.ISOLATION_REPEATABLE_READ:对同一字段的多次读取结果都是一致的，除非数据是被本身事务修改，可以阻止脏读和不可重复读，但是幻读还会有
5. TransactionDefinition.ISOLATION_SERIALIZABLE:最高级别的隔离，完全服从ACID的隔离级别。所有事务依次逐个执行，事务之间没有干扰，可以阻止脏读、幻读和不可重复读。但是因为效率降低，所以通常不使用

### Spring事务的传播行为

支持当前事务的情况

1. TransactionDefinition.PROPAGATION_REQUIRED:如果当前存在事务，则加入该事务；如果不存在，就新建一个
2. TransactionDefinition.PROPAGATION_SUPPORTS:如果当前存在事务，则加入该事务；如果不存在，则以非事务方式运行
3. TransactionDefinition.PROPAGATION_MANDATORY:如果当前存在事务，则加入；如果不存在，则抛出异常

不支持当前事务：

1. TransactionDefinition.PROPAGATION_REQUIRES_NEW:创建一个新事务，如果点钱存在，则把当前事务挂起
2. TransactionDefinition.PROPAGATION_NOT_SUPPORTED:以非事务方式运行，如果存在当前事务，则把当前事务挂起
3. TransactionDefinition.PROPAGATION_NEVER:以非事务方式运行，如果当前存在事务，则抛出异常

其他情况

TransactionDefinition.PROPAGATION_NESTED:如果当前存在事务，则创建一个事务作为当前事务的嵌套来运行，如果当前没有，则新建一个

### @Transactional(rollbackFor = Exception.class)注解

当@Transactional作用于类上时，该类的所有public方法将具有该类型的事务属性，同时我们也可以在方法级别使用该标注来覆盖级别类的定义。如果类或方法有这个注解，那么这个类里方法抛出的 异常，就会回滚，数据库里面的数据也会回滚

在@Transactional注解如果不配置rollbackFor属性，那么事务只会在遇到RuntimeException的时候才会回滚，加上rollbackFor = Exception.class可以让事务在非运行时异常也回滚

## 如何使用JPA在数据库中非持久化一个字段

假设一个Entity类，不想某个属性持久化，可以采取几个方法

1. 加static
2. 加final
3. 加transient
4. 加@Transient注解



