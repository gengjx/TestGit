### spring boot 有哪些方式可以实现热部署？

SpringBoot热部署方式一共有两种，分别使用两种不同的依赖

　　SpringBoot 1.3后才拥有SpringBoot devtools热部署

　　①：spring-boot-devtools  ②：Spring Loaded

- 方式一： 在项目的pom文件中添加依赖:

```java
<!--热部署jar--><dependency>    <groupId>org.springframework.boot</groupId>    <artifactId>spring-boot-devtools</artifactId></dependency>
```

- 方式二： 在项目中添加如下代码

```java
<build>        <plugins>            <plugin>                <!-- springBoot编译插件-->                <groupId>org.springframework.boot</groupId>                <artifactId>spring-boot-maven-plugin</artifactId>                <dependencies>                    <!-- spring热部署 -->                    <!-- 该依赖在此处下载不下来，可以放置在build标签外部下载完成后再粘贴进plugin中 -->                    <dependency>                        <groupId>org.springframework</groupId>                        <artifactId>springloaded</artifactId>                        <version>1.2.6.RELEASE</version>                    </dependency>                </dependencies>            </plugin>        </plugins>    </build>
```

添加完毕后需要使用mvn指令运行：

　　　　首先找到IDEA中的Edit configurations ,然后进行如下操作：（点击左上角的"+",然后选择maven将出现右侧面板，在红色划线部位输入如图所示指令，你可以为该指令命名(此处命名为MvnSpringBootRun)）

![img](https://img-blog.csdnimg.cn/20190417161553893.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE2NjU5OTE=,size_16,color_FFFFFF,t_70)

　　　　点击保存将会在IDEA项目运行部位出现，点击绿色箭头运行即可

![img](https://img-blog.csdnimg.cn/20190417161613725.png)

### jpa 和 hibernate 有什么区别？

```
简单的可以理解为：

- Hibernate是JPA规范的一个具体实现
- hibernate有JPA没有的特性 
- hibernate 的效率更快
- JPA 有更好的移植性，通用性
```



## 6、开启SpringBoot特性有哪几种方式？

> 1）`继承spring-boot-starter-parent项目`

```xml
<parent>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-parent</artifactId>
   <version>1.5.6.RELEASE</version>
</parent>
12345
```

> ```
> 2）导入spring-boot-dependencies项目依赖
> ```

```xml
<dependencyManagement>
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-dependencies</artifactId>
           <version>1.5.6.RELEASE</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
</dependencyManagement>
12345678910
```

> This only works if your Maven project inherits (directly or indirectly) from spring-boot-dependencies. If you have added spring-boot-dependencies in your own dependencyManagement section with import you have to redefine the artifact yourself instead of overriding the property.

Spring Boot依赖包里面的组件的版本都是和当前Spring Boot绑定的，如果要修改里面组件的版本，只需要添加如下属性覆盖即可，但这种方式只对继承有效，导入的方式无效。

```xml
<properties>
   <slf4j.version>1.7.25<slf4j.version>
</properties>
123
```

如果导入的方式要实现版本的升级，达到上面的效果，这样也可以做到，把要升级的组件依赖放到Spring Boot之前。

```xml
<dependencyManagement>
   <dependencies>
       <!-- Override Spring Data release train provided by Spring Boot -->
       <dependency>
           <groupId>org.springframework.data</groupId>
           <artifactId>spring-data-releasetrain</artifactId>
           <version>Fowler-SR2</version>
           <scope>import</scope>
           <type>pom</type>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-dependencies</artifactId>
           <version>1.5.6.RELEASE</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
   </dependencies>
</dependencyManagement>
12345678910111213141516171819
```

------

## 7、SpringBoot需要独立的容器运行吗？

> 可以不需要，`内置了 Tomcat/ Jetty 等容器。`

------

## 8、运行SpringBoot有哪几种方式？

```
1）打包用命令或者放到容器中运行
2）用 Maven/ Gradle 插件运行
3）直接执行 main 方法运行
```

------

## 9、Spring Boot 自动配置原理是什么？

> 注解 @EnableAutoConfiguration, @Configuration, @ConditionalOnClass 就是自动配置的核心，首先它得是一个配置文件，其次根据类路径下是否有这个类去自动配置。

Spring Boot的自动配置注解是@EnableAutoConfiguration， 从上面的@Import的类可以找到下面自动加载自动配置的映射。

```java
 org.springframework.core.io.support.SpringFactoriesLoader.loadFactoryNames(Class<?>, ClassLoader)
1
public static List<String> loadFactoryNames(Class<?> factoryClass, ClassLoader classLoader) {
  String factoryClassName = factoryClass.getName();
  try {
     Enumeration<URL> urls = (classLoader != null ? classLoader.getResources(FACTORIES_RESOURCE_LOCATION) :
              lassLoader.getSystemResources(FACTORIES_RESOURCE_LOCATION));
      List<String> result = new ArrayList<String>();
      while (urls.hasMoreElements()) {
          URL url = urls.nextElement();
          Properties properties = PropertiesLoaderUtils.loadProperties(new UrlResource(url));
          String factoryClassNames = properties.getProperty(factoryClassName);
          result.addAll(Arrays.asList(StringUtils.commaDelimitedListToStringArray(factoryClassNames)));
      }
      return result;
  }
  catch (IOException ex) {
      throw new IllegalArgumentException("Unable to load [" + factoryClass.getName() +
              "] factories from location [" + FACTORIES_RESOURCE_LOCATION + "]", ex);
  }

}
1234567891011121314151617181920
```

> 这个方法会加载类路径及所有jar包下META-INF/spring.factories配置中映射的自动配置的类。

```java
/**
* The location to look for factories.
* <p>Can be present in multiple JAR files.
*/
public static final String FACTORIES_RESOURCE_LOCATION = "META-INF/spring.factories";
查看Spring Boot自带的自动配置的包： spring-boot-autoconfigure-1.5.6.RELEASE.jar，打开其中的META-INF/spring.factories文件会找到自动配置的映射。

org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
org.springframework.boot.autoconfigure.cloud.CloudAutoConfiguration,\
...
12345678910111213141516
```

> 再来看看数据源自动配置的实现注解

```java
@Configuration
@ConditionalOnClass({ DataSource.class, EmbeddedDatabaseType.class })
@EnableConfigurationProperties(DataSourceProperties.class)
@Import({ Registrar.class, DataSourcePoolMetadataProvidersConfiguration.class })
public class DataSourceAutoConfiguration {
...
@Configuration,@ConditionalOnClass就是自动配置的核心，首先它得是一个配置文件，其次根据类路径下是否有这个类去自动配置。
1234567
```

------

## 10、Spring Boot 的目录结构是怎样的？

```java
cn
 +- javastack
     +- MyApplication.java
     |
     +- customer
     |   +- Customer.java
     |   +- CustomerController.java
     |   +- CustomerService.java
     |   +- CustomerRepository.java
     |
     +- order
         +- Order.java
         +- OrderController.java
         +- OrderService.java
         +- OrderRepository.java
123456789101112131415
```

> `这个目录结构是主流及推荐的做法，而在主入口类上加上 @SpringBootApplication 注解来开启 Spring Boot 的各项能力，如自动配置、组件扫描等。`。

------

## 11、你如何理解 Spring Boot 中的 Starters？

> Starters可以理解为启动器，它包含了一系列可以集成到应用里面的依赖包，你可以一站式集成 Spring 及其他技术，而不需要到处找示例代码和依赖包。如你想使用 Spring JPA 访问数据库，只要加入 spring-boot-starter-data-jpa 启动器依赖就能使用了。

> Starters包含了许多项目中需要用到的依赖，它们能快速持续的运行，都是一系列得到支持的管理传递性依赖。具体请看这篇文章《Spring Boot Starters启动器》。

------

## 12、如何在 Spring Boot 启动的时候运行一些特定的代码？

可以实现接口 ApplicationRunner 或者 CommandLineRunner，这两个接口实现方式一样，它们都只提供了一个 run 方法，具体请看这篇文章《Spring Boot Runner启动器》。

------

## 13、Spring Boot 有哪几种读取配置的方式？

Spring Boot 可以通过 @PropertySource,@Value,@Environment, @ConfigurationProperties 来绑定变量，具体请看这篇文章《Spring Boot读取配置的几种方式》。

```
读取application文件
```

在application.yml或者properties文件中添加：

info.address=USA
info.company=Spring
info.degree=high

```
@Value注解读取方式
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
 
@Component
public class InfoConfig1 {
 
   @Value("${info.address}")
   private String address;
 
   @Value("${info.company}")
   private String company;
 
   @Value("${info.degree}")
   private String degree;
 
   public String getAddress() {
       return address;
   }
 
   public void setAddress(String address) {
       this.address = address;
   }
 
   public String getCompany() {
       return company;
   }
 
   public void setCompany(String company) {
       this.company = company;
   }
 
   public String getDegree() {
       return degree;
   }
 
   public void setDegree(String degree) {
       this.degree = degree;
   }
 
}
12345678910111213141516171819202122232425262728293031323334353637383940
@ConfigurationProperties注解读取方式
@Component
@ConfigurationProperties(prefix = "info")
public class InfoConfig2 {
 
   private String address;
   private String company;
   private String degree;
 
   public String getAddress() {
       return address;
   }
 
   public void setAddress(String address) {
       this.address = address;
   }
 
   public String getCompany() {
       return company;
   }
 
   public void setCompany(String company) {
       this.company = company;
   }
 
   public String getDegree() {
       return degree;
   }
 
   public void setDegree(String degree) {
       this.degree = degree;
   }
 
}
123456789101112131415161718192021222324252627282930313233
```

`读取指定文件`
资源目录下建立config/db-config.properties:

```java
db.username=root
db.password=123456
@PropertySource+@Value注解读取方式

@Component
@PropertySource(value = { "config/db-config.properties" })
public class DBConfig1 {
 
   @Value("${db.username}")
   private String username;
 
   @Value("${db.password}")
   private String password;
 
   public String getUsername() {
       return username;
   }
 
   public void setUsername(String username) {
       this.username = username;
   }
 
   public String getPassword() {
       return password;
   }
 
   public void setPassword(String password) {
       this.password = password;
   }
 
}
注意：@PropertySource不支持yml文件读取。
1234567891011121314151617181920212223242526272829303132
@PropertySource+@ConfigurationProperties注解读取方式
@Component
@ConfigurationProperties(prefix = "db")
@PropertySource(value = { "config/db-config.properties" })
public class DBConfig2 {
 
   private String username;
   private String password;
 
   public String getUsername() {
       return username;
   }
 
   public void setUsername(String username) {
       this.username = username;
   }
 
   public String getPassword() {
       return password;
   }
 
   public void setPassword(String password) {
       this.password = password;
   }
 
}
12345678910111213141516171819202122232425
Environment读取方式 以上所有加载出来的配置都可以通过Environment注入获取到。
@Autowired
private Environment env;
 
// 获取参数
String getProperty(String key);
```



## 14、Spring Boot 支持哪些日志框架？推荐和默认的日志框架是哪个？

```
Spring Boot 支持 Java Util Logging, Log4j2, Lockback 作为日志框架，如果你使用 Starters 启动器，Spring Boot 将使用 Logback 作为默认日志框架，Spring Boot支持Java Util Logging,Log4j2,Lockback作为日志框架，如果你使用starters启动器，Spring Boot将使用Logback作为默认日志框架。无论使用哪种日志框架，Spring Boot都支持配置将日志输出到控制台或者文件中。
spring-boot-starter启动器包含spring-boot-starter-logging启动器并集成了slf4j日志抽象及Logback日志框架。
```

------

## 15、SpringBoot 实现热部署有哪几种方式？

> 主要有两种方式：

```
Spring Loaded
Spring-boot-devtools
```

## 17、Spring Boot 如何定义多套不同环境配置？

提供多套配置文件，如：

> applcation.properties
>
> > application-dev.properties
> > application-test.properties
> > application-prod.properties

------

## 18、Spring Boot 可以兼容老 Spring 项目吗，如何做？

> ```
> 可以兼容，使用 @ImportResource 注解导入老 Spring 项目配置文件。
> ```

## 19、保护 Spring Boot 应用有哪些方法？

1. 在生产中使用HTTPS
2. 使用Snyk检查你的依赖关系
3. 升级到最新版本
4. 启用CSRF保护
5. 使用内容安全策略防止XSS攻击

------

## 20、Spring Boot 2.X 有什么新特性？与 1.X 有什么区别？

```
1. 配置变更
2. JDK 版本升级
3. 第三方类库升级
4. 响应式 Spring 编程支持
5. HTTP/2 支持
6. 配置属性绑定
```

### 



## 概述

### 什么是 Spring Boot？

```
Spring Boot 是 Spring 开源组织下的子项目，是 Spring 组件一站式解决方案，主要是简化了使用 Spring 的难度，简省了繁重的配置，提供了各种启动器，开发者能快速上手。

SpringBoot是一个框架，一种全新的编程规范，他的产生简化了框架的使用，所谓简化是指简化了Spring众多框架中所需的大量且繁琐的配置文件，所以 SpringBoot是一个服务于框架的框架，服务范围是简化配置文件
```



### Spring Boot 有哪些优点？

```
Spring Boot 主要有如下优点：

1. 容易上手，提升开发效率，为 Spring 开发提供一个更快、更广泛的入门体验。
2. 开箱即用，远离繁琐的配置。
3. 提供了一系列大型项目通用的非业务性功能，例如：内嵌服务器、安全管理、运行数据监控、运行状况检查和外部化配置等。
4. 没有代码生成，也不需要XML配置。
5. 避免大量的 Maven 导入和各种版本冲突。
------------------------------------------
最明显的特点是，让文件配置变的相当简单、让应用部署变的简单（SpringBoot内置服务器，并装备启动类代码），可以快速开启一个Web容器进行开发

-  （1）一个简单的SpringBoot工程是不需要在pom.xml手动添加什么配置的，如果与其他技术合用 比如postMan（文档在线自动生成、开发功能测试的一套工具）、Swagger(文档在线自动生成、开发功能测试的一套工具)，则需要在pom.xml中添加依赖，由程序自动加载依赖jar包等配置文件。
-  （2）我们之前在利用SSM或者SSH开发的时候，在resources中储存各种对应框架的配置文件，而现在我们只需要一个配置文件即可，配置内容也大体有 服务器端口号、数据库连接的地址、用户名、密码。这样，虽然简单 但在一定问题上而言，这也是极不安全的，将所有配置，放在一个文件里，是很危险的，但对于一般项目而言并不会有太大影响。
-   （3）在SpringBoot创建时会自动创建Bootdemo1Application启动类,代表着本工程项目和服务器的启动加载，在springBoot中是内含服务器的，所以不需手动配置Tomact，但注意端口号冲突问题。
```



### Spring Boot 的核心注解？

```
@SpringBootApplication，它是 Spring Boot 的核心注解，主要组合包含了以下 3 个注解：

@SpringBootConfiguration：组合了 @Configuration 注解，实现配置文件的功能。
@EnableAutoConfiguration：打开自动配置的功能，也可以关闭某个自动配置的选项，如关闭数据源自动配置功能： 	@SpringBootApplication(exclude = { DataSourceAutoConfiguration.class })。
@ComponentScan：Spring组件扫描。
```



## 配置

### 什么是 JavaConfig？

```
Spring JavaConfig 是 Spring 社区的产品，它提供了配置 Spring IoC 容器的纯Java 方法。因此它有助于避免使用 XML 配置。使用 JavaConfig 的优点在于：

（1）面向对象的配置。由于配置被定义为 JavaConfig 中的类，因此用户可以充分利用 Java 中的面向对象功能。一个配置类可以继承另一个，重写它的@Bean 方法等。

（2）减少或消除 XML 配置。基于依赖注入原则的外化配置的好处已被证明。但是，许多开发人员不希望在 XML 和 Java 之间来回切换。JavaConfig 为开发人员提供了一种纯 Java 方法来配置与 XML 配置概念相似的 Spring 容器。从技术角度来讲，只使用 JavaConfig 配置类来配置容器是可行的，但实际上很多人认为将JavaConfig 与 XML 混合匹配是理想的。

（3）类型安全和重构友好。JavaConfig 提供了一种类型安全的方法来配置 Spring容器。由于 Java 5.0 对泛型的支持，现在可以按类型而不是按名称检索 bean，不需要任何强制转换或基于字符串的查找。
```



### --Spring Boot 自动配置原理？

```
在 Spring Boot 里面，可以使用以下几种方式来加载配置。

1）properties文件；

2）YAML文件；

3）系统环境变量；

4）命令行参数；

配置属性加载的顺序如下：
1、开发者工具 Devtools 全局配置参数；
2、单元测试上的 @TestPropertySource 注解指定的参数；
3、单元测试上的 @SpringBootTest 注解指定的参数；
4、命令行指定的参数，如 java -jar springboot.jar --name="Java技术栈"；
5、命令行中的 SPRING_APPLICATION_JSON 指定参数, 如 java -Dspring.application.json='{"name":"Java技术栈"}' -jar springboot.jar
6、ServletConfig 初始化参数；
7、ServletContext 初始化参数；
8、JNDI参数（如 java:comp/env/spring.application.json）；
9、Java系统参数（来源：System.getProperties()）；
10、操作系统环境变量参数；
11、RandomValuePropertySource 随机数，仅匹配：ramdom.*；
12、JAR包外面的配置文件参数（application-{profile}.properties（YAML））
13、JAR包里面的配置文件参数（application-{profile}.properties（YAML））
14、JAR包外面的配置文件参数（application.properties（YAML））
15、JAR包里面的配置文件参数（application.properties（YAML））
16、@Configuration配置文件上 @PropertySource 注解加载的参数；
17、默认参数（通过 SpringApplication.setDefaultProperties 指定）；
```

1、在主应用程序中添加 Java 系统参数。

```java
@Bean
public CommandLineRunner commandLineRunner() {
    return (args) -> {
        System.setProperty("name", "javastack-system-properties");
    };
}
123456
```

2、在 application.properties 文件中添加属性。

```xml
name = javastack-application
1
```

3、在 application-dev.properties 文件中添加属性。

```
name = javastack-application-dev
1
```

4、添加测试类

```java
@RunWith(SpringRunner.class)
@SpringBootTest(value = { "name=javastack-test", "sex=1" })
@ActiveProfiles("dev")
public class SpringBootBestPracticeApplicationTests {
 
    @Value("${name}")
    private String name;
 
    @Test
    public void test() {
        System.out.println("name is " + name);
    }
 
}
1234567891011121314
```

运行 test 单元测试，程序输出：
name is javastack-test

> ```
> 根据以上参数动态调整，发现参数会被正确覆盖。了解了 Spring Boot 各种配置的加载顺序，如果配置被覆盖了我们就知道是什么问题了。
> ```

------

### --Spring Boot 配置加载顺序？

```
在 Spring Boot 里面，可以使用以下几种方式来加载配置。

1）properties文件；

2）YAML文件；

3）系统环境变量；

4）命令行参数；
```

等等……

### Spring Boot 配置文件类型？

```
这里我理解的是两种类型 :
	application.properties和application.yml
application.yml配置结构:		key-value 靠.
application.properties配置结构：key-value 靠缩进


spring boot 核心的两个配置文件：
	- bootstrap (. yml 或者 . properties)：boostrap 由父 ApplicationContext 加载的，比 applicaton 优先加载，配置在应用程序上下文的引导阶段生效。一般来说我们在 Spring Cloud Config 或者 Nacos 中会用到它。且 boostrap 里面的属性不能被覆盖；
	- application (. yml 或者 . properties)： 由ApplicatonContext 加载，用于 spring boot 项目的自动化配置。
```

### 什么是 YAML？

```
YAML 是一种人类可读的数据序列化语言。它通常用于配置文件。与属性文件相比，如果我们想要在配置文件中添加复杂的属性，YAML 文件就更加结构化，而且更少混淆。可以看出 YAML 具有分层配置数据。
```



### YAML 配置的优势 ?

```
1. 配置有序，在一些特殊的场景下，配置有序很关键
2. 支持数组，数组中的元素可以是基本数据类型也可以是对象
3. 简洁

相比 properties 配置文件，YAML 还有一个缺点，就是不支持 @PropertySource 注解导入自定义的 YAML 配置。
```



### Spring Boot 可以用 XML 配置 ?

```
Spring Boot 推荐使用 Java 配置而非 XML 配置，但是 Spring Boot 中也可以使用 XML 配置，通过 @ImportResource 注解可以引入一个 XML 配置。
```

### Spring Profiles？

```
Spring Profiles 允许用户根据配置文件（dev，test，prod 等）来注册 bean。因此，当应用程序在开发中运行时，只有某些 bean 可以加载，而在 PRODUCTION中，某些其他 bean 可以加载。假设我们的要求是 Swagger 文档仅适用于 QA 环境，并且禁用所有其他文档。这可以使用配置文件来完成。Spring Boot 使得使用配置文件非常简单。
```



### 自定义端口？

```
为了在自定义端口上运行 Spring Boot 应用程序，您可以在application.properties 中指定端口。server.port = 8090
```



## 安全

### 如何实现 Spring Boot 应用程序的安全性？

```
为了实现 Spring Boot 的安全性，我们使用 spring-boot-starter-security 依赖项，并且必须添加安全配置。它只需要很少的代码。配置类将必须扩展WebSecurityConfigurerAdapter 并覆盖其方法。
```



### 比较一下 Spring Security 和 Shiro 各自的优缺点 ?

```
由于 Spring Boot 官方提供了大量的非常方便的开箱即用的 Starter ，包括 Spring Security 的 Starter ，使得在 Spring Boot 中使用 Spring Security 变得更加容易，甚至只需要添加一个依赖就可以保护所有的接口，所以，如果是 Spring Boot 项目，一般选择 Spring Security 。当然这只是一个建议的组合，单纯从技术上来说，无论怎么组合，都是没有问题的。Shiro 和 Spring Security 相比，主要有如下一些特点：

1. Spring Security 是一个重量级的安全管理框架；Shiro 则是一个轻量级的安全管理框架
2. Spring Security 概念复杂，配置繁琐；Shiro 概念简单、配置简单
3. Spring Security 功能强大；Shiro 功能简单
```



### Spring Boot 中如何解决跨域问题 ?

```
跨域可以在前端通过 JSONP 来解决，但是 JSONP 只可以发送 GET 请求，无法发送其他类型的请求，在 RESTful 风格的应用中，就显得非常鸡肋，因此我们推荐在后端通过 （CORS，Cross-origin resource sharing） 来解决跨域问题。这种解决方案并非 Spring Boot 特有的，在传统的 SSM 框架中，就可以通过 CORS 来解决跨域问题，只不过之前我们是在 XML 文件中配置 CORS ，现在可以通过实现WebMvcConfigurer接口然后重写addCorsMappings方法解决跨域问题。
```

```
项目中前后端分离部署，所以需要解决跨域的问题。
我们使用cookie存放用户登录的信息，在spring拦截器进行权限控制，当权限不符合时，直接返回给用户固定的json结果。
当用户登录以后，正常使用；当用户退出登录状态时或者token过期时，由于拦截器和跨域的顺序有问题，出现了跨域的现象。
我们知道一个http请求，先走filter，到达servlet后才进行拦截器的处理，如果我们把cors放在filter里，就可以优先于权限拦截器执行。
```



### 什么是 CSRF 攻击？

```
CSRF 代表跨站请求伪造。这是一种攻击，迫使最终用户在当前通过身份验证的Web 应用程序上执行不需要的操作。CSRF 攻击专门针对状态改变请求，而不是数据窃取，因为攻击者无法查看对伪造请求的响应。
```



###  Spring Boot 实现异常处理？ 

```
Spring 提供了一种使用 ControllerAdvice 处理异常的非常有用的方法。 我们通过实现一个 

ControlerAdvice 类，来处理控制器类抛出的所有异常。 
```



## 监视器

### Spring Boot 中的监视器是什么？

```
Spring boot actuator 是 spring 启动框架中的重要功能之一。Spring boot 监视器可帮助您访问生产环境中正在运行的应用程序的当前状态。有几个指标必须在生产环境中进行检查和监控。即使一些外部应用程序可能正在使用这些服务来向相关人员触发警报消息。监视器模块公开了一组可直接作为 HTTP URL 访问的REST 端点来检查状态。
```



### 如何在 Spring Boot 中禁用 Actuator 端点安全性？

```
默认情况下，所有敏感的 HTTP 端点都是安全的，只有具有 ACTUATOR 角色的用户才能访问它们。安全性是使用标准的 HttpServletRequest.isUserInRole 方法实施的。 我们可以使用来禁用安全性。只有在执行机构端点在防火墙后访问时，才建议禁用安全性。
```



### 我们如何监视所有 Spring Boot 微服务？

```
Spring Boot 提供监视器端点以监控各个微服务的度量。这些端点对于获取有关应用程序的信息（如它们是否已启动）以及它们的组件（如数据库等）是否正常运行很有帮助。但是，使用监视器的一个主要缺点或困难是，我们必须单独打开应用程序的知识点以了解其状态或健康状况。想象一下涉及 50 个应用程序的微服务，管理员将不得不击中所有 50 个应用程序的执行终端。为了帮助我们处理这种情况，我们将使用位于的开源项目。 它建立在 Spring Boot Actuator 之上，它提供了一个 Web UI，使我们能够可视化多个应用程序的度量。
```



## 整合第三方项目

### 什么是 WebSockets？

```
WebSocket 是一种计算机通信协议，通过单个 TCP 连接提供全双工通信信道。

1、WebSocket 是双向的 -使用 WebSocket 客户端或服务器可以发起消息发送。
2、WebSocket 是全双工的 -客户端和服务器通信是相互独立的。
3、单个 TCP 连接 -初始连接使用 HTTP，然后将此连接升级到基于套接字的连接。然后这个单一连接用于所有未来的通信
4、Light -与 http 相比，WebSocket 消息数据交换要轻得多。
```



### 什么是 Spring Data ?

```
Spring Data 是 Spring 的一个子项目。用于简化数据库访问，支持NoSQL 和 关系数据存储。其主要目标是使数据库的访问变得方便快捷。Spring Data 具有如下特点：

SpringData 项目支持 NoSQL 存储：

1. MongoDB （文档数据库）
2. Neo4j（图形数据库）
3. Redis（键/值存储）
4. Hbase（列族数据库）

SpringData 项目所支持的关系数据存储技术：

1. JDBC
2. JPA

Spring Data Jpa 致力于减少数据访问层 (DAO) 的开发量. 开发者唯一要做的，就是声明持久层的接口，其他都交给 Spring Data JPA 来帮你完成！Spring Data JPA 通过规范方法的名字，根据符合规范的名字来确定方法需要实现什么样的逻辑。
```



### 什么是 Spring Batch？

```
Spring Boot Batch 提供可重用的函数，这些函数在处理大量记录时非常重要，包括日志/跟踪，事务管理，作业处理统计信息，作业重新启动，跳过和资源管理。它还提供了更先进的技术服务和功能，通过优化和分区技术，可以实现极高批量和高性能批处理作业。简单以及复杂的大批量批处理作业可以高度可扩展的方式利用框架处理重要大量的信息。
```



### 什么是 FreeMarker 模板？

```
FreeMarker 是一个基于 Java 的模板引擎，最初专注于使用 MVC 软件架构进行动态网页生成。使用 Freemarker 的主要优点是表示层和业务层的完全分离。程序员可以处理应用程序代码，而设计人员可以处理 html 页面设计。最后使用freemarker 可以将这些结合起来，给出最终的输出页面。
```



### 如何集成 Spring Boot 和 ActiveMQ？

```
对于集成 Spring Boot 和 ActiveMQ，我们使用依赖关系。 它只需要很少的配置，并且不需要样板代码。
```



### 什么是 Apache Kafka？

```
Apache Kafka 是一个分布式发布 - 订阅消息系统。它是一个可扩展的，容错的发布 - 订阅消息系统，它使我们能够构建分布式应用程序。这是一个 Apache 顶级项目。Kafka 适合离线和在线消息消费。
```



### 什么是 Swagger？你用 Spring Boot 实现了它吗？

```
Swagger 广泛用于可视化 API，使用 Swagger UI 为前端开发人员提供在线沙箱。Swagger 是用于生成 RESTful Web 服务的可视化表示的工具，规范和完整框架实现。它使文档能够以与服务器相同的速度更新。当通过 Swagger 正确定义时，消费者可以使用最少量的实现逻辑来理解远程服务并与其进行交互。因此，Swagger消除了调用服务时的猜测。
```



### 前后端分离，如何维护接口文档 ?

```
前后端分离开发日益流行，大部分情况下，我们都是通过 Spring Boot 做前后端分离开发，前后端分离一定会有接口文档，不然会前后端会深深陷入到扯皮中。一个比较笨的方法就是使用 word 或者 md 来维护接口文档，但是效率太低，接口一变，所有人手上的文档都得变。在 Spring Boot 中，这个问题常见的解决方案是 Swagger ，使用 Swagger 我们可以快速生成一个接口文档网站，接口一旦发生变化，文档就会自动更新，所有开发工程师访问这一个在线网站就可以获取到最新的接口文档，非常方便。
```



## 其他

### Spring Boot实现热部署？

```
这可以使用 DEV 工具来实现。通过这种依赖关系，您可以节省任何更改，嵌入式tomcat 将重新启动。Spring Boot 有一个开发工具（DevTools）模块，它有助于提高开发人员的生产力。Java 开发人员面临的一个主要挑战是将文件更改自动部署到服务器并自动重启服务器。开发人员可以重新加载 Spring Boot 上的更改，而无需重新启动服务器。这将消除每次手动部署更改的需要。Spring Boot 在发布它的第一个版本时没有这个功能。这是开发人员最需要的功能。DevTools 模块完全满足开发人员的需求。该模块将在生产环境中被禁用。它还提供 H2 数据库控制台以更好地测试应用程序。
```

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

### 您使用了哪些 starter maven 依赖项？

```
使用了下面的一些依赖项

spring-boot-starter-activemq

spring-boot-starter-security

这有助于增加更少的依赖关系，并减少版本的冲突。
```



### Spring Boot 中的 starter 到底是什么 ?

```
首先，这个 Starter 并非什么新的技术点，基本上还是基于 Spring 已有功能来实现的。首先它提供了一个自动化配置类，一般命名为 `XXXAutoConfiguration` ，在这个配置类中通过条件注解来决定一个配置是否生效（条件注解就是 Spring 中原本就有的），然后它还会提供一系列的默认配置，也允许开发者根据实际情况自定义相关配置，然后通过类型安全的属性注入将这些配置属性注入进来，新注入的属性会代替掉默认属性。正因为如此，很多第三方框架，我们只需要引入依赖就可以直接使用了。当然，开发者也可以自定义 Starter
```



### spring-boot-starter-parent 有什么用 ?

```
我们都知道，新创建一个 Spring Boot 项目，默认都是有 parent 的，这个 parent 就是 spring-boot-starter-parent ，spring-boot-starter-parent 主要有如下作用：

1. 定义了 Java 编译版本为 1.8 。
2. 使用 UTF-8 格式编码。
3. 继承自 spring-boot-dependencies，这个里边定义了依赖的版本，也正是因为继承了这个依赖，所以我们在写依赖时才不需要写版本号。
4. 执行打包操作的配置。
5. 自动化的资源过滤。
6. 自动化的插件配置。
7. 针对 application.properties 和 application.yml 的资源过滤，包括通过 profile 定义的不同环境的配置文件，例如 application-dev.properties 和 application-dev.yml。
```



### Spring Boot 打成的 jar 和普通的 jar 有什么区别 ?

```
Spring Boot 项目最终打包成的 jar 是可执行 jar ，这种 jar 可以直接通过 `java -jar xxx.jar` 命令来运行，这种 jar 不可以作为普通的 jar 被其他项目依赖，即使依赖了也无法使用其中的类。

Spring Boot 的 jar 无法被其他项目依赖，主要还是他和普通 jar 的结构不同。普通的 jar 包，解压后直接就是包名，包里就是我们的代码，而 Spring Boot 打包成的可执行 jar 解压后，在 `\BOOT-INF\classes` 目录下才是我们的代码，因此无法被直接引用。如果非要引用，可以在 pom.xml 文件中增加配置，将 Spring Boot 项目打包成两个 jar ，一个可执行，一个可引用。
```



### 运行 Spring Boot 有哪几种方式？

```
1）打包用命令或者放到容器中运行

2）用 Maven/ Gradle 插件运行

3）直接执行 main 方法运行
```



### Spring Boot 需要独立的容器运行吗？

```
可以不需要，内置了 Tomcat/ Jetty 等容器。
```



### 开启 Spring Boot 特性有哪几种方式？

```
1）继承spring-boot-starter-parent项目

2）导入spring-boot-dependencies项目依赖
```



### 如何使用 Spring Boot 实现异常处理？

```
Spring 提供了一种使用 ControllerAdvice 处理异常的非常有用的方法。 我们通过实现一个 ControlerAdvice 类，来处理控制器类抛出的所有异常。
```



### 如何使用 Spring Boot 实现分页和排序？

```
使用 Spring Boot 实现分页非常简单。使用 Spring Data-JPA 可以实现将可分页的传递给存储库方法。
```



### 微服务中如何实现 session 共享 ?

```
在微服务中，一个完整的项目被拆分成多个不相同的独立的服务，各个服务独立部署在不同的服务器上，各自的 session 被从物理空间上隔离开了，但是经常，我们需要在不同微服务之间共享 session ，常见的方案就是 Spring Session + Redis 来实现 session 共享。将所有微服务的 session 统一保存在 Redis 上，当各个微服务对 session 有相关的读写操作时，都去操作 Redis 上的 session 。这样就实现了 session 共享，Spring Session 基于 Spring 中的代理过滤器实现，使得 session 的同步操作对开发人员而言是透明的，非常简便。
```



### Spring Boot 中如何实现定时任务 ?

```
定时任务也是一个常见的需求，Spring Boot 中对于定时任务的支持主要还是来自 Spring 框架。

在 Spring Boot 中使用定时任务主要有两种不同的方式，一个就是使用 Spring 中的 @Scheduled 注解，另一个则是使用第三方框架 Quartz。

使用 Spring 中的 @Scheduled 的方式主要通过 @Scheduled 注解来实现。

使用 Quartz ，则按照 Quartz 的方式，定义 Job 和 Trigger 即可。
```

