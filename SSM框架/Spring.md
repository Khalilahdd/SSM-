# Spring

## 1.1、简介

- Spring：春天------> 给软件行业带来了春天
- 2002，首次推出了Spring框架的雏形：interface21框架！
- Spring框架即以interface21框架为基础经过重新设计，并不断丰富其内涵，于2004年3月24日，分布了1.0正式版
- spring理念：使现有的技术更加容易使用，本身是一个大杂烩，整合现有的技术框架



- SSH：Struct2+Spring+Hibernate
- SSM：SpringMVC+Spring+Mybatis



# 

```java
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc --><br/>
<dependency><br/>    
    <groupId>org.springframework</groupId><br/>   
    <artifactId>spring-webmvc</artifactId><br/>   
    <version>5.2.11.RELEASE</version><br/>
    </dependency>
    
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc --><br/>
<dependency><br/>   
    <groupId>org.springframework</groupId><br/>   
    <artifactId>spring-jdbc</artifactId><br/>    
    <version>5.2.11.RELEASE</version><br/>
    </dependency>
```



## 1.2、优点

- Spring是一个开源的免费的框架（容器）！
- Spring是一个轻量级的、非入侵的框架！
- 控制反转（IOC），面向切面编程（AOP）！
- 支持事务的处理，对框架整合的支持！   



000





==**总结:Sping就是一个轻量级的控制反转（IOC)和面向切面编程的框架！**==





## 1.3、组成

| ![img](https://img-blog.csdn.net/20170713150400373?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzMwODI3MzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) |
| ------------------------------------------------------------ |



## 1.4、扩展

在Spring的官网有这个介绍：现代化的java开发！说白了就是基于Spring的开发！

- Spring Boot
  - 一个基于快速开发的脚手架
  - 基于SpringBoot可以快速的开发单个微服务

- Spring Cloud
  - Spring Cloud是基于SpringBoot实现的



因为现在大多数公司都在使用SpringBoot进行快速开发，学习SpringBoot的前提，需要完全掌握Spring及SpringMVC！承上启下的作用！



弊端：发展了太久之后，违背了原来的理念！配置十分繁琐，人称：“配置地狱”

# 2、IOC理论推导

1.UserDao接口

```java

public interface UserDao{

    void getUser();
}
```



2.UserDaoImpl实现类

```java
public class UserDaoImpl implements UserDao{

    public void getUser(){
        System.out.println("默认获取用户的数据");
    }
}
```



3.UserService业务接口

```java
public interface UserService {

    void getUser();
}

```



4.UserServiceImpl业务实现类

```java
import com.kuang.dao.UserDao;
import com.kuang.dao.UserDaoImpl;
import com.kuang.dao.UserDaoMysqlImpl;
import com.kuang.dao.UserDaoOracleImpl;

public class UserServiceImpl implements UserService{

    private UserDao userDao;

    //利用set进行动态实现值的注入！
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void getUser() {
        userDao.getUser();
    }
}

```





在我们之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改源代码！如果程序代码量十分大，修改一次的成本代价十分昂贵！

![image-20210728121909746](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210728121909746.png)

我们使用一个Set接口实现，已经发生了革命性的变化

```java
private UserDao userDao;

    //利用set进行动态实现值的注入！
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
```



- 之前，程序是主动创建对象！控制器在程序员手上！
- 使用了set注入之后，程序不再具有主动性，而是变成了被动的接收对象！



这种思想，从本质上解决了问题，我们程序员不用再去管理对象的创建了，系统的耦合性大大降低，可以更加专注在业务的实现上！这是IOC的原型!

![image-20210728122011824](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210728122011824.png)

## IOC本质

控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。



采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。**

Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从Ioc容器中取出需要的对象。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcyMDE4LmNuYmxvZ3MuY29tL2Jsb2cvMTQxODk3NC8yMDE5MDcvMTQxODk3NC0yMDE5MDcyNjExMjg1NTA3NC02Njk5OTg3OTYucG5n)

# 3、HelloSpring

## 	1、导入Spring相关的jar包

注 : spring 需要导入commons-logging进行日志记录 . 我们利用maven , 他会自动下载对应的依赖项 .

```java
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>4.3.7.RELEASE</version>
  </dependency>
```

## 	2、编写相关代码

### 		2.1 编写一个Hello实体类

```java
public class Hello {
     private String name;
 
     public String getName() {
         return name;
     }
     public void setName(String name) {
         this.name = name;
     }
 
     public void show(){
         System.out.println("Hello"+ name );
     }
 }
```

### 2.2 写编写我们的spring文件 , 这里我们命名为beans.xml

```java 
<?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--bean就是java对象 , 由Spring创建和管理-->
       <bean id="hello" class="com.kuang.pojo.Hello">
           <property name="name" value="Spring"/>
      </bean> 
  </beans>
```

### 2.3 我们可以去进行测试了 

```java
@Test
 public void test(){
     //解析beans.xml文件 , 生成管理相应的Bean对象
     ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
     //getBean : 参数即为spring配置文件中bean的id.
     Hello hello = (Hello) context.getBean("hello");
     hello.show();
 }
```

## 思考问题？

- Hello对象是谁创建的？

  hello对象是由Spring创建的

- Hello对象的属性是怎么设置的？

  hello对象的属性是由Spring容器设置的

这个过程就叫控制反转 :

控制 : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , 使用Spring后 , 对象是由Spring来创建的 .

反转 : 程序本身不创建对象 , 而变成被动的接收对象 .

依赖注入 : 就是利用set方法来进行注入的.

IOC是一种编程思想 , 由主动的编程变成被动的接收 .

可以通过new ClassPathXmlApplicationContext去浏览一下底层源码 .

**OK，到了现在 , 我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改 , 所谓的IoC,一句话搞定 : 对象由Spring 来创建 , 管理 , 装配 !**





# 4、IOC创建对象的方式

1、使用无参构造创建对象，默认！

```java
	 <bean id="user" class="com.kuang.pojo.User">
            <!--使用无参构造，用下面的方法-->
<!--        <property name="name" value="美女"/>-->
```



2、假设我们要使用有参构造创建对象。

​	1.下标赋值

```java
  <!--使用有参构造
            1.第一种，下标赋值！
        -->
        <constructor-arg index="0" value="迪丽热巴"/>
```

2.通过类型创建

```java
<!--<constructor-arg index="0" value="迪丽热巴"/>-->
            <!--
            2.第二种，通过类型创建
                不建议使用！
            -->
        <constructor-arg type="java.lang.String" value="古力娜扎"/>
```

3.直接通过参数名来设置

```java
<!--        第三种，直接通过参数名来设置-->
        <constructor-arg name="name" value="李知恩"/>
```

总结：在配置文件加载的数据，容器中管理的对象就已经初始化了！



# 5、Spring配置



## 5.1、别名

```java
    
<!--    别名，如果添加了别名，我们也可以使用别名获取到这个对象-->
    <alias name="user" alias="userNew"/>
```



## 5.2、Bean的配置

```java
<!--
        id：bean的唯一标识符，也就是相当于我们学的对象名
        class：bean对象所对应的全限定名：   包名 + 类型
        name:也是别名，而且name可以同时取多个别名
-->
    <bean id="userT" class="com.kuang.pojo.UserT" name="userT2,u2">
            <property name="name" value="干就完事了"/>
    </bean>
```



## 5.3、import

这个import，一般用于团队开发使用，他可以将多个配置文件。导入合并为一个

假设，现在项目中有多个人开发，这三人负责不同的类开发，不同的类需要注册在不同的bean中，我们可以利用import将所有人的beans.xml合并为一个总的！

- 张三

- 李四

- 王五

- applicationContext.xml

  ```java
  	<import resource="beans.xml"/>
      <import resource="beans2.xml"/>
      <import resource="beans3.xml"/>
  ```

  

使用的时候，直接使用总的配置就可以了

# 6、依赖注入



## 6.1、构造器注入

1、使用无参构造创建对象，默认！

```java

    <bean id="user" class="com.kuang.pojo.User">
            <!--使用无参构造，用下面的方法-->
<!--        <property name="name" value="美女"/>-->
```

2、假设我们要使用有参构造创建对象。

​	1.下标赋值

```java
  <!--使用有参构造
            1.第一种，下标赋值！
        -->
        <constructor-arg index="0" value="迪丽热巴"/>
```

2.通过类型创建

```java
<!--<constructor-arg index="0" value="迪丽热巴"/>-->
            <!--
            2.第二种，通过类型创建
                不建议使用！
            -->
        <constructor-arg type="java.lang.String" value="古力娜扎"/>
```

3.直接通过参数名来设置

```java
<!--        第三种，直接通过参数名来设置-->
        <constructor-arg name="name" value="李知恩"/>
```

总结：在配置文件加载的数据，容器中管理的对象就已经初始化了！



## 6.2、Set方式注入【重点】

- 依赖注入：Set注入
  - 依赖：bean对象的创建依赖于容器
  - 注入：bean对象中的所有属性，由容器来注入



【环境搭建】

1.复杂类型

```java
package com.kuang.pojo;

public class Address {

    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```

2.真实测试对象

```java
public class Student {

    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbies;
    private Map<String,String> card;
    private Set<String> games;
    private String wife;
    private Properties info;
    
}
```

3.beans.xml

```java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="student" class="com.kuang.pojo.Student">
<!--        第一种，普通值注入 value-->
        <property name="name" value="迪丽热巴"/>
    </bean>

</beans>
```

4.测试类

```java
public class MyTest {

    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Student student = (Student) context.getBean("student");
        System.out.println(student.getName());
    }
}
```

完善注入信息

```java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="address" class="com.kuang.pojo.Address">
        <property name="address" value="深圳"/>
    </bean>


    <bean id="student" class="com.kuang.pojo.Student">
<!--        第一种，普通值注入 value-->
        <property name="name" value="迪丽热巴"/>

<!--        第二种，Bean注入，value-->
        <property name="address" ref="address"/>

<!--        数组注入，ref-->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>西游记</value>
                <value>水浒传</value>
                <value>三国演义</value>
            </array>
        </property>

<!--        List-->
        <property name="hobbies">
            <list>
                <value>听歌</value>
                <value>敲代码</value>
                <value>看电影</value>
            </list>
        </property>

<!--        Map-->
        <property name="card">
            <map>
                <entry key="身份证" value="1111112345678999087"/>
                <entry key="银行卡" value="6645734355745354356"/>
            </map>
        </property>

<!--        Set-->
        <property name="games">
            <set>
                <value>LOL</value>
                <value>DNF</value>
            </set>
        </property>

<!--        null-->
        <property name="wife" value=""/>

<!--        Properties-->
        <property name="info">
            <props>
                <prop key="driver">191120412</prop>
                <prop key="url">男</prop>
                <prop key="username">小明</prop>
                <prop key="password">123456</prop>
            </props>
        </property>
    </bean>

</beans>
```





## 6.3、拓展方式注入

我们可以使用p命名空间和c命名空间进行注入

官方解释





使用：

```java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:P="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    P命令空间注入，可以直接注入属性的值，property-->
    <bean id="user" class="com.kuang.pojo.User" P:name="迪丽热巴" P:age="18"/>

<!--    c命名空间注入，通过构造器注入；construct-args -->
    <bean id="user2" class="com.kuang.pojo.User" c:age="18" c:name="古力娜扎"/>

</beans>
```

测试：

```java
   @Test
    public void test2(){
        ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
        User user = context.getBean("user2",User.class);
        System.out.println(user);
    }
```



注意点：p命名和c命名空间不能直接使用，需要导入xml约束！

```java
	   xmlns:P="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
```

## 6.4、Bean的作用域

![img](https://img-blog.csdn.net/20160417164310654?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

1.单例模式（Spring默认机制）

```java
 <bean id="user2" class="com.kuang.pojo.User" c:age="18" c:name="古力娜扎" scope="singleton"/>
```

2.原型模式:每次从容器中get的时候，都会产生一个新对象！

```java
 <bean id="user2" class="com.kuang.pojo.User" c:age="18" c:name="古力娜扎" scope="prototype"/>
```

3.其余的request、session、application，这些个只能在web开发中使用到！



# 7、Bean的自动装配

- 自动装配是Spring满足bean依赖的一种方式！
- Spring会在上下文中自动寻找，并自动给bean装配属性！



在Spring中有三种装配方式

​	1.在xml中显式的配置

​	2.在java中显式的配置

​	3.隐式的自动装配bean【重要】



## 7.1、测试

1.环境搭建

- 一个人有两个宠物！

  

## 7.2、ByName自动装配

```java
<!--        
        byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的bean id！
        -->
        <bean id="people" class="com.kuang.pojo.People" autowire="byName">
            <property name="name" value="迪丽热巴"/> 
        </bean>
```

## 7.3、ByType自动装配

```java
	 	<bean  class="com.kuang.pojo.Cat"/>
        <bean  class="com.kuang.pojo.Dog"/>

        <!--
        byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的bean id！
        byType:会自动在容器上下文中查找，和自己对象属性类型相同的bean！
        -->
        <bean id="people" class="com.kuang.pojo.People" autowire="byType">
            <property name="name" value="迪丽热巴"/>
        </bean>

```

小结：

- byname的时候，需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致！
- bytype的时候，需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性的类型一致！

## 7.4、使用注解实现自动装配

jdk1.5支持注解，Spring2.5支持注解了！

The introduction of annotation—based configuration raised the question of whether this approach is “better”  than XML。

要使用注解须知：

​	1.导入约束:context约束

​	2.配置注解的支持：==context:annotation-config/==    【重点】

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
    
    
    <context:annotation-config/>
    
</beans>
```



==@Autowired 由Spring容器提供，默认按byType==

直接在属性上使用即可！也可以在set方式上使用！

使用Autowired我们可以不用编写Set方法了，前提是你这个自动装配的属性在IOC（Spring）容器中存在，且符合名字byname！

科普：

```xml
@Nullable	字段标记了这个注解，说明这个字段可以为null
```

```java
public @interface Autowired {
    boolean required() default true;
}
```

测试代码：

```java
public class People {

    //如果显示定义了Autowired的required属性为false，说明这个对象可以为null，否则不允许为空
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
    
    
}
```



如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们可以使用@Qualifier(value = “xxx”)去配置@Autowired使用，指定一个唯一的bean对象注入！

```java
public class People {

    @Autowired
    @Qualifier(value = "cat11")
    private Cat cat;
    
    @Autowired
    @Qualifier(value = "dog222")
    private Dog dog;
    private String name;

}
```



**@Resource注解**

```java

public class People{
    
    @Resource(name = "cat2")
    private Cat cat;
    
    @Resource
    private Dog dog;
} 
```



## 7.5、小结

@Resource和@Autowired的区别：

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired通过byType的方式实现，而且必须要求对象存在！【常用】
- @Resource默认通过byname的方式实现，如果找不到名字，则通过byType实现！如果两个都找不到的情况下，就报错！
- 执行顺序不同：@Autowired通过byType的方式实现，@Resource默认通过byname的方式实现



# 8、使用注解开发



在Spring4之后，要使用注解开发，必须要保证aop的包导入了

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210730101218993.png" alt="image-20210730101218993" style="zoom:80%;" />

使用注解需要导入context约束，增加注解的支持！

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
    
    
    <context:annotation-config/>
    
</beans>
```

## 1.bean

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
    
    
    <context:annotation-config/>
    
</beans>
```

## 2.属性如何注入

```java
@Component
public class User {

//    public String name = "迪丽热巴";
    
    public String name;

    //相当于 <property name="name" value="古力娜扎"/>
    @Value("古力娜扎")
    public void setName(String name) {
        this.name = name;
    }
}

```

## 3.衍生的注解

@Component有几个衍生注解，我们在web开发中，会按照MVC三层架构分层！

- dao 【@Repository】

- service【@Service】

- controller【@Controller】

  这四个注解功能都是一样的，都是代表将某个类注入到Spring中，装配Bean

## 4.自动装配

```java
- @Autowired：自动装配通过类型、名字
    如果Autowired不能唯一自动装配上属性，则需要通过@Qualifier(value = "XXX")
- @Nullable	字段标记了这个注解，说明这个字段可以为null    
- @Resource：自动装配通过名字，类型
```

## 5.作用域

```java
@Component
@Scope("prototype")
public class User {

//    public String name = "迪丽热巴";

    public String name;

    //相当于 <property name="name" value="古力娜扎"/>
    @Value("古力娜扎")
    public void setName(String name) {
        this.name = name;
    }
}

```

## 6.小结

xml与注解：

- xml更加万能，适用于任何场合！维护简单方便！
- 注解不是自己类使用不了，维护相对复杂！

xml与注解最佳实践：

- xml用来管理bean；
- 注解只负责完成属性的注入；
- 我们在使用的过程中，只需要注意一个问题：必须让注解生效，就需要开启注解的支持

```xml
<!--    指定要扫描的包，这个包下的注解就会生效-->
    <context:component-scan base-package="com.kuang"/>
    <context:annotation-config/>
```



# 9、使用Java的方式配置的Spring

我们现在要完全不使用Spring的xml配置，全权交给java来做！

JavaConfig是Spring的一个子项目，在Spring 4之后，它成为了一个核心功能！

![image-20210730111328840](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210730111328840.png)

实体类

```java
package com.kuang.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

//这里这个注解的意思，就是说明这个类被Spring接管了，注册到容器中
@Component
public class User {

    private String name;

    public String getName() {
        return name;
    }

    @Value("迪丽热巴")//属性注入值
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

配置类

```java
package com.kuang.config;

import com.kuang.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

//这个也会被Spring容器托管，注入到容器中，因为它本来就是一个@Component,
//@Configuration 代表这是一个配置类，就和我们之前看的beans.xml
@Configuration
@ComponentScan("com.kuang.pojo")
@Import(KuangConfig.class)
public class KuangConfig {

    //注册一个bean，就相当于我们之前写的一个bean标签
    //这个方法的名字，就相当于bean标签中的id属性
    //这个方法的返回值，就相当于bean标签中的class属性
    @Bean
    public User getUser() {
        return new User();//就是返回要注入到bean的对象！
    }
}
```

测试类

```java
import com.kuang.config.KuangConfig;
import com.kuang.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MyTest {

    public static void main(String[] args) {
        //如果完全使用了配置类方法去做，我们就只能通过AnnotationConfig上下文来获取容器，通过配置类的class对象加载！
        ApplicationContext context = new AnnotationConfigApplicationContext(KuangConfig.class);
        User getUser = (User) context.getBean("user");
        System.out.println(getUser.getName());
    }
}
```



这种纯java的配置方式。在SpringBoot中随处可见！



# 10、代理模式

为什么要学习代理模式？因为这就是SpringAOP的底层！【SpringAOP和SpringMVC】

代理模式的分类：

- 静态代理
- 动态代理

![image-20210730151857564](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210730151857564.png)

## 10.1、静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
- 客户：访问代理对象的人！



代码步骤：

​	1.接口

```java
package com.kuang.demo01;

//租房
public interface Rent {

    public void rent();
}

```

​	2.真实角色

```java
package com.kuang.demo01;

//房东
public class Host implements Rent{

    @Override
    public void rent() {
        System.out.println("房东要出租房子！");
    }
}

```

​	3.代理角色

```java
package com.kuang.demo01;


public class Proxy implements Rent{

    private Host host;

    public Proxy() {
    }

    public Proxy(Host host) {
        this.host = host;
    }

    public void rent() {
        seeHouse();
        host.rent();
        hetong();
        fare();
    }

    //看房
    public void seeHouse() {
        System.out.println("中介带你看房");
    }

    //合同
    public void hetong(){
        System.out.println("签租赁合同");
    }

    //收中介费
    public void fare() {
        System.out.println("收中介费");
    }
}

```



​	4.客户端访问代理角色

```java
package com.kuang.demo01;

public class Client {

    public static void main(String[] args) {
        Host host = new Host();
        //代理，中介帮房东出租房子，但是呢？代理角色一般会有一些附属操作！
        Proxy proxy = new Proxy(host);

        //你不需要面对房东，直接找中介租房即可！
        proxy.rent();
    }
}
```

代理模式的好处：

- 可以使用真实角色的操作更加纯粹！不用去关注一些公共的业务
- 公共也就是交给代理角色！实现了业务的分工！
- 公共业务发生扩展的时候，方便集中管理！

缺点：

- 一个真实角色就会产生一个代理角色；代码量会翻倍~开发效率会变低



## 10.2、加深理解

1、接口

```java
package com.kuang.demo02;

public interface UserService {

    public void add();
    public void delete();
    public void update();
    public void query();
}

```

2、真实对象

```java
package com.kuang.demo02;

//真实对象
public class UserServiceImpl implements UserService{

    public void add() {
        System.out.println("增加了一个用户");
    }


    public void delete() {
        System.out.println("删除了一个用户");
    }


    public void update() {
        System.out.println("修改了一个用户");
    }


    public void query() {
        System.out.println("查询了一个用户");
    }
}

```

3、代理对象

```java
package com.kuang.demo02;

public class UserServiceProxy implements UserService{

    UserServiceImpl userService;

    public void setUserService(UserServiceImpl userService) {
        this.userService = userService;
    }

    public void add() {
        log("add");
        userService.add();
    }

    public void delete() {
        log("delete");
        userService.add();
    }

    public void update() {
        log("update");
        userService.add();
    }

    public void query() {
        log("query");
        userService.add();
    }

    //日志方法
    public void log(String msg){
        System.out.println("使用了" + msg + "方法");
    }
}

```

4、测试类

```java
package com.kuang.demo02;

public class Client {

    public static void main(String[] args) {
        UserServiceImpl userService = new UserServiceImpl();

        UserServiceProxy proxy = new UserServiceProxy();
        proxy.setUserService(userService);

        proxy.add();
    }
}

```

聊聊AOP

![image-20210730205520571](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210730205520571.png)

## 10.3、动态代理

- 动态代理和静态代理角色一样

- 动态代理的代理类是动态生成的，不是我们直接写好的！

- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
  - 基于接口--JDK动态代理【我们在这里使用】
  - 基于类：cglib
  - java字节码实现：javasist



需要了解两个类：Proxy：代理，InvocationHandler：调用处理程序

代理步骤：

1.接口

```java
package com.kuang.demo03;

//租房
public interface Rent {

    public void rent();
}
```

2.真实对象

```java
package com.kuang.demo03;

//房东
public class Host implements Rent {

    @Override
    public void rent() {
        System.out.println("房东要出租房子！");
    }
}
```

3.代理对象

```java
package com.kuang.demo03;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

//等会我们会用这个类，自动生成代理类！
public class ProxyInvitationHandler implements InvocationHandler {

//    Foo f = (Foo) Proxy.newProxyInstance(Foo.class.getClassLoader(),
//            new Class<?>[] { Foo.class },
//            handler);

    //被代理的接口
    private Rent rent;

    public void setRent(Rent rent) {
        this.rent = rent;
    }

    //生成得到的代理类
    public Object getProxy() {
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),rent.getClass().getInterfaces(),this);
    }

    //处理代理实例，并返回结果
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        //动态代理的本质，就是使用反射机制实现！
        seeHouse();
        Object result = method.invoke(rent, args);
        fare();
        return result;
    }

    public void seeHouse() {
        System.out.println("中介带看房子");
    }

    public void fare() {
        System.out.println("收中介费");
    }
}
```

4.测试类

```java
package com.kuang.demo03;

public class Client { 

    public static void main(String[] args) {
        //真实角色
        Host host = new Host();

        //代理角色，现在没有
        ProxyInvitationHandler pih = new ProxyInvitationHandler();
        //通过调用程度处理角色来处理我们要调用的接口对象！
        pih.setRent(host);
        Rent proxy = (Rent) pih.getProxy();//这里的proxy就是动态生成的，我们并没有写~
        proxy.rent();
    }
}

```

**ProxyInvitationHandler 类：该类中的代理接口改变，只需要做相应的修改，可以当成一个模板来使用**（**代理的是一类接口**）

```java
package com.kuang.demo04;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

//等会我们会用这个类，自动生成代理类！
public class ProxyInvitationHandler implements InvocationHandler {

//    Foo f = (Foo) Proxy.newProxyInstance(Foo.class.getClassLoader(),
//            new Class<?>[] { Foo.class },
//            handler);

    //被代理的接口
    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }

    //生成得到的代理类
    public Object getProxy() {
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),target.getClass().getInterfaces(),this);
    }

    //处理代理实例，并返回结果
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log(method.getName());
        //动态代理的本质，就是使用反射机制实现！
        Object result = method.invoke(target, args);
        return result;
    }

    public void log(String msg) {
        System.out.println("执行了" + msg + "方法");
    }
}

```



动态代理的好处：

- 可以使用真实角色的操作更加纯粹！不用去关注一些公共的业务

- 公共也就是交给代理角色！实现了业务的分工！

- 公共业务发生扩展的时候，方便集中管理！

- 一个动态代理类代理的是一个接口，一般就是对应的一类业务

- 一个动态代理类可以代理多个类，只要是实现了同一个接口即可！

  

# 11、AOP

## 11.1 什么是AOP

AOP为（Aspect Oriented Programming），意为：[面向切面编程](https://baike.baidu.com/item/面向切面编程/6016335)，通过[预编译](https://baike.baidu.com/item/预编译/3191547)方式和运行期间动态代理实现程序功能的统一维护的一种技术。AOP是[OOP](https://baike.baidu.com/item/OOP)的延续，是软件开发中的一个热点，也是[Spring](https://baike.baidu.com/item/Spring)框架中的一个重要内容，是[函数式编程](https://baike.baidu.com/item/函数式编程/4035031)的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的[耦合度](https://baike.baidu.com/item/耦合度/2603938)降低，提高程序的可重用性，同时提高了开发的效率。

![img](https://img-blog.csdnimg.cn/20200525204324152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoYXNlbWFucm9zZQ==,size_16,color_FFFFFF,t_70)

## 11.2 AOP在Spring中的作用

==提供声明式事务：允许用户自定义切面==

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，完全，缓冲，事务等等····

- 切面（ASPECT）：横切关注点被模块化的特殊对象。即，它是一个类。

- 通知（Advice）: 切面必须要完成的工作。即，它是类中的一个方法。

- 目标（Target）：被通知的对象。

- 代理（Proxy）：向目标对象应用通知之后创建的对象。

- 切入点（PointCut）：切面通知执行的“地点”的定义。

- 连接点（JointPoint）：与切入点匹配的执行点。

  ![img](https://img-blog.csdnimg.cn/20200525204515190.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoYXNlbWFucm9zZQ==,size_16,color_FFFFFF,t_70)

  

SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice：

![image-20210731134920895](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210731134920895.png)

即AOP在不改变原代码的情况下，去增加新的功能。



## 11.3 使用Spring实现AOP

【重点】使用AOP织入，需要导入一个依赖包！

```xml

    <dependencies>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
        </dependency>
    </dependencies>
```



### 方式一：使用Spring的API接口【主要Spring接口实现】

1.接口类

```java 
package com.kuang.service;

public interface UserService {

    public void add();
    public void delete();
    public void update();
    public void select();
}

```

2.真实对象

```java
package com.kuang.service;

public class UserServiceImpl implements UserService {

    public void add() {
        System.out.println("增加了一个用户");
    }


    public void delete() {
        System.out.println("删除了一个用户");
    }


    public void update() {
        System.out.println("更新了一个用户");
    }


    public void select() {
        System.out.println("查询了一个用户");
    }
}
```

3.前置通知

```java
package com.kuang.log;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

public class Log implements MethodBeforeAdvice {

    //method:要执行的目标对象的方法
    //args：参数
    //target：目标对象
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName() + "的" + method.getName() + "被执行了");
    }
}
```

4.后置通知

```java
package com.kuang.log;

import org.springframework.aop.AfterReturningAdvice;

import java.lang.reflect.Method;

public class AfterLog implements AfterReturningAdvice {

    //returnValue:返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了" + method.getName() + "方法，返回结果为： " + returnValue);
    }
}

```

5.测试类

```java
import com.kuang.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userService");

        userService.select();
    }
}
```

applicationContext.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/aop
           http://www.springframework.org/schema/aop/spring-aop.xsd">
<!--注册bean-->
    <bean id="userService" class="com.kuang.service.UserServiceImpl"/>
    <bean id="log" class="com.kuang.log.Log"/>
    <bean id="afterLog" class="com.kuang.log.AfterLog"/>


<!--    方式一：使用原生Spring API接口-->
<!--    配置aop:需要导入aop的约束-->
    <aop:config>
<!--        切入点:expression：表达式，execution(需要执行的位置！*****)-->
        <aop:pointcut id="pointcut" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>

<!--        执行环绕增加！-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>

</beans>
```

### 方式二：自定义类来实现【主要是切面定义】

1.接口类

```java 
package com.kuang.service;

public interface UserService {

    public void add();
    public void delete();
    public void update();
    public void select();
}
```

2.真实对象类

```java 
package com.kuang.service;

public class UserServiceImpl implements UserService{

    public void add() {
        System.out.println("增加了一个用户");
    }


    public void delete() {
        System.out.println("删除了一个用户");
    }


    public void update() {
        System.out.println("更新了一个用户");
    }


    public void select() {
        System.out.println("查询了一个用户");
    }
}

```

3.代理类

```java 
package com.kuang.diy;

public class DiyPointCut {

    public void before() {
        System.out.println("========方法执行前=========");
    }

    public void after() {
        System.out.println("========方法执行后=========");
    }
}

```

4.测试类

```java
import com.kuang.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userService");

        userService.select();
    }
}

```

applicationContext.xml配置文件

```xml
		<!--方式二：自定义类-->
    <bean id="diy" class="com.kuang.diy.DiyPointCut"/>

    <aop:config>
		<!--自定义切面，ref 要引用的类-->
        <aop:aspect ref="diy">
		<!--切入点-->
            <aop:pointcut id="point" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>
		<!--通知-->
            <aop:before method="before" pointcut-ref="point"/>
            <aop:after method="after" pointcut-ref="point"/>
        </aop:aspect>
    </aop:config>
```

### 方式三：使用注解实现！

1.接口类

```java
package com.kuang.service;

public interface UserService {

    public void add();
    public void delete();
    public void update();
    public void select();
}
```

2.真实对象类

```java
package com.kuang.service;

public class UserServiceImpl implements UserService{

    public void add() {
        System.out.println("增加了一个用户");
    }


    public void delete() {
        System.out.println("删除了一个用户");
    }


    public void update() {
        System.out.println("更新了一个用户");
    }


    public void select() {
        System.out.println("查询了一个用户");
    }
}

```

3.代理类

```java
package com.kuang.diy;

//方式三：使用注解方式实现AOP

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect//标注这个类是一个切面
public class AnnotationPointCut {

    @Before("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void before() {
        System.out.println("===========方法执行前=============");
    }

    @After("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void after() {
        System.out.println("===========方法执行后=============");
    }

    //在环绕增强中，我们可以给定一个参数，代表我们要获取处理切入的点；
    @Around("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint jp) throws Throwable {
        System.out.println("环绕前");

        Signature signature = jp.getSignature();//获取签名
        System.out.println("signature" + signature);
        //执行方法
        Object proceed = jp.proceed();

        System.out.println("环绕后");

        System.out.println(proceed);
    }
}
```

4.测试类

```java
import com.kuang.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userService");

        userService.select();
    }
}
```

applicationContext.xml配置文件

```xml
<!--    方式三-->
    <bean id="annotationPointCut" class="com.kuang.diy.AnnotationPointCut"/>
<!--    开启注解支持！     JDK(默认：proxy-target-class="false")     cglib(proxy-target-class="true")-->
    <aop:aspectj-autoproxy/>
```

# 12、整合Mybatis

步骤：

 1.导入相关jar包

- junit
- mybatis
- mysql数据库
- spring相关的
- aop织入
- mybatis-spring【new 】

2.编写配置文件

```xml
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.0.RELEASE</version>
        </dependency>
<!--        Spring操作数据库的话，还需要一个spring-jdbc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.2</version>
        </dependency>
    </dependencies>
```

3.测试





## 12.1、回忆mybatis

1.编写实体类

2.编写核心配置文件

3.编写接口

4.编写Mapper.xml

5.测试



## 12.2、mybatis-spring

1.编写数据源配置

```xml
  <!--DataSource:使用Spring数据源替换Mybatis的配置  c3p0 dbcp druid
    我们这里使用Spring提供的JDBC:org.springframework.jdbc.datasource.DriverManagerDataSource
    -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url"
                  value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;userUnicode=true&amp;characterEncoding=UTF-8"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>
```

2.sqlSessionFactory

```xml
<!--SQLSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--绑定Mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/kuang/mapper/UserMapper.xml"/>
    </bean>
```

3.SQLSessionTemplate

```xml
<!--SqlSessionTemplate:就是我们使用的sqlSession-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!--只能使用构造器注入SqlSessionTemplate，因为它没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>
```

4.需要给接口加实现类【】

```java
package com.kuang.mapper;

import com.kuang.pojo.User;
import org.mybatis.spring.SqlSessionTemplate;

import java.util.List;

public class UserMapperImpl implements UserMapper{

    //我们的所以操作，都使用SqlSession来执行，在原来，现在都使用SQLSessionTemplate；
    private SqlSessionTemplate sqlSession;

    public void setSqlSession(SqlSessionTemplate sqlSession) {
        this.sqlSession = sqlSession;
    }

    public List<User> selectUser() {
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.selectUser();
    }
}
```

5.将自己写的实现类，注入到Spring中

6.测试类

```java
import com.kuang.mapper.UserMapper;
import com.kuang.pojo.User;
import org.junit.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    @Test
    public void test() {
//        SqlSession sqlSession = MybatisUtils.getSqlSession();
//
//        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
//
//        List<User> users = mapper.selectUser();
//
//        for (User user : users) {
//            System.out.println(user);
//        }
//
//
//        sqlSession.close();

        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        UserMapper userMapper = context.getBean("userMapper", UserMapper.class);

        for (User user : userMapper.selectUser()) {
            System.out.println(user);
        }
    }
}
```



# 13、声明式事务

## 1、简介

- 把一组业务当成一个业务来做；要么都成功，要么都失败！
- 事务在项目开发中，十分重要，涉及到数据的一致性问题，不能马虎！
- 确保完整性和一致性！



事务ACID原则：

- 原子性
- 一致性
- 隔离性
  - 多个业务可能操作同一个资源，防止数据损坏
- 持久性
  - 事务一旦提交，无论系统发生什么问题，结果都不会再被影响，被持久化的写到存储器中！



## 2、spring中的事务管理

- 声明式事务：AOP
- 编程式事务：需要在代码中，进行事务的管理

```xml
 	<!--配置声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="dataSource"/>
    </bean>

    <!--结合AOP实现事务的织入-->
    <!--配置事务的通知：-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--给那些方法配置事务-->
        <!--配置事务的传播特性：new-->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>


    <!--配置事务的切入-->
    <aop:config>
        <aop:pointcut id="txPointCut" expression="execution(* com.kuang.mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
    </aop:config>

```

思考：

为什么需要事务！

- 如果不配置事务，可能存在数据提交不一致的情况下；
- 如果我们不做spring中去配置声明式事务，我们就需要在代码中手动配置事务！
- 事务在项目的开发中十分重要，设计到数据的一致性和完整性问题，不容马虎！
