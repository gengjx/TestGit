### 1.mybatis 中 #{}和 ${}区别？

```
-1. #将传入的数据都当成一个字符串，会对自动传入的数据加一个双引号。
	如：order by #user_id#，
	如果传入的值是111,那么解析成sql时的值为order by "111"
    如果传入的值是id，则解析成的sql为order by "id".
-2. $将传入的数据直接显示生成在sql中。
	如：order by $user_id$，
	如果传入的值是111,那么解析成sql时的值为order by user_id, 
	如果传入的值是id，则解析成的sql为order by id.
-3. #方式能够很大程度防止sql注入。
-4. $方式无法防止Sql注入。
-5. $方式一般用于传入数据库对象，例如传入表名.
-6. 一般能用#的就别用$.
- MyBatis排序时使用order by 动态参数时需要注意，用$而不是#
```

### 2.mybatis 有几种分页方式？

```
- 1、数组分页
- 2、sql分页
- 3、拦截器分页
- 4、RowBounds分页：是一次性查询全部结果，只不过会根据参数丢掉一部分
	分页插件：比如PageHelper等
```

### 3.mybatis 逻辑分页和物理分页区别？

```
-  1：逻辑分页 虽然看起来实现了分页的功能，但实际上是将查询的所有结果放置在内存中，每次都从内存获取。内存开销比较大,在数据量比较小的情况下效率比物理分页高;在数据量很大的情况下,内存开销过大,容易内存溢出,不建议使用

-  2：物理分页 这种分页方法从底层上就是每次只查询对应条目数量的数据,内存开销比较小,在数据量比较小的情况下效率比逻辑分页还是低,在数据量很大的情况下,建议使用物理分页
```



### 4.mybatis 支持延迟加载？

```
 在mybatis 中默认没有使用延迟加载 ,但是通过配置 
 <setting name="lazyLoadingEnabled" value="true"/>实现延迟加载
```

### 5.延迟加载的原理

```
动态代理:在Hibernate中,被动态代理的延迟对象是one方;many方还是第一个普通的一个对象;
```

### 6.mybatis 的一级缓存和二级缓存？

```
一级缓存是SqlSession级别的缓存。在操作数据库时需要构造sqlSession对象，在对象中有一个数据结构用于存储缓存数据。不同的sqlSession之间的缓存数据区域是互相不影响的。也就是他只能作用在同一个sqlSession中，不同的sqlSession中的缓存是互相不能读取的

二级缓存是mapper级别的缓存，多个SqlSession去操作同一个Mapper的sql语句，多个SqlSession可以共用二级缓存，二级缓存是跨SqlSession的。二级缓存的作用范围更大。
```



### 7.mybatis 和 hibernate 区别？

- ```
  -   Hibernate 框架** 
  
  ​    **Hibernate**是一个开放源代码的对象关系映射框架,它对JDBC进行了非常轻量级的对象封装,建立对象与数据库表的映射。是一个全自动的、完全面向对象的持久层框架。
  
  -  **Mybatis框架**
  
  ​    **Mybatis**是一个开源对象关系映射框架，原名：ibatis,2010年由谷歌接管以后更名。是一个半自动化的持久层框架。
  
  -   **2.1 开发方面**
  
  ​    在项目开发过程当中，就速度而言：
  
  ​      hibernate开发中，sql语句已经被封装，直接可以使用，加快系统开发；
  
  ​      Mybatis 属于半自动化，sql需要手工完成，稍微繁琐；
  
  ​    但是，凡事都不是绝对的，如果对于庞大复杂的系统项目来说，发杂语句较多，选择hibernate 就不是一个好方案。
  
  -   **2.2 sql优化方面**
  
  ​    Hibernate 自动生成sql,有些语句较为繁琐，会多消耗一些性能；
  
  ​    Mybatis 手动编写sql，可以避免不需要的查询，提高系统性能；
  
  -   **2.3 对象管理比对**
  
  ​    Hibernate 是完整的对象-关系映射的框架，开发工程中，无需过多关注底层实现，只要去管理对象即可；
  
  ​    Mybatis 需要自行管理 映射关系；
  
  -   2.4 缓存方面  
      -  **相同点：**
      -  Hibernate和Mybatis的二级缓存除了采用系统默认的缓存机制外，都可以通过实现你自己的缓存或为其他第三方缓   存方案，创建适配器来完全覆盖缓存行为。
      -  **不同点：**
      -  **Hibernate的二级缓存配置在SessionFactory生成的配置文件中进行详细配置，然后再在具体的表-对象映射中配置是那种缓存。**
      -  MyBatis的二级缓存配置都是在每个具体的表-对象映射中进行详细配置，这样针对不同的表可以自定义不同的缓存机制。并且Mybatis可以在命名空间中共享相同的缓存配置和实例，通过Cache-ref来实现。
  
  **比较：**
  
  - **Hibernate** 具有良好的管理机制，用户不需要关注SQL，如果二级缓存出现脏数据，系统会保存，；
  - Mybatis 在使用的时候要谨慎，避免缓存CAche 的使用。
  
  **Hibernate优势**
  
  - Hibernate的DAO层开发比MyBatis简单，Mybatis需要维护SQL和结果映射。
  - Hibernate对对象的维护和缓存要比MyBatis好，对增删改查的对象的维护要方便。
  - Hibernate数据库移植性很好，MyBatis的数据库移植性不好，不同的数据库需要写不同SQL。
  - Hibernate有更好的二级缓存机制，可以使用第三方缓存。MyBatis本身提供的缓存机制不佳。
  
  **Mybatis优势**
  
  - MyBatis可以进行更为细致的SQL优化，可以减少查询字段。
  - MyBatis容易掌握，而Hibernate门槛较高。
  
  **一句话总结**
  
  - **Mybatis**：小巧、方便、高效、简单、直接、半自动化
  - **Hibernate**：强大、方便、高效、复杂、间接、全自动化
  ```

  

### 8.mybatis 执行器（Executor）？

```
Mybatis有三种基本的Executor执行器: **SimpleExecutor、ReuseExecutor、BatchExecutor。**

- **SimpleExecutor：**每执行一次update或select，就开启一个Statement对象，用完立刻关闭Statement对象。

- **ReuseExecutor：**执行update或select，以sql作为key查找Statement对象，存在就使用，不存在就创建，用完后，不关闭Statement对象，而是放置于Map内，供下一次使用。简言之，就是重复使用Statement对象。

- **BatchExecutor：**执行update（没有select，JDBC批处理不支持select），将所有sql都添加到批处理中（addBatch()），等待统一执行（executeBatch()），它缓存了多个Statement对象，每个Statement对象都是addBatch()完毕后，等待逐一执行executeBatch()批处理。与JDBC批处理相同。

  作用范围：Executor的这些特点，都严格限制在SqlSession生命周期范围内。
```



### 9.Mybatis指定一种Executor？

```
答：在Mybatis配置文件中，可以指定默认的ExecutorType执行器类型，也可以手动给DefaultSqlSessionFactory的创建SqlSession的方法传递ExecutorType类型参数。
```

### 10.mybatis 分页插件实现原理？

```
实现原理：就是在StatementHandler之前进行拦截，对MappedStatement进行一系列的操作（大致就是拼上分页sql）
```



### 11.mybatis 如何编写一个自定义插件？



### 1、什么是Mybatis？

```
（1）Mybatis是一个半ORM（对象关系映射）框架，它内部封装了JDBC，开发时只需要关注SQL语句本身，不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。程序员直接编写原生态sql，可以严格控制sql执行性能，灵活度高。

（2）MyBatis 可以使用 XML 或注解来配置和映射原生信息，将 POJO映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

（3）通过xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过java对象和 statement中sql的动态参数进行映射生成最终执行的sql语句，最后由mybatis框架执行sql并将结果映射为java对象并返回。（从执行sql到返回result的过程）。
```

### 2、Mybaits的优点

```
（1）基于SQL语句编程，相当灵活，不会对应用程序或者数据库的现有设计造成任何影响，SQL写在XML里，解除sql与程序代码的耦合，便于统一管理；提供XML标签，支持编写动态SQL语句，并可重用。

（2）与JDBC相比，减少了50%以上的代码量，消除了JDBC大量冗余的代码，不需要手动开关连接；

（3）很好的与各种数据库兼容（因为MyBatis使用JDBC来连接数据库，所以只要JDBC支持的数据库MyBatis都支持）。

（4）能够与Spring很好的集成；

（5）提供映射标签，支持对象与数据库的ORM字段关系映射；提供对象关系映射标签，支持对象关系组件维护。
```

### 3、MyBatis框架的缺点

```
（1）SQL语句的编写工作量较大，尤其当字段多、关联表多时，对开发人员编写SQL语句的功底有一定要求。

（2）SQL语句依赖于数据库，导致数据库移植性差，不能随意更换数据库。
```

### 4、MyBatis框架适用场合

```
（1）MyBatis专注于SQL本身，是一个足够灵活的DAO层解决方案。

（2）对性能的要求很高，或者需求变化较多的项目，如互联网项目，MyBatis将是不错的选择。

```

5、MyBatis与Hibernate区别？

```
（1）Mybatis和hibernate不同，它不完全是一个ORM框架，因为MyBatis需要程序员自己编写Sql语句。

（2）Mybatis直接编写原生态sql，可以严格控制sql执行性能，灵活度高，非常适合对关系数据模型要求不高的软件开发，因为这类软件需求变化频繁，一但需求变化要求迅速输出成果。但是灵活的前提是mybatis无法做到数据库无关性，如果需要实现支持多种数据库的软件，则需要自定义多套sql映射文件，工作量大。 

（3）Hibernate对象/关系映射能力强，数据库无关性好，对于关系模型要求高的软件，如果用hibernate开发可以节省很多代码，提高效率。 
```

 

### 6、#{}和${}的区别是什么？

```
#{}是预编译处理，${}是字符串替换。

Mybatis在处理#{}时，会将sql中的#{}替换为?号，调用PreparedStatement的set方法来赋值；

Mybatis在处理${}时，就是把${}替换成变量的值。

使用#{}可以有效的防止SQL注入，提高系统安全性。
```

 

### 7、实体类的属性名和表的字段名不一致？

```
第1种： 通过在查询的sql语句中定义字段名的别名，让字段名的别名和实体类的属性名一致。
    <select id=”selectorder” parametertype=”int” resultetype=”me.gacl.domain.order”> 			select order_id id, order_no orderno form orders where order_id=#{id};
    </select>

第2种： 通过<resultMap>来映射字段名和实体类属性名的一一对应的关系。
	<select id="getOrder" parameterType="int" resultMap="orderresultmap">
        select * from orders where order_id=#{id}
    </select>
 
   <resultMap type=”me.gacl.domain.order” id=”orderresultmap”>
        <!–用id属性来映射主键字段–>
        <id property=”id” column=”order_id”>
        <!–用result属性来映射非主键字段，property为实体类属性名，column为数据表中的属性–>
        <result property = “orderno” column =”order_no”/>
        <result property=”price” column=”order_price” />
    </reslutMap>
 
```

### 8、 模糊查询like语句该怎么写?

```
第1种：在Java代码中添加sql通配符。
	<select id=”selectlike”>
     select * from foo where bar like #{value}
    </select>

第2种：在sql语句中拼接通配符，会引起sql注入
	<select id=”selectlike”>
     select * from foo where bar like "%"${value}"%"
    </select>
```



### 9、Xml映射文件和Dao接口对应？

```
Dao接口的工作原理是什么？
Dao接口里的方法，参数不同时，方法能重载吗？

Dao接口即Mapper接口。接口的全限名，就是映射文件中的namespace的值；接口的方法名，就是映射文件中Mapper的Statement的id值；接口方法内的参数，就是传递给sql的参数。

Mapper接口是没有实现类的，当调用接口方法时，接口全限名+方法名拼接字符串作为key值，可唯一定位一个MapperStatement。在Mybatis中，每一个<select>、<insert>、<update>、<delete>标签，都会被解析为一个MapperStatement对象。

举例：com.mybatis3.mappers.StudentDao.findStudentById，可以唯一找到namespace为com.mybatis3.mappers.StudentDao下面 id 为 findStudentById 的 MapperStatement。

Mapper接口里的方法，是不能重载的，因为是使用 全限名+方法名 的保存和寻找策略。Mapper 接口的工作原理是JDK动态代理，Mybatis运行时会使用JDK动态代理为Mapper接口生成代理对象proxy，代理对象会拦截接口方法，转而执行MapperStatement所代表的sql，然后将sql执行结果返回。
```

 

### 10、Mybatis是如何进行分页的？分页插件的原理是什么？

​    

```
	Mybatis使用RowBounds对象进行分页，它是针对ResultSet结果集执行的内存分页，而非物理分页。可以在sql内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。

	分页插件的基本原理是使用Mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的sql，然后重写sql，根据dialect方言，添加对应的物理分页语句和物理分页参数。
```

 

### 11、Mybatis是如何将sql执行结果封装为目标对象并返回的？都有哪些映射形式？

```
第一种是使用<resultMap>标签，逐一定义数据库列名和对象属性名之间的映射关系。

第二种是使用sql列的别名功能，将列的别名书写为对象属性名。

有了列名与属性名的映射关系后，Mybatis通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。
```

 

### 12、如何执行批量插入?

首先,创建一个简单的insert语句:

```java
首先,创建一个简单的insert语句: 
<insert id=”insertname”>   insert into names (name) values (#{value})    </insert>
java代码中像下面这样执行批处理插入:
    list<string> names = new arraylist();
    // 注意这里 executortype.batch
    sqlsession sqlsession = sqlsessionfactory.opensession(executortype.batch);
    try {
     namemapper mapper = sqlsession.getmapper(namemapper.class);
     for (string name : names) {
         mapper.insertname(name);
     }
     sqlsession.commit();
    }catch(Exception e){
     e.printStackTrace();
     sqlSession.rollback(); 
     throw e; 
    }
     finally {
         sqlsession.close();
    }
```



### 13、如何获取自动生成的(主)键值?

```
insert 方法总是返回一个int值 ，这个值代表的是插入的行数。

如果采用自增长策略，自动生成的键值在 insert 方法执行完后可以被设置到传入的参数对象中。

示例：
```



### 14、在mapper中如何传递多个参数?

```java
（1）第一种：DAO层的函数
    Public UserselectUser(String name,String area);  
	//对应的xml,#{0}代表接收的是dao层中的第一个参数，#{1}代表dao层中第二参数，更多参数一致往后加即可。
	<select id="selectUser"resultMap="BaseResultMap">      
        select *  fromuser_user_t   whereuser_name = #{0} anduser_area=#{1}  
	</select>   
        
（2）第二种： 使用 @param 注解
 user selectuser(@param(“username”) string username,@param(“password”) string password);
	<select id=”selectuser” resulttype=”user”>         
        select id, username, hashedpassword from some_table 
        where username = #{username} and hashedpassword = #{hashedpassword}
	</select> 
        
（3）第三种：多个参数封装成map

```

 

### 15、动态sql？执行原理？有哪些？

```
Mybatis动态sql可以在Xml映射文件内，以标签的形式编写动态sql，执行原理是根据表达式的值 完成逻辑判断并动态拼接sql的功能。

Mybatis提供了9种动态sql标签：trim | where | set | foreach | if | choose | when | otherwise | bind。
```

 

### 16、Xml映射文件中，常见标签？

```
<select><insert><updae><delete>
<resultMap>、<parameterMap>、<sql>、<include>、<selectKey>
动态sql的9个标签，其中<sql>为sql片段标签，通过<include>标签引入sql片段，<selectKey>为不支持自增的主键生成策略标签。
```



### 17、Mybatis不同的Xml，id是否可以重复？

```
不同的Xml映射文件，如果配置了namespace，那么id可以重复；如果没有配置namespace，那么id不能重复；

原因就是namespace+id是作为Map<String, MapperStatement>的key使用的，如果没有namespace，就剩下id，那么，id重复会导致数据互相覆盖。有了namespace，自然id就可以重复，namespace不同，namespace+id自然也就不同。

但是，在以前的Mybatis版本的namespace是可选的，不过新版本的namespace已经是必须的了。

 
```



### 18、Mybatis是半自动ORM映射工具？区别？

```
Hibernate属于全自动ORM映射工具，使用Hibernate查询关联对象或者关联集合对象时，可以根据对象关系模型直接获取，所以它是全自动的。而Mybatis在查询关联对象或关联集合对象时，需要手动编写sql来完成，所以，称之为半自动ORM映射工具。
```

 

### 19、 一对一 & 一对多的关联查询 ？

```xml
<!--association  一对一关联查询 -->  
    <select id="getClass" parameterType="int" resultMap="ClassesResultMap">  
        select * from class c,teacher t where c.teacher_id=t.t_id and c.c_id=#{id}  
    </select>  
 
    <resultMap type="com.lcb.user.Classes" id="ClassesResultMap">  
        <!-- 实体类的字段名和数据表的字段名映射 -->  
        <id property="id" column="c_id"/>  
        <result property="name" column="c_name"/>  
        <association property="teacher" javaType="com.lcb.user.Teacher">  
            <id property="id" column="t_id"/>  
            <result property="name" column="t_name"/>  
        </association>  
    </resultMap> 

 <!--collection  一对多关联查询 -->  
    <select id="getClass2" parameterType="int" resultMap="ClassesResultMap2">  
        select * from class c,teacher t,student s 
        where c.teacher_id=t.t_id and c.c_id=s.class_id and c.c_id=#{id}  
    </select>  
 
    <resultMap type="com.lcb.user.Classes" id="ClassesResultMap2">  
        <id property="id" column="c_id"/>  
        <result property="name" column="c_name"/>  
        <association property="teacher" javaType="com.lcb.user.Teacher">  
            <id property="id" column="t_id"/>  
            <result property="name" column="t_name"/>  
        </association>  
 
        <collection property="student" ofType="com.lcb.user.Student">  
            <id property="id" column="s_id"/>  
            <result property="name" column="s_name"/>  
        </collection>  
    </resultMap>  

```

 

### 20、MyBatis实现一对一有几种方式?

```
有联合查询和嵌套查询,联合查询是几个表联合查询,只查询一次, 通过在resultMap里面配置association节点配置一对一的类就可以完成；

嵌套查询是先查一个表，根据这个表里面的结果的 外键id，去再另外一个表里面查询数据,也是通过association配置，但另外一个表的查询通过select属性配置。
```

 

### 21、MyBatis实现一对多有几种方式,怎么操作的？

```
    有联合查询和嵌套查询。联合查询是几个表联合查询,只查询一次,通过在resultMap里面的collection节点配置一对多的类就可以完成；嵌套查询是先查一个表,根据这个表里面的 结果的外键id,去再另外一个表里面查询数据,也是通过配置collection,但另外一个表的查询通过select节点配置。
```

 

### 22、Mybatis是否支持延迟加载？如果支持，它的实现原理是什么？

```
答：Mybatis仅支持association关联对象和collection关联集合对象的延迟加载，association指的就是一对一，collection指的就是一对多查询。在Mybatis配置文件中，可以配置是否启用延迟加载lazyLoadingEnabled=true|false。

它的原理是，使用CGLIB创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用a.getB().getName()，拦截器invoke()方法发现a.getB()是null值，那么就会单独发送事先保存好的查询关联B对象的sql，把B查询上来，然后调用a.setB(b)，于是a的对象b属性就有值了，接着完成a.getB().getName()方法的调用。这就是延迟加载的基本原理。

当然了，不光是Mybatis，几乎所有的包括Hibernate，支持延迟加载的原理都是一样的。
```

 

###  23、Mybatis的一级、二级缓存:

```
1）一级缓存: 基于 PerpetualCache 的 HashMap 本地缓存，其存储作用域为 Session，当 Session flush 或 close 之后，该 Session 中的所有 Cache 就将清空，默认打开一级缓存。

2）二级缓存与一级缓存其机制相同，默认也是采用 PerpetualCache，HashMap 存储，不同在于其存储作用域为 Mapper(Namespace)，并且可自定义存储源，如 Ehcache。默认不打开二级缓存，要开启二级缓存，使用二级缓存属性类需要实现Serializable序列化接口(可用来保存对象的状态),可在它的映射文件中配置<cache/> ；

3）对于缓存数据更新机制，当某一个作用域(一级缓存 Session/二级缓存Namespaces)的进行了C/U/D 操作后，默认该作用域下所有 select 中的缓存将被 clear 掉并重新更新，如果开启了二级缓存，则只根据配置判断是否刷新。

 
```

### 24、什么是MyBatis的接口绑定？有哪些实现方式？

```
接口绑定，就是在MyBatis中任意定义接口,然后把接口里面的方法和SQL语句绑定, 我们直接调用接口方法就可以,这样比起原来了SqlSession提供的方法我们可以有更加灵活的选择和设置。

接口绑定有两种实现方式,一种是通过注解绑定，就是在接口的方法上面加上 @Select、@Update等注解，里面包含Sql语句来绑定；另外一种就是通过xml里面写SQL来绑定, 在这种情况下,要指定xml映射文件里面的namespace必须为接口的全路径名。当Sql语句比较简单时候,用注解绑定, 当SQL语句比较复杂时候,用xml绑定,一般用xml绑定的比较多。
```

### 25、使用MyBatis的mapper接口调用时有哪些要求？

```
① Mapper接口方法名和mapper.xml中定义的每个sql的id相同；
② Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql 的parameterType的类型相同；
③ Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同；
④ Mapper.xml文件中的namespace即是mapper接口的类路径。
```

### 26、Mapper编写有哪几种方式？

```
**第一种：接口实现类继承SqlSessionDaoSupport：**使用此种方法需要编写mapper接口，mapper接口实现类、mapper.xml文件。
（1）在sqlMapConfig.xml中配置mapper.xml的位置
<mappers>
<mapper resource="mapper.xml文件的地址" />
<mapper resource="mapper.xml文件的地址" />
</mappers>
（2）定义mapper接口
（3）实现类集成SqlSessionDaoSupport
mapper方法中可以this.getSqlSession()进行数据增删改查。
（4）spring 配置
<bean id=" " class="mapper接口的实现">
<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
</bean>


**第二种：使用org.mybatis.spring.mapper.MapperFactoryBean：**
（1）在sqlMapConfig.xml中配置mapper.xml的位置，如果mapper.xml和mappre接口的名称相同且在同一个目录，这里可以不用配置
<mappers>
<mapper resource="mapper.xml文件的地址" />
<mapper resource="mapper.xml文件的地址" />
</mappers>
（2）定义mapper接口：
①mapper.xml中的namespace为mapper接口的地址
②mapper接口中的方法名和mapper.xml中的定义的statement的id保持一致
③Spring中定义
<bean id="" class="org.mybatis.spring.mapper.MapperFactoryBean">
<property name="mapperInterface"  value="mapper接口地址" /> 
<property name="sqlSessionFactory" ref="sqlSessionFactory" /> 
</bean>


**第三种：使用mapper扫描器：**
（1）mapper.xml文件编写：
mapper.xml中的namespace为mapper接口的地址；
mapper接口中的方法名和mapper.xml中的定义的statement的id保持一致；
如果将mapper.xml和mapper接口的名称保持一致则不用在sqlMapConfig.xml中进行配置。 
（2）定义mapper接口：
注意mapper.xml的文件名和mapper的接口名称保持一致，且放在同一个目录
（3）配置mapper扫描器：
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
<property name="basePackage" value="mapper接口包地址"></property>
<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/> 
</bean>
（4）使用扫描器后从spring容器中获取mapper的实现对象。
```

### 27、简述Mybatis的插件运行原理，以及如何编写一个插件。

```
答：Mybatis仅可以编写针对ParameterHandler、ResultSetHandler、StatementHandler、Executor这4种接口的插件，Mybatis使用JDK的动态代理，为需要拦截的接口生成代理对象以实现接口方法拦截功能，每当执行这4种接口对象的方法时，就会进入拦截方法，具体就是InvocationHandler的invoke()方法，当然，只会拦截那些你指定需要拦截的方法。

编写插件：实现Mybatis的Interceptor接口并复写intercept()方法，然后在给插件编写注解，指定要拦截哪一个接口的哪些方法即可，记住，别忘了在配置文件中配置你编写的插件。
```

