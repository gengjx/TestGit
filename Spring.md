### 1.为什么要使用 spring？

```
Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器框架，它由Rod Johnson创建。它是为了解决企业应用开发的复杂性而创建的。Spring使用基本的JavaBean来完成以前只可能由EJB完成的事情。
```



### 2.什么是 aop？

```
  AOP（Aspect-Oriented Programming）指一种程序设计范型，该范型以一种称为切面（aspect）的语言构造为基础，切面是一种新的模块化机制，用来描述分散在对象、类或方法中的横切关注点（crosscutting concern）。

  Spring提供了面向切面编程的丰富支持，允许通过分离应用的业务逻辑与系统级服务（例如审计（auditing）和事务（transaction）管理）进行内聚性的开发。应用对象只实现它们应该做的——完成业务逻辑——仅此而已。它们并不负责（甚至是意识）其它的系统级关注点，例如日志或事务支持。
  
  Spring AOP中的动态代理主要有两种方式，JDK动态代理和CGLIB动态代理：
  ①JDK动态代理只提供接口的代理，不支持类的代理。核心InvocationHandler接口和Proxy类，InvocationHandler 通过invoke()方法反射来调用目标类中的代码，动态地将横切逻辑和业务编织在一起；接着，Proxy利用 InvocationHandler动态创建一个符合某一接口的的实例,  生成目标类的代理对象。
	②如果代理类没有实现 InvocationHandler 接口，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成指定类的一个子类对象，并覆盖其中特定方法并添加增强代码，从而实现AOP。CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。

```



### 3.什么是 ioc？

```
    IoC就是(Inversion of Control)，控制反转。在Java开发中，IoC意味着将你设计好的类交给系统去控制，而不是在你的类内部控制。这称为控制反转。

  Spring通过一种称作控制反转（IoC）的技术促进了松耦合。当应用了IoC，一个对象依赖的其它对象会通过被动的方式传递进来，而不是这个对象自己创建或者查找依赖对象。你可以认为IoC与JNDI相反——不是对象从容器中查找依赖，而是容器在对象初始化时不等对象请求就主动将依赖传递给它。
  
  Spring的IOC有三种注入方式 ：构造器注入、setter方法注入、根据注解注入。
```



### 4.spring 有哪些主要模块？

**Spring有七大模块组成：**

![img](https://img-blog.csdnimg.cn/20190412160350745.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE2NjU5OTE=,size_16,color_FFFFFF,t_70)

```
- 核心容器（Spring Core）

　　核心容器提供Spring框架的基本功能。Spring以bean的方式组织和管理Java应用中的各个组件及其关系。Spring使用BeanFactory来产生和管理Bean，它是工厂模式的实现。BeanFactory使用控制反转(IoC)模式将应用的配置和依赖性规范与实际的应用程序代码分开。

- 应用上下文（Spring Context）

　　Spring上下文是一个配置文件，向Spring框架提供上下文信息。Spring上下文包括企业服务，如JNDI、EJB、电子邮件、国际化、校验和调度功能。

- Spring面向切面编程（Spring AOP）

　　通过配置管理特性，Spring AOP 模块直接将面向方面的编程功能集成到了 Spring框架中。所以，可以很容易地使 Spring框架管理的任何对象支持 AOP。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖 EJB 组件，就可以将声明性事务管理集成到应用程序中。

- JDBC和DAO模块（Spring DAO）

　　JDBC、DAO的抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理，和不同数据库供应商所抛出的错误信息。异常层次结构简化了错误处理，并且极大的降低了需要编写的代码数量，比如打开和关闭链接。

- 对象实体映射（Spring ORM）

　　Spring框架插入了若干个ORM框架，从而提供了ORM对象的关系工具，其中包括了Hibernate、JDO和 IBatis SQL Map等，所有这些都遵从Spring的通用事物和DAO异常层次结构。

- Web模块（Spring Web）

　　Web上下文模块建立在应用程序上下文模块之上，为基于web的应用程序提供了上下文。所以Spring框架支持与Struts集成，web模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。

- MVC模块（Spring Web MVC）

　　MVC框架是一个全功能的构建Web应用程序的MVC实现。通过策略接口，MVC框架变成为高度可配置的。MVC容纳了大量视图技术，其中包括JSP、POI等，模型来有JavaBean来构成，存放于m当中，而视图是一个街口，负责实现模型，控制器表示逻辑代码，由c的事情。Spring框架的功能可以用在任何J2EE服务器当中，大多数功能也适用于不受管理的环境。Spring的核心要点就是支持不绑定到特定J2EE服务的可重用业务和数据的访问的对象，毫无疑问这样的对象可以在不同的J2EE环境，独立应用程序和测试环境之间重用。
```



### 5.spring 常用的注解？

```
- @Configuration	把一个类作为一个IoC容器，它的某个方法头上如果注册了@Bean，就会作为这个Spring容器中的Bean。
- @Scope			作用域
- @Lazy(true) 		表示延迟初始化
- @Service			用于标注业务层组件、
- @Controller		用于标注控制层组件（如struts中的action）
- @Repository		用于标注数据访问组件，即DAO组件。
- @Component		泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。
- @Scope			用于指定scope作用域的（用在类上）
- @PostConstruct	用于指定初始化方法（用在方法上）
- @PreDestory		用于指定销毁方法（用在方法上）
- @DependsOn 		定义Bean初始化及销毁时的顺序
- @Primary 			自动装配时当出现多个Bean候选者时，被注解为@Primary的Bean将作为首选者，否则将抛出异常
- @Autowired 		默认按类型装配，如果我们想使用按名称装配，可以结合@Qualifier注解一起使用。如下：
- @Autowired @Qualifier("personDaoBean") 存在多个实例配合使用
- @Resource			默认按名称装配，当找不到与名称匹配的bean才会按类型装配。
- @PostConstruct 	初始化注解
- @PreDestroy 		摧毁注解 默认 单例 启动就加载
- @Async			异步方法调用
```



### 6.spring 中的 bean 是线程安全的吗？

```
Spring框架并没有对单例bean进行任何多线程的封装处理。关于单例bean的线程安全和并发问题需要开发者自行去搞定。但实际上，大部分的Spring bean并没有可变的状态(比如Serview类和DAO类)，所以在某种程度上说Spring的单例bean是线程安全的。如果你的bean有多种状态的话（比如 View Model 对象），就需要自行保证线程安全。

最浅显的解决办法就是将多态bean的作用域由“singleton”变更为“prototype”。

多线程操作单例bean，存在线程安全问题。 
因此创建ThreadLocal变量，将需要成员变量存储在ThreadLocal中
```

### 7.spring 支持几种 bean 的作用域？

```
当通过spring容器创建一个Bean实例时，不仅可以完成Bean实例的实例化，还可以为Bean指定特定的作用域。Spring支持如下5种作用域：

- singleton：单例模式，在整个Spring IoC容器中，使用singleton定义的Bean将只有一个实例
- prototype：原型模式，每次通过容器的getBean方法获取prototype定义的Bean时，都将产生一个新的Bean实例
- request：对于每次HTTP请求，使用request定义的Bean都将产生一个新实例，
			即每次HTTP请求将会产生不同的Bean实例。只有在Web应用中使用Spring时，该作用域才有效
- session：对于每次HTTP Session，使用session定义的Bean豆浆产生一个新实例。
			同样只有在Web应用中使用Spring时，该作用域才有效
- globalsession：每个全局的HTTP Session，使用session定义的Bean都将产生一个新实例。
			典型情况下，仅在使用portlet context的时候有效。
			同样只有在Web应用中使用Spring时，该作用域才有效

　　其中比较常用的是singleton和prototype两种作用域。对于singleton作用域的Bean，每次请求该Bean都将获得相同的实例。容器负责跟踪Bean实例的状态，负责维护Bean实例的生命周期行为；如果一个Bean被设置成prototype作用域，程序每次请求该id的Bean，Spring都会新建一个Bean实例，然后返回给程序。在这种情况下，Spring容器仅仅使用new 关键字创建Bean实例，一旦创建成功，容器不在跟踪实例，也不会维护Bean实例的状态。

　　如果不指定Bean的作用域，Spring默认使用singleton作用域。Java在创建Java实例时，需要进行内存申请；销毁实例时，需要完成垃圾回收，这些工作都会导致系统开销的增加。因此，prototype作用域Bean的创建、销毁代价比较大。而singleton作用域的Bean实例一旦创建成功，可以重复使用。因此，除非必要，否则尽量避免将Bean被设置成prototype作用域。
```

### 8.spring 自动装配 bean 有哪些方式？

```
　Spring中bean有三种装配机制，分别是：
	1. 在xml中显示配置；
	2. 在java中显示配置；
	3. 隐式的bean发现机制和自动装配。
```



### 9.spring 事务实现方式有哪些？

```
事务：事务逻辑上的一组操作,组成这组操作的各个逻辑单元,要么一起成功,要么一起失败.比如，保证数据的运行不会说A给B钱,A钱给了B却没收到。

实现事务的三种方式：

- 1.aspectJ AOP实现事务：
- 2.事务代理工厂Bean实现事务：
- 3.注解方式实现事务：

在需要进行事务的方法上增加一个注解“@Transactional(rollbackFor = MyExepction.class )”
```

### 10. spring 的事务隔离？

```
- 事务特性（4种）:
  - 原子性 （atomicity）:强调事务的不可分割.
  - 一致性 （consistency）:事务的执行的前后数据的完整性保持一致.
  - 隔离性 （isolation）:一个事务执行的过程中,不应该受到其他事务的干扰
  - 持久性（durability） :事务一旦结束,数据就持久到数据库

- 如果不考虑隔离性引发安全性问题:
  - 脏读 :一个事务读到了另一个事务的未提交的数据
  - 不可重复读 :一个事务读到了另一个事务已经提交的 update 的数据导致多次查询结果不一致.
  - 虚幻读 :一个事务读到了另一个事务已经提交的 insert 的数据导致多次查询结果不一致.

- 解决读问题: 设置事务隔离级别（5种）
  - DEFAULT 这是一个PlatfromTransactionManager默认的隔离级别，使用数据库默认的事务隔离级别.
  - 未提交读（read uncommited） :脏读，不可重复读，虚读都有可能发生
  - 已提交读 （read commited）:避免脏读。但是不可重复读和虚读有可能发生
  - 可重复读 （repeatable read） :避免脏读和不可重复读.但是虚读有可能发生.
  - 串行化的 （serializable） :避免以上所有读问题.
  - Mysql 默认:可重复读
  - Oracle 默认:读已提交
  
1. read uncommited：是最低的事务隔离级别，它允许另外一个事务可以看到这个事务未提交的数据。
2. read commited：保证一个事物提交后才能被另外一个事务读取。另外一个事务不能读取该事物未提交的数据。
3. repeatable read：这种事务隔离级别可以防止脏读，不可重复读。但是可能会出现幻象读。
	它除了保证一个事务不能被另外一个事务读取未提交的数据之外还避免了以下情况产生（不可重复读）。
4. serializable：这是花费最高代价但最可靠的事务隔离级别。事务被处理为顺序执行。
	除了防止脏读，不可重复读之外，还避免了幻象读（避免三种）。

```

### 11.Spring事务的传播

```
事务的传播行为（7种）
	PROPAGION_XXX :事务的传播行为
  - 保证同一个事务中
  - PROPAGATION_REQUIRED 支持当前事务，如果不存在 就新建一个(默认)
  - PROPAGATION_SUPPORTS 支持当前事务，如果不存在，就不使用事务
  - PROPAGATION_MANDATORY 支持当前事务，如果不存在，抛出异常
  - 保证没有在同一个事务中
  - PROPAGATION_REQUIRES_NEW 如果有事务存在，挂起当前事务，创建一个新的事务
  - PROPAGATION_NOT_SUPPORTED 以非事务方式运行，如果有事务存在，挂起当前事务
  - PROPAGATION_NEVER 以非事务方式运行，如果有事务存在，抛出异常
  - PROPAGATION_NESTED 如果当前事务存在，则嵌套事务执行
```



### 12.@RestController @Controller
```
@Controller 返回页面,
@RestController = @ResponseBody + @Controller 
	将JSON数据 || XML数据存储在HttpResponse的实体中返回 
```


### 13.@Component 和 @ Bean
```
    1. @Component 作用于类， @ Bean 作用于方法，必须在【配置类】中使用
    2. @Bean 比 @Component 自定义性强，可以装配第三方库的类
   
```

### 14.Spring生命周期
```
生命周期
    1. 容器 根据【配置文件】 【读取】Resource（Class类文件信息）
    2. 解析【Resource】--->获得BeanDefinition(含有创建bean实例的信息)
    3. 根据BeanDefinition信息-->调用 Java Reflection API --> 创建Bean实例
    4. Set方法设置属性值
    5. 如果 Bean 实现一系列Aware接口，则调用相应的方法。
        Aware接口 = {
            1. BeanNameAware    // 传入 String实例
            2. BeanFactoryAware // 传入 工厂实例
            3. BeanClassLoaderAware  // 传入ClassLoader实例
        }
    6. 如果 加载beanPorstProcessor对象，则会在【初始化前】，调用postProcessorBeforeInitialization()
    7. 如果 bean 实现 IntializationBean接口，则调用afterPropertiesSet方法
    8. 如果 配置文件 指定 bean的 init—Method【属性】，则执行指定方法
    9. beanPostProcess() 【初始化后】，则调用postProcessAfterInitialization()
    10. 销毁bean时，
        如果bean实现 DispoableBean 接口 ，则执行destory()方法
        如果配置文件 指定bean的destory-method属性，则执行指定方法 
    
```




### 15.Spring如何解决循环依赖
```
构造器(初始化与赋值没法分开)与prototype(没有实现三级缓存)会报错

三级缓存分别为
1.初始化完成的bean（singletonObjects）
2.实例化的bean（尚未绑定属性，earlySingletonObjects）
3.beanfactory（singletonFactories）

比如有两个beanA和B循环依赖
在A的实例化阶段标记，将自己曝光到第三级缓存中，发现自己依赖B，去初始化B，B初始化过程中发现自己依赖A，从第三级缓存中getObject拿到A（注意此时A只是实例化完成，并没有初始化），此时B顺利进行初始化，将自己放到一级缓存中，此时返回A中，A顺利拿到B，完成了初始化阶段，放到了一级缓存。

Spring首先从singletonObjects（一级缓存）中尝试获取，
如果获取不到并且对象在创建中，则尝试从earlySingletonObjects(二级缓存)中获取，
如果还是获取不到并且允许从singletonFactories通过getObject获取，则通过singletonFactory.getObject()(三级缓存)获取。
如果获取到了则将singletonObject放入到earlySingletonObjects,也就是 将三级缓存提升到二级缓存中


```

### 16.BeanFactory和ApplicationContext？

```
    BeanFactory和ApplicationContext是Spring的两大核心接口，都可以当做Spring的容器。其中ApplicationContext是BeanFactory的子接口。

（1）BeanFactory：是Spring里面最底层的接口，包含了各种Bean的定义，读取bean配置文档，管理bean的加载、实例化，控制bean的生命周期，维护bean之间的依赖关系。ApplicationContext接口作为BeanFactory的派生，除了提供BeanFactory所具有的功能外，还提供了更完整的框架功能：

    ①继承MessageSource，因此支持国际化。
    ②统一的资源文件访问方式。
    ③提供在监听器中注册bean的事件。
    ④同时加载多个配置文件。
    ⑤载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层。

（2）①BeanFactroy采用的是延迟加载形式来注入Bean的，即只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化。这样，我们就不能发现一些存在的Spring的配置问题。如果Bean的某一个属性没有注入，BeanFacotry加载后，直至第一次使用调用getBean方法才会抛出异常。
	②ApplicationContext，它是在容器启动时，一次性创建了所有的Bean。这样，在容器启动时，我们就可以发现Spring中存在的配置错误，这样有利于检查所依赖属性是否注入。 ApplicationContext启动后预载入所有的单实例Bean，通过预载入单实例bean ,确保当你需要的时候，你就不用等待，因为它们已经创建好了。
	③相对于基本的BeanFactory，ApplicationContext 唯一的不足是占用内存空间。当应用程序配置Bean较多时，程序启动较慢。

（3）BeanFactory通常以编程的方式被创建，ApplicationContext还能以声明的方式创建，如使用ContextLoader。

（4）BeanFactory和ApplicationContext都支持BeanPostProcessor、BeanFactoryPostProcessor的使用，但两者之间的区别是：BeanFactory需要手动注册，而ApplicationContext则是自动注册。
```

### 17.Spring的自动装配：

```
在spring中，对象无需自己查找或创建与其关联的其他对象，由容器负责把需要相互协作的对象引用赋予各个对象，使用autowire来配置自动装载模式。

--在Spring框架xml配置中共有5种自动装配：
	（1）no：默认的方式是不进行自动装配的，通过手工设置ref属性来进行装配bean。
	（2）byName：通过bean的名称进行自动装配，如果一个bean的property与另一bean 的name 相同，就进行自动装配。 
	（3）byType：通过参数的数据类型进行自动装配。
	（4）constructor：利用构造函数进行装配，并且构造函数的参数通过byType进行装配。
	（5）autodetect：自动探测，如果有构造方法，通过 construct的方式自动装配，否则使用 byType的方式自动装配。

--基于注解的方式：

使用@Autowired注解来自动装配指定的bean。在使用@Autowired注解之前需要在Spring配置文件进行配置，<context:annotation-config />。在启动spring IoC时，容器自动装载了一个AutowiredAnnotationBeanPostProcessor后置处理器，当容器扫描到@Autowied、@Resource或@Inject时，就会在IoC容器自动查找需要的bean，并装配给该对象的属性。在使用@Autowired时，首先在容器中查询对应类型的bean：

如果查询结果刚好为一个，就将该bean装配给@Autowired指定的数据；
如果查询的结果不止一个，那么@Autowired会根据名称来查找；
如果上述查找的结果为空，那么会抛出异常。解决方法时，使用required=false。

@Autowired可用于：构造函数、成员变量、Setter方法

注：@Autowired和@Resource之间的区别
(1) @Autowired默认是按照类型装配注入的，默认情况下它要求依赖对象必须存在（可以设置它required属性为false）。
(2) @Resource默认是按照名称来装配注入的，只有当找不到与名称匹配的bean才会按照类型来装配注入。
```



 

### 18.Spring 框架中设计模式？

```
（1）工厂模式：BeanFactory就是简单工厂模式的体现，用来创建对象的实例；
（2）单例模式：Bean默认为单例模式。
（3）代理模式：Spring的AOP功能用到了JDK的动态代理和CGLIB字节码生成技术；
（4）模板方法：用来解决代码重复的问题。比如. RestTemplate, JmsTemplate, JpaTemplate。
（5）观察者模式：定义对象键一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都会得到通知被制动更新，如Spring中listener的实现--ApplicationListener。
```

 

### 19.Spring事务的种类：

```
spring支持编程式事务管理和声明式事务管理两种方式：

①编程式事务管理使用TransactionTemplate。

②声明式事务管理建立在AOP之上的。其本质是通过AOP功能，对方法前后进行拦截，将事务处理的功能编织到拦截的方法中，也就是在目标方法开始之前加入一个事务，在执行完目标方法之后根据执行情况提交或者回滚事务。

声明式事务最大的优点就是不需要在业务逻辑代码中掺杂事务管理的代码，只需在配置文件中做相关的事务规则声明或通过@Transactional注解的方式，便可以将事务规则应用到业务逻辑中。

声明式事务管理要优于编程式事务管理，这正是spring倡导的非侵入式的开发方式，使业务代码不受污染，只要加上注解就可以获得完全的事务支持。唯一不足地方是，最细粒度只能作用到方法级别，无法做到像编程式事务那样可以作用到代码块级别。
```



### 20.Spring框架中有哪些不同类型的事件？

```
Spring 提供了以下5种标准的事件：

（1）上下文更新事件（ContextRefreshedEvent）：在调用ConfigurableApplicationContext 接口中的refresh()方法时被触发。

（2）上下文开始事件（ContextStartedEvent）：当容器调用ConfigurableApplicationContext的Start()方法开始/重新开始容器时触发该事件。

（3）上下文停止事件（ContextStoppedEvent）：当容器调用ConfigurableApplicationContext的Stop()方法停止容器时触发该事件。

（4）上下文关闭事件（ContextClosedEvent）：当ApplicationContext被关闭时触发该事件。容器被关闭时，其管理的所有单例Bean都被销毁。

（5）请求处理事件（RequestHandledEvent）：在Web应用中，当一个http请求（request）结束触发该事件。

如果一个bean实现了ApplicationListener接口，当一个ApplicationEvent 被发布以后，bean会自动被通知。
```

 

### 21.Spring AOP 名词

```
（1）切面（Aspect）：被抽取的公共模块，可能会横切多个对象。 在Spring AOP中，切面可以使用通用类（基于模式的风格） 或者在普通类中以 @AspectJ 注解来实现。

（2）连接点（Join point）：指方法，在Spring AOP中，一个连接点 总是 代表一个方法的执行。 

（3）通知（Advice）：在切面的某个特定的连接点（Join point）上执行的动作。通知有各种类型，其中包括“around”、“before”和“after”等通知。许多AOP框架，包括Spring，都是以拦截器做通知模型， 并维护一个以连接点为中心的拦截器链。

（4）切入点（Pointcut）：切入点是指 我们要对哪些Join point进行拦截的定义。通过切入点表达式，指定拦截的方法，比如指定拦截add*、search*。

（5）引入（Introduction）：（也被称为内部类型声明（inter-type declaration））。声明额外的方法或者某个类型的字段。Spring允许引入新的接口（以及一个对应的实现）到任何被代理的对象。例如，你可以使用一个引入来使bean实现 IsModified 接口，以便简化缓存机制。

（6）目标对象（Target Object）： 被一个或者多个切面（aspect）所通知（advise）的对象。也有人把它叫做 被通知（adviced） 对象。 既然Spring AOP是通过运行时代理实现的，这个对象永远是一个 被代理（proxied） 对象。

（7）织入（Weaving）：指把增强应用到目标对象来创建新的代理对象的过程。Spring是在运行时完成织入。

切入点（pointcut）和连接点（join point）匹配的概念是AOP的关键，这使得AOP不同于其它仅仅提供拦截功能的旧技术。 切入点使得定位通知（advice）可独立于OO层次。 例如，一个提供声明式事务管理的around通知可以被应用到一组横跨多个对象中的方法上（例如服务层的所有业务操作）。
```



### 22.Spring通知有哪些类型？

```
（1）前置通知（Before advice）：在某连接点（join point）之前执行的通知，但这个通知不能阻止连接点前的执行（除非它抛出一个异常）。

（2）返回后通知（After returning advice）：在某连接点（join point）正常完成后执行的通知：例如，一个方法没有抛出任何异常，正常返回。 

（3）抛出异常后通知（After throwing advice）：在方法抛出异常退出时执行的通知。 

（4）后通知（After (finally) advice）：当某连接点退出的时候执行的通知（不论是正常返回还是异常退出）。 

（5）环绕通知（Around Advice）：包围一个连接点（join point）的通知，如方法调用。这是最强大的一种通知类型。 环绕通知可以在方法调用前后完成自定义的行为。它也会选择是否继续执行连接点或直接返回它们自己的返回值或抛出异常来结束执行。 环绕通知是最常用的一种通知类型。大部分基于拦截的AOP框架，例如Nanning和JBoss4，都只提供环绕通知。 
```

