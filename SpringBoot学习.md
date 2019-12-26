## Springboot学习

#### 一、mybatis

* 使用mybatis时，需在pom.xml中添加如下代码：

  ~~~xml
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.4.6</version>
  </dependency>
  ~~~

  

* 基本配置：

  1. mybatis-config.xml（若没有此文件，则自行创建，一般位于项目的src/main/resources文件下）配置文件

  ~~~xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                  <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql://localhost:3306/studentadmin"/>
                  <property name="username" value="root"/>
                  <property name="password" value="123456"/>
              </dataSource>
          </environment>
      </environments>
      <!--映射器-->
      <mappers>
          <!--每一个定义的SQL映射文件的位置-->
          <mapper resource="mapper/userMapper.xml"/>
      </mappers>
  </configuration>
  ~~~

* XML映射文件：

  1. 基本格式：

     ~~~xml
     <?xml version="1.0" encoding="UTF-8"?>
     <!DOCTYPE mapper
             PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
     <mapper namespace="com.aplin.demo.UserMapper">
         <!-- 查询所有用户信息 -->
         <select id="selectAllUser"  resultType="com.aplin.demo.User">
             select * from user
         </select>
         <!-- 添加一个用户，#{uname}为 com.aplin.demo.MyUser 的属性值 -->
         <insert id="addUser" parameterType="com.aplin.demo.User">
             insert into user (username,password)
             values(#{username},#{password})
         </insert>
         <!--修改一个用户 -->
         <update id="updateUser" parameterType="com.aplin.demo.User">
             update user set username =
             #{username},password = #{password} where username = #{username}
         </update>
         <!-- 删除一个用户 -->
         <delete id="deleteUser" parameterType="String">
             delete from user where username
             = #{username}
         </delete>
     </mapper>
     ~~~

     **注：**

     ​	namespace：接口的位置（命名空间）

     ​	com.aplin.demo.UserMapper：接口的路径

     ​	语句中的id：是这个接口的方法名（在命名空间中唯一的标识符，可以被用来引用这条语句。）
     ​	resultType：方法的返回类型（从这条语句中返回的期望类型的类的完全限定名或别名）

     ​	parameterType：方法的参数类型（将会传入这条语句的参数类的完全限定名或别名）

### 二、spring中jsp使用

 1. 在pom.xml添加依赖

    ~~~xml
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
    </dependency>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
    </dependency>
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-jasper</artifactId>
    </dependency>
    ~~~

    **注**不能使用thymeleaf，即pom.xml不能包含以下代码（原因目前不清楚）

    ~~~xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    ~~~

 2. 在application.properties中添加如下内容，用于视图定位jsp的位置

    ~~~
    spring.mvc.view.prefix=/WEB-INF/jsp/
    spring.mvc.view.suffix=.jsp
    ~~~

3. Controller

   ~~~java
   //不能使用@RestController
   @Controller
   public class FirstController {
       @RequestMapping("/one")
       public String text1(){
           return "hello";
       }
   }
   ~~~

   **注：**

   1. 使用 **@Controller** 注解，在对应的方法上，**视图解析器可以解析return 的jsp,html页面**，并且跳转到相应页面。若返回json等内容到页面，则需要加@ResponseBody注解
   1. **@RestController注解**，相当于 **@Controller+@ResponseBody两个注解的结合**，返回json数据不需要在方法前面加@ResponseBody注解了，但使用@RestController这个注解，就不能返回jsp,html页面，视图解析器无法解析jsp,html页面

3. 在main目录下，新建-> webapp/WEB-INF/jsp 目录，并新建hello.jsp

5. 最后在网页中使用localhost:8080/one，可以对hello.jsp进行访问

   

