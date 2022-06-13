# Mybatis-9.28

环境：

- JDK1.8
- Mysql 5.7
- maven 3.6.1
- IDEA

回顾：

- JDBC

- Mysql

- Java基础

- Maven

- Junit

ssm框架:配置文件的。最好的方式：看官网文档；

# 1、简介

## 1.1、什么是Mybatis

![image-20210801110627204](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210801110627204.png)

- MyBatis 是一款优秀的**持久层框架**。
- 它支持自定义 SQL、存储过程以及高级映射。
- MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。
- MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
- MyBatis 本是apache的一个[开源项目](https://baike.baidu.com/item/开源项目/3406069)iBatis, 2010年这个[项目](https://baike.baidu.com/item/项目/477803)由apache software foundation 迁移到了[google code](https://baike.baidu.com/item/google code/2346604)，并且改名为MyBatis 。
- 2013年11月迁移到[Github](https://baike.baidu.com/item/Github/10145341)。

如何获取Mybatis？

- maven仓库

- GitHub：[Releases · mybatis/mybatis-3 (github.com)](https://github.com/mybatis/mybatis-3/releases)

- 中文文档：[mybatis – MyBatis 3 | 简介](https://mybatis.org/mybatis-3/zh/index.html)

## 1.2、持久化

数据持久化

- 持久化就是将程序的数据在持久状态和瞬时状态转化的过程

- 内存：**断电即失**

- 数据库（JDBC），io文件持久化。

- 生活：冷藏，罐头

**为什么需要持久化？**

- 有一些对象，不能让他丢掉。

- 内存太贵了

## 1.3、持久层

Dao层，Service层，Controller层…

- 完成持久化工作的代码块
- 层界限十分明显。

## 1.4、为什么需要Mybatis

- 帮助程序员将数据存入到数据库中

- 方便

- 传统的JDBC代码实在太复杂了，简化。框架。

- 不用Mybatis也可以。更容易上手。**技术没有高低之分。**

- 优点：
  
  - 简单易学：本身就很小且简单。没有任何第三方依赖，最简单安装只要两个jar文件+配置几个sql映射文件易于学习，易于使用，通过文档和源代码，可以比较完全的掌握它的设计思路和实现。
  - 灵活：mybatis不会对应用程序或者数据库的现有设计强加任何影响。 sql写在xml里，便于统一管理和优化。通过sql语句可以满足操作数据库的所有需求。
  - 解除sql与程序代码的耦合：通过提供DAO层，将业务逻辑和数据访问逻辑分离，使系统的设计更清晰，更易维护，更易单元测试。sql和代码的分离，提高了可维护性。
  - 提供映射标签，支持对象与数据库的orm字段关系映射
  - 提供对象关系映射标签，支持对象关系组建维护
  - 提供xml标签，支持编写动态sql。 [2]

**最重要的一点：使用的人多！**

Spring     SpringMVC     SpringBoot

# 2、第一个Mybatis程序

思路：搭建环境-->导入Mybatis-->编写代码–>测试！

## 2.1、搭建环境

搭建数据库

```java
drop database if exists mybatis;

Create database mybatis default character set utf8;

use mybatis;

create table user(
    id int(20) NOT NULL,
    name varchar(30) DEFAULT NULL,
    pwd varchar(30) DEFAULT NULL,
    primary key(id)
);

insert into user(id,name,pwd) values
(1,'迪丽热巴','123456'),
(2,'古力娜扎','12345'),
(3,'张三','1234523')
```

新建项目

​    1.新建一个普通的Maven项目

​    2.删除src目录

​    3.导入maven依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>Myabtis-Study</artifactId>
    <version>1.0-SNAPSHOT</version>


<!--    导入依赖-->
    <dependencies>
<!--        mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
<!--        mybatis-->
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.6</version>
        </dependency>
<!--        junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>

</project>
```

## 2.2、创建一个模块

- 编写mybatis的核心配置文件
  
  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <!--configuration核心配置文件-->
  <configuration>
  
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                  <property name="driver" value="com.mysql.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&userUnicode=true&characterEncoding=UTF-8"/>
                  <property name="username" value="root"/>
                  <property name="password" value="root"/>
              </dataSource>
          </environment>
      </environments>
  </configuration>
  ```

- 编写mybatis工具类

```java
package com.kuang.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

//SqlSessionFactory -->  sqlSession
public class MybatisUtils {

    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            //使用Mybatis第一步：获取SqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。
    // SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。

    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession(true);
    }
}
```

## 2.3、编写代码

- 实体类
  
  ```java
  package com.kuang.pojo;
  
  //实体类
  public class User {
  
      private int id;
      private String name;
      private String pwd;
  
      public User() {
      }
  
      public User(int id, String name, String pwd) {
          this.id = id;
          this.name = name;
          this.pwd = pwd;
      }
  
      public int getId() {
          return id;
      }
  
      public void setId(int id) {
          this.id = id;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public String getPwd() {
          return pwd;
      }
  
      public void setPwd(String pwd) {
          this.pwd = pwd;
      }
  
      @Override
      public String toString() {
          return "User{" +
                  "id=" + id +
                  ", name='" + name + '\'' +
                  ", pwd='" + pwd + '\'' +
                  '}';
      }
  }
  ```

- Dao接口
  
  ```java
  public interface UserDao {
  
      List<User> getUserList();
  }
  ```

- 接口实现类由原来的UserDaoImpl转变为一个Mapper配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace=绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.kuang.dao.UserDao">
<!--    select查询语句-->
    <select id="getUserList" resultType="com.kuang.pojo.User">
        select * from mybatis.user;
    </select>

</mapper>
```

## 2.4、测试

**注意点：**

org.apache.ibatis.binding.BindingException: Type interface com.kuang.dao.UserDao is not known to the MapperRegistry.

**MapperRegistry是什么？**

核心配置文件中注册mappers

```xml
<!--Intellij idea 报错：Error : java 不支持发行版本14 配置下面的信息-->
    <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <java.version>14</java.version>
    <maven.compiler.source>14</maven.compiler.source>
    <maven.compiler.target>14</maven.compiler.target>
    <encoding>UTF-8</encoding>
    </properties>

    <!--（在所以Maven项目中，先配置该文件）    在build中配置resource，来防止资源导出失败的问题-->
    <build>
        <resources>
            <resource>
                <directory>src/main/resource</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
<!--<filtering>true</filtering>-->
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
<!--<filtering>true</filtering>-->
            </resource>
        </resources>
    </build>
```

- junit测试

```java
public class UserDaoTest {

    @Test
    public void test() {

        //第一步：获得SqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        //方式一：getMapper
        UserDao userDao = sqlSession.getMapper(UserDao.class);
        List<User> userList = userDao.getUserList();

        for (User user : userList) {
            System.out.println(user);
        }

        //关闭SqlSession
        sqlSession.close();
    }
}
```

可能会遇到的问题：

​    1.配置文件没有注册

​    2.绑定接口错误。

​    3.方法名不对

​    4.返回类型不对

​    5.Maven导出资源问题

# 3、CRUD

## 1、namespace

namespace中的包名要和Dao/mapper接口的包名一致！

## 2、select

选择，查询语句

- id：就是对应的namespace中的方法名；
- resultType：SQL语句执行的返回值！
- parameterType：参数类型！

1.编写接口

```java
    //查询全部用户
    List<User> getUserList();
    //根据ID查询用户
    User getUserById(int id);
```

2.编写对应的Mapper中的SQL语句

```xml
<!--    select查询语句-->
    <select id="getUserList" resultType="com.kuang.pojo.User">
        select * from mybatis.user;
    </select>

    <select id="getUserById" parameterType="int" resultType="com.kuang.pojo.User">
        select * from mybatis.user where id = #{id}
    </select>
```

3.测试

```java
    @Test
    public void test() {

        //第一步：获得SqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        try {
            //方式一：getMapper
            UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
            List<User> userList = userMapper.getUserList();

            //方式二：
            //List<User> userList = sqlSession.selectList("com.kuang.UserDao.getUserList");

            for (User user : userList) {
                System.out.println(user);
            }

        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            //关闭SqlSession
            sqlSession.close( );
        }
    }

    @Test
    public void getUserById() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        User user = mapper.getUserById(1);
        System.out.println(user);

        sqlSession.close( );
    }
```

## 3、Insert

1.编写接口

```xml
    //insert一个用户
    int addUser(User user);
```

2.编写对应的mapper中的SQL语句

```xml
<!--    对象中的属性，可以直接取出来-->
    <insert id="addUser" parameterType="com.kuang.pojo.User">
        insert into mybatis.user(id,name,pwd) value (#{id},#{name},#{pwd});
    </insert>
```

3.测试

```java
//增删改需要提交事务
    @Test
    public void addUser() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        int res = mapper.addUser(new User(4,"佟丽娅","245433"));

        if (res > 0) {
            System.out.println("插入成功！");
        }

        //提交事务
        sqlSession.commit();
        sqlSession.close( );
    }
```

## 4、Update

1.编写接口

```java
    //修改用户
    int updateUser(User user);
```

2.编写对应的mapper中的sql语句

```xml
  <update id="updateUser" parameterType="com.kuang.pojo.User">
        update mybatis.user set name = #{name},pwd=#{pwd} where id=#{id};
    </update>
```

3.测试

```java
 @Test
    public void updateUser() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        mapper.updateUser(new User(4,"高圆圆","2233"));

        sqlSession.commit();
        sqlSession.close( );
    }
```

## 5、Delete

1.编写接口

```java
 //删除一个用户
    int deleteUser(int id);
```

2.编写对应的mapper中的SQL语句

```xml
    <delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id = #{id};
    </delete>
```

3.测试

```java
    @Test
    public void deleteUser() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        mapper.deleteUser(4);

        sqlSession.commit();
        sqlSession.close();
    }
```

注意点：

- 增删改需要提交事务！

## 6、分析错误

- 标签不要匹配错
- resource绑定mapper，需要使用路径！
- 程序配置文件必须符合规范！
- NullPointerException，没有注册到资源！
- 输出的xml文件中存在中文乱码问题！
- maven资源没有导出问题！

## 7、万能的Map

假设，我们的实体类，或者数据库中的表，字段或者参数过多，我们应当考虑使用Map！

1.接口类

```java
   //万能的Map
    User addUser2(Map<String,Object> map);
```

2.编写Mapper中的sql语句

```xml
    <!--    对象中的属性，可以直接取出来
        传递map中的key
    -->
    <insert id="addUser2" parameterType="map">
        insert into mybatis.user(id,name,pwd) value (#{userid},#{username},#{password});
    </insert>
```

3.测试

```java
@Test
    public void addUser2() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        HashMap<String, Object> map = new HashMap<>();
        map.put("userid",5);
        map.put("username","Hello");
        map.put("password","222333");

        mapper.addUser2(map);

        //提交事务
        sqlSession.commit();
        sqlSession.close();
    }
```

Map传递参数，直接在SQL中取出key即可！【parameterType=“map”】

对象传递参数，直接在sql中取出对象的属性即可！【parameterType="Object"】

只有一个基本类型参数的情况下，可以直接在SQL中取到！

多个参数用Map，**或者注解！**

## 8、思考题

模糊查询怎么写？

1.java代码执行的时候，传递通配符%#%

```java
List<User> userLike = mapper.getUserLike("%张%");
```

2.在sql拼接中使用通配符！

```xml
select * from mybatis.user where name like "%"#{value}"%"
```

# 4、配置解析

## 1、核心配置文件

- mybatis-config.xml

- Mybatis的配置文件中包含了会深深影响Mybatis行为的设置和属性信息。
  
  ```xml
  configuration（配置）
  properties（属性）
  settings（设置）
  typeAliases（类型别名）
  typeHandlers（类型处理器）
  objectFactory（对象工厂）
  plugins（插件）
  environments（环境配置）
  environment（环境变量）
  transactionManager（事务管理器）
  dataSource（数据源）
  databaseIdProvider（数据库厂商标识）
  mappers（映射器）
  ```

## 2、环境配置（environments）

MyBatis 可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

学会使用配置多套运行环境！(**将default修改就可以转到另外的运行环境！**)

```xml
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&userUnicode=true&characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>

        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&userUnicode=true&characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
```

Mybatis默认的事务管理器就是JDBC，连接池:POOLED

```xml
<environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&userUnicode=true&characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
```

## 3、属性（properties）

我们可以通过properties属性来实现引用配置文件

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。【db.properties】

编写一个配置文件

db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=true&userUnicode=true&characterEncoding=UTF-8"
username=root
password=root
```

在核心配置文件中引入

```xml
 <properties resource="db.properties">
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </properties>
```

- 可以直接引入外部文件
- 可以在其中增加一些属性配置
- 如果两个文件有同一个字段，优先使用外部配置文件的！

## 4、类型别名（typeAliases）

- 类型别名可为 Java 类型设置一个缩写名字。 
- 存在的意义仅在于用来降低冗余的全限定类名书写

```xml
<!--    可以给实体类别名-->
    <typeAliases>
        <typeAlias type="com.kuang.pojo.User" alias="User"/>
    </typeAliases>
```

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean.

扫描实体类的包，它默认别名就为这个类的类名，首字母小写！

```xml
<!--    可以给实体类别名-->
    <typeAliases>
<!--        <typeAlias type="com.kuang.pojo.User" alias="User"/>-->
        <package name="com.kuang.pojo"/>
    </typeAliases>
```

在实体类比较少的时候，使用第一种方式。

如果实体类十分多，建议使用第二种。

第一种可以DIY别名，第二种则不行，如果非要改，需要在实体类上增加注解

```java
@Alias("User")
public class User{

}
```

## 5、设置

这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。 

![image-20210803105827854](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210803105827854.png)

![image-20210803115817882](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210803115817882.png)

## 6、其他配置

- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
- [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
  - mybatis-generator-core
  - mybatis-plus
  - 通用mapper

## 7、映射器（mappers）

MapperRegistry：注册绑定我们的Mapper文件；

方式一：【推荐使用】

```xml
<!--    每一个Mapper.xml都需要在Mybatis核心配置文件中注册！-->
    <mappers>
        <mapper resource="com/kuang/dao/UserMapper.xml"/>
    </mappers>
```

方式二：使用class文件绑定注册

```xml
<!--    每一个Mapper.xml都需要在Mybatis核心配置文件中注册！-->
    <mappers>
<!--        <mapper resource="com/kuang/dao/UserMapper.xml"/>-->
        <mapper class="com.kuang.dao.UserMapper"/>
    </mappers>
```

注意点：

- 接口和它的Mapper配置文件必须同名
- 接口和它的Mapper配置文件必须在同一个包下！

方式三：使用扫描包进行注入绑定

```xml
<!--    每一个Mapper.xml都需要在Mybatis核心配置文件中注册！-->
    <mappers>
<!--        <mapper resource="com/kuang/dao/UserMapper.xml"/>-->
<!--        <mapper class="com.kuang.dao.UserMapper"/>-->
        <package name="com.kuang.dao"/>
    </mappers>
```

注意点：

- 接口和它的Mapper配置文件必须同名！
- 接口和它的Mapper配置文件必须在同一个包下！

练习：

- 将数据库配置文件外部引入！
- 实体类别名！
- 包在UserMapper接口和UserMapper.xml改为一致，并且放在同一个包下！

## 8、生命周期和作用域

![image-20210803124502954](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210803124502954.png)

生命周期和作用域是至关重要的，因为错误的使用会导致非常严重的并发问题。

**SQLSessionFactoryBuilder：**

- 一旦创建了 SqlSessionFactory，就不再需要它了
- 局部变量

**SQLSessionFactory：**

- 说白了就是可以想象成：数据库连接池、
- SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它或重新创建另一个实例**。
- 因此 SqlSessionFactory 的最佳作用域是应用作用域。 
- 最简单的就是使用单例模式或者静态单例模式。

**SQLSession：**

- 连接到连接池的一个请求！
- SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。
- 用完之后需要赶紧关闭，否则资源被占用！

![image-20210803125428426](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210803125428426.png)

这里面的每一个Mapper，就代表一个具体的业务！

# 5、解决属性名和字段名不一致的问题

## 1、测试

数据库中的字段

![image-20210803125701322](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210803125701322.png)

新建一个项目，拷贝之前的，测实体类不一致的情况

```java
public class User {

    private int id;
    private String name;
    private String password;
```

测试结果：

![image-20210803153103217](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210803153103217.png)

```java
 // select * from mybatis.user where id = #{id}
    //类型处理器
    // select id,username,pwd from mybatis.user where id = #{id}
```

解决方法：

- 起别名

```xml
    <select id="getUserById" parameterType="int" resultType="com.kuang.pojo.User">
        select id,name,pwd as password from mybatis.user where id = #{id}
    </select>
```

## 2、resultMap

结果集映射

```
id name pwd
id name password
```

```xml
<!--    结果集映射-->
    <resultMap id="UserMap" type="User">
<!--        column 数据库中的字段，property实体类中的属性-->
        <result column="id" property="id"/>
        <result column="name" property="name"/>
        <result column="pwd" property="password"/>
    </resultMap>

    <select id="getUserById" resultMap="UserMap">
        select * from mybatis.user where id = #{id}
    </select>
```

- `resultMap` 元素是 MyBatis 中最重要最强大的元素。
- ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。
- ResultMap最优秀的地方在于，虽然你已经对它相对了解了，但是根本不需要显式地用到他们。
- 如果这个世界总是这么简单就好了。

# 6、日志

## 6.1、日志工厂

如果一个数据库操作，出现了异常，我们需要排错，日志就是最好的助手！

曾经：sout、debug

现在：日志工厂

![image-20210803161000399](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210803161000399.png)

- SLF4J
- LOG4J【掌握】
- LOG4J2
- JDK_LOGGING
- COMMONS_LOGGING
- STDOUT_LOGGING 【掌握】
- NO_LOGGING

在Mybatis中具体使用哪一个日志实现，在设置中设定！

**STDOUT_LOGGING标准日志输出**

在mybatis核心配置文件中，配置我们的日志

```xml
<!--    设置日志文件-->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
```

![image-20210803165339420](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210803165339420.png)

## 6.2、LOG4J

什么是LOG4J？

- Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件

- 我们也可以控制每一条日志的输出格式

- 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程

- 通过一个[配置文件](https://baike.baidu.com/item/配置文件/286550)来灵活地进行配置，而不需要修改应用的代码

1.先导入log4j的包

```xml
    <!-- https://mvnrepository.com/artifact/log4j/log4j -->
    <dependencies>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
    </dependencies>
```

2.log4j.properties

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/kuang.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

3.配置log4j为日志的实现

```xml
<!--    设置日志文件-->
    <settings>
<!--        标准的日志工厂实现-->
        <setting name="logImpl" value="LOG4J"/>
    </settings>
```

4.log4j的使用！直接运行刚才的查询

![image-20210804084432677](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210804084432677.png)

**简单使用**

1.在要使用log4j的类中，导入包import org.apache.log4j.Logger;

2.日志对象，参数为当前类的class

```java
 static Logger logger = Logger.getLogger(UserDaoTest.class);
```

3.日志级别

```java
        logger.info("info:进入了testLog4j");
        logger.debug("debug:进入了testLog4j");
        logger.error("error:进入了testLog4j");
```

# 7、分页

**思考：为什么要分页？**

- 减少数据的处理量

## 7.1、Limit分页

使用limit分页

```sql
select * from user limit startIndex,pageSize;
select * from user limit 3;#[0,n]
```

使用Mybatis实现分页，核心SQL

1.接口

```java
    //分页
    List<User> getUserByLimit(Map<String,Integer> map);
```

2.Mapper.xml

```xml
    <select id="getUserByLimit" parameterType="map" resultMap="UserMap">
        select * from mybatis.user limit #{startIndex},#{pageSize}
    </select>
```

3.测试

```java
    @Test
    public void getUserByLimit() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        HashMap<String, Integer> map = new HashMap<>();
        map.put("startIndex",0);
        map.put("pageSize",2);

        List<User> userList = mapper.getUserByLimit(map);

        for (User user : userList) {
        System.out.println(user);
        }

        sqlSession.close();
    }
```

## 7.2、RowBounds分页

不再使用SQL实现分页

1.接口

```java
    //分页2
    List<User> getUserByRowBounds();
```

2.mapper.xml

```xml
<!--分页2-->
    <select id="getUserByRowBounds" resultMap="UserMap">
        select * from mybatis.user
    </select>
```

3.测试

```java
    @Test
    public void getUserByRowBounds() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        //通过RowBounds实现
        RowBounds rowBounds = new RowBounds(1, 2);


        //通过Java代码层面实现分页
        List<User> userList = sqlSession.selectList("com.kuang.dao.UserMapper.getUserByRowBounds",null,rowBounds);

        for (User user : userList) {
            System.out.println(user);
        }

        sqlSession.close();
    }
```

## 7.3、分页插件

![image-20210804103525560](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210804103525560.png)

了解即可，万一以后的公司的架构师说要使用，你需要知道它是什么东西！

# 8、使用注解开发

## 8.1、面向接口编程

大家之前都学过面向对象编程，也学过接口，但在真正的开发中，很多时候我们会选择面向接口编程

**根本原因：==解耦==，可拓展，提高复用，分层开发中，上层不用管具体的实现，大家都遵守共同的标准，使得开发变得容易，规范性更好**

在一个[面向对象](https://baike.baidu.com/item/面向对象)的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的,对[系统设计](https://baike.baidu.com/item/系统设计)人员来讲就不那么重要了；

而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。

**关于接口的理解。**

- 接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。

- 接口的本身反映了系统设计人员对系统的抽象理解。

- 接口应有两类：
  
  - 第一类是对一个个体的抽象，它可对应为一个抽象体(abstract class)；
  - 第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）；

- 一个体有可能有多个抽象面。抽象体与抽象面是有区别的。

**三个面向区别**

- [面向对象](https://baike.baidu.com/item/面向对象)是指，我们考虑问题时，以对象为单位，考虑它的属性及方法

- [面向过程](https://baike.baidu.com/item/面向过程)是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现

- 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题，更多体现就是对系统整体的架构

## 8.2、使用注解开发

1、注解在接口上实现

```java
    @Select("select * from mybatis.user")
    List<User> getUsers();
```

2、需要在核心配置文件中绑定接口！

```xml
<!--    绑定接口-->
    <mappers>
        <mapper class="com.kuang.dao.UserMapper"/>
    </mappers>
```

3、测试

```java
package com.kuang.dao;

import com.kuang.pojo.User;
import com.kuang.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

public class UserMapperTest {

    @Test
    public void test1() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //底层主要应用反射
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> users = mapper.getUsers();

        for (User user : users) {
            System.out.println(user);
        }
        sqlSession.close();
    }
}
```

本质：反射机制实现

底层：动态代理！

![image-20210804150826993](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210804150826993.png)

**Mybatis详细的执行流程！**

![img](https://img-blog.csdnimg.cn/20201012103425932.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNjQ5NTAz,size_16,color_FFFFFF,t_70#pic_center)

## 8.3、CRUD

我们可以在工具类创建的时候实现自动提交事务！

```java
    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession(true);
    }
```

编写接口，增加注解

```java
package com.kuang.dao;

import com.kuang.pojo.User;
import org.apache.ibatis.annotations.*;

import java.util.List;
import java.util.Map;

public interface UserMapper {

    @Select("select * from mybatis.user")
    List<User> getUsers();

    //方法存在多个参数，所以参数的前面必须加上 @Param("id") 注解
    @Select("select * from mybatis.user where id = #{id} ")
    User getUserById(@Param("id") int id);

    @Insert("insert into mybatis.user(id,name,pwd) values (#{id} ,#{name} ,#{password} )")
    int addUser(User user);

    @Update("update mybatis.user set name = #{name} ,pwd = #{password} where id = #{id} ")
    int updateUser(User user);

    @Delete("delete from mybatis.user where id = #{id} ")
    int deleteUser(@Param("id") int id);
}
```

测试类

```java
package com.kuang.dao;

import com.kuang.pojo.User;
import com.kuang.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

public class UserMapperTest {

    @Test
    public void test1() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //底层主要应用反射
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
//        List<User> users = mapper.getUsers();
//
//        for (User user : users) {
//            System.out.println(user);
//        }

//        User userById = mapper.getUserById(1);
//        System.out.println(userById);

//        mapper.addUser(new User(5,"李四","1231235"));


//        mapper.updateUser(new User(5,"Hello","423231"));

        mapper.deleteUser(5);

        sqlSession.close();
    }
}
```

注意：我们必须要讲接口注册绑定到我们的核心配置文件中！

## 8.4、注意点

**关于@Param()注解**

- 基本类型的参数或者String类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型的话，可以忽略，但是建议大家都加上！
- 我们在SQL中引用的就是我们这里的@Param(“”)中设定的属性名！

# 9、Lombok

```java
Project Lombok is a java library that automatically plugs into your editor and build tools, spicing up your java.
Never write another getter or equals method again, with one annotation your class has a fully featured builder, Automate your logging variables, and much more.
```

- java libray
- plugs
- build tools
- with one annotation yous class

使用步骤：

​    1.在IDEA中安装Lombok插件！

​    2.在项目中导入lombok的jar包

```xml
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.10</version>
            <scope>provided</scope>
        </dependency>
```

​    3.在实体类上加注解即可！ 

```java
@Getter and @Setter
@FieldNameConstants
@ToString
@EqualsAndHashCode
@AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor
@Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
@Data
@Builder
@SuperBuilder
@Singular
@Delegate
@Value
@Accessors
@Wither
@With
@SneakyThrows
@val
@var
experimental @var
@UtilityClass
@ExtensionMethod (Experimental, activate manually in plugin settings)
Lombok config system
Code inspections
Refactoring actions (lombok and delombok)
```

说明：

```java
@Data:无参构造，get、set、toString、Hashcode、equals
@AllArgsConstructor
@NoArgsConstructor
@ToString
@EqualsAndHashCode
```

# 10、多对一处理

多对一：

![image-20210805091846908](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210805091846908.png)

- 多个学生，对应一个老师
- 对于学生这边而言，关联（多个学生关联一个老师）【多对一】
- 对于老师而言，集合，一个老师有很多学生【一对多】

```sql
drop database if exists mybatis;

Create database mybatis default character set utf8;

use mybatis;

create table user(
    id int(20) NOT NULL,
    name varchar(30) DEFAULT NULL,
    pwd varchar(30) DEFAULT NULL,
    primary key(id)
);

insert into user(id,name,pwd) values
(1,'迪丽热巴','123456'),
(2,'古力娜扎','12345'),
(3,'张三','1234523');


create table teacher(
    id int(10) not null,
    name varchar(30) default null,
    primary key(id)
)engine=innodb default charset=utf8;

insert into teacher(id,name) values(1,'秦老师');

drop table student;

create table student(
    id int(10) not null,
    name varchar(30) default null,
    tid int(10) default null,
    primary key(id),
    key fktid(tid),
    constraint fktid foreign key(tid) references teacher(id)
)engine = innodb default charset=utf8;

insert into student(id,name,tid) values (1,'小明',1);
insert into student(id,name,tid) values (2,'小红',1);
insert into student(id,name,tid) values (3,'小张',1);
insert into student(id,name,tid) values (4,'小李',1);
insert into student(id,name,tid) values (5,'小王',1);
```

## 10.1、测试环境搭建

1.导入Lombok

2.新建实体类Teacher，Student

3.建立Mapper接口

4.建立Mapper.xml文件

5.在核心配置文件中绑定注册我们的Mapper接口或者文件！【方式很多，随心选】

6.测试查询是否能够成功！

## 10.2、按照查询嵌套处理

```xml
    <!--
       思路：
            1.查询所以的学生信息
            2.根据查询出来的学生的tid，寻找对应的老师！
    -->
    <select id="getStudent" resultMap="StudentTeacher">
        select *
        from mybatis.student;
    </select>

    <resultMap id="StudentTeacher" type="Student">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <!--复杂的属性，我们需要单独处理
            对象：association
            集合：collection
        -->
        <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
    </resultMap>

    <select id="getTeacher" resultType="Teacher">
        select *
        from mybatis.teacher
        where id = #{id}
    </select>
```

## 10.3、按照结果集查询

```xml
    <!--按照结果集嵌套处理-->
    <select id="getStudent2" resultMap="StudentTeacher2">
        select s.id sid,s.name sname,t.name tname
        from student s,teacher t
        where s.tid = t.id;
    </select>

    <resultMap id="StudentTeacher2" type="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <association property="teacher" javaType="Teacher">
            <result property="name" column="tname"/>
        </association>
    </resultMap>
```

回顾Mysql多对一查询方式：

- 子查询
- 联表查询

# 11、一对多

比如：一个老师拥有多个学生！

对于老师而言，就是一对多的关系！

## 环境搭建

1、环境搭建

实体类

```java
package com.kuang.pojo;

import lombok.Data;

@Data
public class Student {

    private int id;
    private String name;
    private int tid;
}
```

```java
package com.kuang.pojo;

import lombok.Data;

import java.util.List;

@Data
public class Teacher {

    private int id;
    private String name;

    //一个老师拥有多个学生
    private List<Student> students;

}
```

## 按照结果嵌套处理

```xml
    <!--按照结果嵌套查询-->
    <select id="getTeacher" resultMap="TeacherStudent">
        select s.id sid, s.name sname, t.name tname, t.id tid
        from student s,
             teacher t
        where s.tid = t.id
          and t.id = #{tid}
    </select>

    <resultMap id="TeacherStudent" type="Teacher">
        <result property="id" column="tid"/>
        <result property="name" column="tname"/>
        <!--复杂的属性，我们需要单独处理
            对象：association
            集合：collection
            javaType="" 指定的属性类型
            集合中的泛型信息，我们使用ofType获取
            -->
        <collection property="students" ofType="Student">
            <result property="id" column="sid"/>
            <result property="name" column="sname"/>
            <result property="tid" column="tid"/>
        </collection>

    </resultMap>
```

## 按照查询嵌套处理

```xml
<select id="getTeacher2" resultMap="TeacherStudent2">
        select * from teacher where id = #{tid}
    </select>
    <resultMap id="TeacherStudent2" type="Teacher">
        <collection property="students" javaType="ArrayList" ofType="Student" select="getStudentByTeacherId" column="id"/>
    </resultMap>

    <select id="getStudentByTeacherId" resultType="Student">
        select * from student where tid = #{tid}
    </select>
```

## 小结

1、关联-association【多对一】

2、集合-collection【一对多】

3、JavaType & ofType

​    1、JavaType 用来指定实体类中属性的类型

​    2、ofType 用来指定映射到List或者集合中的pojo类型，泛型中的约束类型！

注意点：

- 保证SQL的可读性，尽量保证通俗易懂
- 注意一对多和多对一中，属性名和字段的问题！
- 如果问题不好排查错误，可以使用日志，尽量使用Log4j！

==**面试高频**：==

- **Mysql引擎**
- **InnoDB底层原理**
- **索引**
- **索引优化**

# 12、动态SQL

==**什么是动态SQL：动态SQL就是指根据不同的条件生成不同的SQL语句**==

利用动态 SQL，可以彻底摆脱这种痛苦。

```xml
如果你之前用过 JSTL 或任何基于类 XML 语言的文本处理器，你对动态 SQL 元素可能会感觉似曾相识。在 MyBatis 之前的版本中，需要花时间了解大量的元素。借助功能强大的基于 OGNL 的表达式，MyBatis 3 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少。
if
choose (when, otherwise)
trim (where, set)
foreach
```

## 搭建环境

```sql
CREATE TABLE `blog` (
`id` varchar(50) NOT NULL COMMENT '博客id',
`title` varchar(100) NOT NULL COMMENT '博客标题',
`author` varchar(30) NOT NULL COMMENT '博客作者',
`create_time` datetime NOT NULL COMMENT '创建时间',
`views` int(30) NOT NULL COMMENT '浏览量'
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

创建一个基础工程

1.导包

2.编写配置文件

3.编写实体类

```java
package com.kuang.pojo;

import lombok.Data;

import java.util.Date;

@Data
public class Blog {
    private int id;
    private String title;
    private String author;
    private Date createTime;
    private int views;
}
```

4.编写实体类对应Mapper接口和Mapper.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.dao.BlogMapper">
    <insert id="addBlog" parameterType="Blog">
        insert into blog(id, title, author, create_time, views)
        VALUES (#{id}, #{title}, #{author}, #{createTime}, #{views})
    </insert>
</mapper>
```

## IF

1、接口类

```java
  //查询博客
    List<Blog> queryBlogIF(Map map);
```

2、配置文件

```xml
    <select id="queryBlogIF" parameterType="map" resultType="Blog">
        select *
        from blog
        where 1 = 1
        <if test="title != null">
            and title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
    </select>
```

3、测试类

```java
 @Test
  public void queryBlogIF() {
    SqlSession sqlSession = MybatisUtils.getSqlSession();

    BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);

    HashMap map = new HashMap<>();
    //map.put("title","Java如此简单");
    map.put("author","狂神说");
    List<Blog> blogs = mapper.queryBlogIF(map);

    for (Blog blog : blogs) {
      System.out.println(blog);
    }

    sqlSession.close();
  }
```

## choose（when，otherwise）

```xml
    <select id="queryBlogChoose" parameterType="map" resultType="Blog">
        select *
        from blog
        <where>
            <choose>
                <when test="title != null">
                    title = #{title}
                </when>
                <when test="author != author">
                    and author = #{author}
                </when>
                <otherwise>
                    and views = #{views}
                </otherwise>
            </choose>
        </where>
    </select>
```

## trim(where，set)

```xml
    <select id="queryBlogIF" parameterType="map" resultType="Blog">
        select *
        from blog
        <where>
        <if test="title != null">
            title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
        </where>
    </select>
```

```xml
    <update id="updateBlog" parameterType="map">
        update blog
        <set>
            <if test="title != null">
                title = #{title},
            </if>
            <if test="author != null">
                author = #{author}
            </if>
        </set>
        where id = #{id}
    </update>
```

==**所谓的动态SQL，本质还是SQL语句，只是我们可以在SQL层面，去执行一个逻辑代码**==

## SQL片段

有的时候，我们可能会将一些公共的部分抽取出来，方便复用！

​    1.使用SQL标签抽取公共的部分

```xml
 <sql id="if-title-author">
        <if test="title != null">
            title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
    </sql>
```

​    2.在需要使用的地方使用include标签引用即可

```xml
   <select id="queryBlogIF" parameterType="map" resultType="Blog">
        select *
        from blog
        <where>
            <include refid="if-title-author"></include>
        </where>
    </select>
```

注意事项：

- 最好基于单表来定义SQL片段！
- 不要存在where标签！

## Foreach

```sql
select * from user where 1=1 and 

<foreach item="id" index="index" collection="ids"
      open="(" separator="or" close=")">
        #{id}
  </foreach>


(id=1 or id=2 or id=3)
```

![image-20210806125041975](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210806125041975.png)

1、配置文件

```xml
    <!--
    select * from blog where 1=1 and (id = 1 or id = 2 or id = 3)

    我们现在传递一个万能的map，这map中可以存在一个集合！
    -->
    <select id="queryBlogForeach" parameterType="map" resultType="Blog">
        select * from blog
        <where>
            <foreach collection="ids" item="id" open="and (" close=")" separator="or">
                id = #{id}
            </foreach>
        </where>
    </select>
```

2、接口类

```java
    //查询第1-2-3号记录的博客
    List<Blog> queryBlogForeach(Map map);
```

3、测试类

```java
  @Test
  public void queryBlogForEach() {
    SqlSession sqlSession = MybatisUtils.getSqlSession();

    BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);

    HashMap map = new HashMap();

    ArrayList<Integer> ids = new ArrayList<>();
    ids.add(1);
    ids.add(2);

    map.put("ids",ids);

    List<Blog> blogs = mapper.queryBlogForeach(map);

    for (Blog blog : blogs) {
      System.out.println(blog);
    }

    sqlSession.close();
  }
```

**==动态SQL就是在拼接SQL语句，我们只要保证SQL的正确性，按照SQL的格式，去排列组合就可以了==**

建议：

- 先在Mysql中写出完整的SQL，再对应的去修改成为动态SQL实现通用即可！

# 13、缓存

## 13.1、简介

```
查询 ； 连接数据库 ， 耗资源！
    一次查询的结果，给他暂存在一个可以直接取到的地方！-->内存 ：缓存

我们再次查询相同的数据时，直接走缓存，就不用走数据库了
```

1、什么是缓存 [ Cache ]？

- 存在内存中的临时数据。

- 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。

2、为什么使用缓存？

- 减少和数据库的交互次数，减少系统开销，提高系统效率。
  3、什么样的数据能使用缓存？

- 经常查询并且不经常改变的数据。【可以使用缓存】

## 13.2、Mybatis缓存

- MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。

- MyBatis系统中默认定义了两级缓存：一级缓存和二级缓存
  
  - 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）
  - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
  - 为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存

## 13.3、一级缓存

**一级缓存也叫本地缓存：**

- 与数据库同一次会话期间查询到的数据会放在本地缓存中。
- 以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库。

测试步骤：

​    1.开启日志

​    2.测试在一个Session中查询两次相同的记录

​    3.查看日志输出

1、接口类

```java
public interface UserMapper {

    //查询用户
    User queryUserById(@Param("id") int id);
}
```

2、配置UserMapper.xml文件

```xml
<mapper namespace="com.kuang.dao.UserMapper">

    <select id="queryUserById" resultType="User">
        select * from user where id = #{id}
    </select>
</mapper>
```

3、测试类

```java
public class MyTest {

  @Test
  public void test() {
    SqlSession sqlSession = MybatisUtils.getSqlSession();

    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    User user = mapper.queryUserById(1);
    System.out.println(user);
    User user2 = mapper.queryUserById(1);
    System.out.println(user);

    System.out.println(user == user2);

    sqlSession.close();
  }
}
```

结果分析：

![img](https://img-blog.csdnimg.cn/20201013085757598.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNjQ5NTAz,size_16,color_FFFFFF,t_70#pic_center)

缓存失效的情况：

​    1.查询不同的东西

​    2.增删改操作，可能会改变原来的数据，所以必定会刷新缓存！

​    3.查询不同的Mapper.xml

​    4.手动清除缓存

**一级缓存是SqlSession级别的缓存，是一直开启的，我们关闭不了它；**

**一级缓存失效情况：**没有使用到当前的一级缓存，效果就是，还需要再向数据库中发起一次查询请求！

![image-20210806164935753](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210806164935753.png)

小结：一级缓存默认是开启的，只在一次SqlSession中有效，也就是拿到连接到关闭连接这个区间段！

一级缓存相当于一个Map。

## 13.4、二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存

- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存；

- 工作机制
  
  - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
  
  - 如果当前会话关闭了，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中；
  
  - 新的会话查询信息，就可以从二级缓存中获取内容；
  
  - 不同的mapper查出的数据会放在自己对应的缓存（map）中；

步骤：

​    1、开启全局缓存

```xml
<!--显式的开始全局缓存-->
        <setting name="cacheEnabled" value="true"/>
```

​    2、在要使用二级缓存的Mapper中开启

```xml
    <!--在当前Mapper.xml使用二级缓存-->
    <cache/>
```

也可以自定义参数

```xml
    <!--在当前Mapper.xml使用二级缓存-->
    <cache eviction="FIFO"
           flushInterval="60000"
           size="512"
           readOnly="true"/>
```

这个更高级的配置创建了一个 FIFO 缓存，每隔 60 秒刷新，最多可以存储结果对象或列表的 512 个引用，而且返回的对象被认为是只读的，因此对它们进行修改可能会在不同线程中的调用者产生冲突。

3、测试

​    1.问题：我们需要将实体类序列化！否则就会报错！

小结：

- 只要开启了二级缓存，在同一个Mapper下就有效
- 所以的数据都会先放在一级缓存中；
- 只有当会话提交，或者关闭的时候，才会提交到二级缓存中！

## 13.5、缓存原理

![img](https://img-blog.csdnimg.cn/20201013095849984.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNjQ5NTAz,size_16,color_FFFFFF,t_70#pic_center)

![img](https://img-blog.csdnimg.cn/20201013093137919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNjQ5NTAz,size_16,color_FFFFFF,t_70#pic_center)

## 13.6、自定义缓存-ehcache

```xml
Ehcache是一种广泛使用的开源Java分布式缓存。主要面向通用缓存,
```

要在程序中使用ehcache，先要导包！

```xml
    <dependency>
            <groupId>org.mybatis.caches</groupId>
            <artifactId>mybatis-ehcache</artifactId>
            <version>1.1.0</version>
        </dependency>
```

在Mapper中指定使用我们的ehcache缓存实现！

```java
 <!--在当前Mapper.xml使用二级缓存-->
    <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```

ehcache.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <!--
       diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。参数解释如下：
       user.home – 用户主目录
       user.dir – 用户当前工作目录
       java.io.tmpdir – 默认临时文件路径
     -->
    <diskStore path="./tmpdir/Tmp_EhCache"/>

    <defaultCache
            eternal="false"
            maxElementsInMemory="10000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="259200"
            memoryStoreEvictionPolicy="LRU"/>

    <cache
            name="cloud_user"
            eternal="false"
            maxElementsInMemory="5000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="1800"
            memoryStoreEvictionPolicy="LRU"/>
    <!--
       defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
     -->
    <!--
      name:缓存名称。
      maxElementsInMemory:缓存最大数目
      maxElementsOnDisk：硬盘最大缓存个数。
      eternal:对象是否永久有效，一但设置了，timeout将不起作用。
      overflowToDisk:是否保存到磁盘，当系统宕机时
      timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
      timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
      diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
      diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
      diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
      memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
      clearOnFlush：内存数量最大时是否清除。
      memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
      FIFO，first in first out，这个是大家最熟的，先进先出。
      LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
      LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
   -->
</ehcache>
```

Redis数据库来做缓存！K-V
