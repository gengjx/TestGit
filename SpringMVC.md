### 1.什么是Spring MVC ?

```
Spring MVC是一个基于Java的实现了MVC设计模式的请求驱动类型的轻量级Web框架，通过把Model，View，Controller分离，将web层进行职责解耦，把复杂的web应用分成逻辑清晰的几部分，简化开发，减少出错，方便组内开发人员之间的配合。
```



### 2.Springmvc的优点？

```
（1）可以支持各种视图技术,而不仅仅局限于JSP；

（2）与Spring框架集成（如IoC容器、AOP等）；

（3）清晰的角色分配：前端控制器(dispatcherServlet) , 请求到处理器映射（handlerMapping), 处理器适配器（HandlerAdapter), 视图解析器（ViewResolver）。

（4） 支持各种请求资源的映射策略。
```



### 2.spring mvc 运行流程？

![img](https://img-blog.csdnimg.cn/20190412163441472.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE2NjU5OTE=,size_16,color_FFFFFF,t_70)

```
流程 
- 1、用户发送请求至前端控制器DispatcherServlet 
- 2、DispatcherServlet收到请求调用HandlerMapping处理器映射器。 
- 3、处理器映射器找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。 
- 4、DispatcherServlet调用HandlerAdapter处理器适配器 
- 5、HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。 
- 6、Controller执行完成返回ModelAndView 
- 7、HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet 
- 8、DispatcherServlet将ModelAndView传给ViewReslover视图解析器 
- 9、ViewReslover解析后返回具体View 
- 10、DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。 
- 11、DispatcherServlet响应用户
```



### 3.spring mvc 组件？

| **名称**                          | **简介**                                                     |
| --------------------------------- | ------------------------------------------------------------ |
| **`HandlerMapping`**              | **管理请求(`request`)和处理句柄(`Handler`)之间的映射关系**   |
| **`HandlerAdapter`**              | **处理句柄适配器, 1.每个`HandlerAdapter`包装一个`Handler`, 2.`HandlerAdapter`主要用于隔离调用者`DispatcherServlet`和各式各样的`Handler`** |
| **`HandlerExceptionResolver`**    | **处理句柄异常解析器**                                       |
| **`ViewResolver`**                | **视图解析器：根据视图名称和`Locale`解析为具体`View`对象用来渲染页面** |
| **`RequestToViewNameTranslator`** | **请求到视图名称的翻译器:从`request`中获取视图名称,作为`ViewResolver`的补充** |
| **`LocaleResolver`**              | **`Locale`地区解析器 处理本地化/国际化语言相关，结合上下文和当前请求分析得到应该使用的`Locale`地区** |
| **`ThemeResolver`**               | **主题解析器，处理`UI`主题(`look and feel`)相关**            |
| **`MultipartResolver`**           | **`Multipart`上传数据解析器**                                |
| **`FlashMapManager`**             | **管理`FlashMap`结构的重定向参数**                           |

### 4.@RequestMapping 作用？

```
@RequestMapping
	RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上。

```



### 5.@Autowired 作用？

这个注解就是spring可以自动帮你把bean里面引用的对象的setter/getter方法省略，它会自动帮你set/get。

```cs
<bean id="userDao" class="..."/><bean id="userService" class="...">    <property name="userDao">      <ref bean="userDao"/>    </property></bean>
```

这样你在userService里面要做一个userDao的setter/getter方法。

但如果你用了@Autowired的话，你只需要在UserService的实现类中声明即可。

```
@Autowired private IUserDao userdao;
```

### 5.springMVC和struts2区别?

```
（1）springmvc的入口是一个servlet即前端控制器（DispatchServlet），而struts2入口是一个filter过虑器（StrutsPrepareAndExecuteFilter）。

（2）springmvc是基于方法开发(一个url对应一个方法)，请求参数传递到方法的形参，可以设计为单例或多例(建议单例)，struts2是基于类开发，传递参数是通过类的属性，只能设计为多例。

（3）Struts采用值栈存储请求和响应的数据，通过OGNL存取数据，springmvc通过参数解析器是将request请求内容解析，并给方法形参赋值，将数据和视图封装成ModelAndView对象，最后又将ModelAndView中的模型数据通过reques域传输到页面。Jsp视图解析器默认使用jstl。
```

 

### 6.SpringMVC重定向和转发？

```
（1）转发：在返回值前面加"forward:"，譬如"forward:user.do?name=method4"

（2）重定向：在返回值前面加"redirect:"，如"redirect:http://www.baidu.com"

```



### 7.SpringMVC和AJAX相互调用？

```
通过Jackson框架就可以把Java里面的对象直接转化成Js可以识别的Json对象。具体步骤如下 ：

（1）加入Jackson.jar

（2）在配置文件中配置json的映射

（3）在接受Ajax方法里面可以直接返回Object,List等,但方法前面要加上@ResponseBody注解。
```

 

### 8.POST请求中文乱码问题，GET如何处理？

```
（1）解决post请求乱码问题：
	在web.xml中配置一个CharacterEncodingFilter过滤器，设置成utf-8；
（2）get请求中文参数出现乱码解决方法有两个：
	①修改tomcat配置文件添加编码与工程编码一致，如下：
	②另外一种方法对参数进行重新编码：
	String userName = new 						String(request.getParamter("userName").getBytes("ISO8859-1"),"utf-8")
	ISO8859-1是tomcat默认编码，需要将tomcat编码后的内容按utf-8编码。
```



### 9.Spring MVC的异常处理 ？

```
答：可以将异常抛给Spring框架，由Spring框架来处理；我们只需要配置简单的异常处理器，在异常处理器中添视图页面即可。
```



### 10.SpringMVC的控制器是不是单例模式？

```
如果是,有什么问题,怎么解决?
答：是单例模式,所以在多线程访问的时候有线程安全问题,不要用同步,会影响性能的,解决方案是在控制器里面不能写字段。
```



### 11. SpringMVC常用的注解有哪些？

```
@RequestMapping：用于处理请求 url 映射的注解，可用于类或方法上。
				用于类上，则表示类中的所有响应请求的方法都是以该地址作为父路径。
@RequestBody：注解实现接收http请求的json数据，将json转换为java对象。
@ResponseBody：注解实现将conreoller方法返回对象转化为json对象响应给客户。
```



### 12.SpingMVC控制器的注解一般用那个,有没有别的注解可以替代？

```
答：一般用@Controller注解,也可以使用@RestController,
@RestController注解相当于@ResponseBody ＋ @Controller,表示是表现层,除此之外，一般不用别的注解代替。
```



### 13.拦截get方式提交的方法,怎么配置？

```
答：可以在@RequestMapping注解里面加上method=RequestMethod.GET。
```



### 14、在方法里得到Request||Session？

```
答：直接在方法的形参中声明request,SpringMvc就自动把request对象传入。
```



### 15.在拦截的方法里获取前台传入参数？

```
答：直接在形参里面声明这个参数就可以,但必须名字和传过来的参数一样。
```



### 16.前台有很多个参数传入,并且这些参数都是一个对象的,那么怎么样快速得到这个对象？

```
答：直接在方法中声明这个对象,SpringMvc就自动会把属性赋值到这个对象里面。
```



### 17、SpringMVC中函数的返回值是什么？

```
答：返回值可以有很多类型,有String, ModelAndView。ModelAndView类把视图和数据都合并的一起的，但一般用String比较好。
```



### 18、SpringMVC用什么对象从后台向前台传递数据的？

```
答：通过ModelMap对象,可以在这个对象里面调用put方法,把对象加到里面,前台就可以通过el表达式拿到。
```



### 19、怎么样把ModelMap里面的数据放入Session里面？

```
答：可以在类上面加上@SessionAttributes注解,里面包含的字符串就是要放入session里面的key。
```

 

### 20、SpringMvc里面拦截器是怎么写的：

```
有两种写法,一种是实现HandlerInterceptor接口，另外一种是继承适配器类，接着在接口方法当中，实现处理逻辑；然后在SpringMvc的配置文件中配置拦截器即可：
```



### 21、注解原理：

```
注解本质是一个继承了Annotation的特殊接口，其具体实现类是Java运行时生成的动态代理类。我们通过反射获取注解时，返回的是Java运行时生成的动态代理对象。通过代理对象调用自定义注解的方法，会最终调用AnnotationInvocationHandler的invoke方法。该方法会从memberValues这个Map中索引出对应的值。而memberValues的来源是Java常量池。
```

