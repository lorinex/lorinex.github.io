---
categories:
  - 编程语言
  - Java
---
# Spring Boot

## 用户登录系统原理

对于用户登录，我们一定都听说过`cookie`这个词。其实，`cookie`是网络编程中使用最广泛的技术之一，它用于储存用户的登录信息。它储存在用户本地，每次随着网络请求发送给服务端，服务端就用这个判断用户是否登录。可以看看这个图：

![用户登录网络请求示意图-Cookie机制.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/37b0876bf95c4db0bc01bc3138aa5068~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

可见，用户未登录之前，http请求是不带cookie的，登录后，客户端会将登录信息放在请求中给服务端，服务端进行验证，登录成功后，服务端会把用户信息放在cookie里面，随着响应返回给客户端，客户端就会储存这个cookie。下次再访问网站，cookie会和客户端的请求一起发给服务端，服务端验证信息正确，判断这个用户是登录状态。

cookie里面也是以`key-value`形式储存数据，并且cookie有它自己的属性例如生命周期、生效域名等等。

**但是实际上，cookie里面由于存放着用户信息例如用户名密码等等，很容易存在安全隐患，cookie可以被拦截甚至伪造。**

因此现在网站登录都使用`session`机制，它和`cookie`机制最大的区别就是**用户信息不再放在客户端而是服务端**，交互过程如图：

![用户登录网络请求示意图-Session机制.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9948ebf6db7941f6b7efbb88ba3e8da0~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

可见`session`机制就是**以cookie作为载体，cookie里面只储存一个session id，与服务端通信。每个客户端每一次登录请求都会和服务端会生成唯一的session id，相当于客户端每次只要告诉服务端session id，服务端就可以找到相对应的客户端数据。**

至于session id是什么，其实也很好理解。session id用于标识某个电脑和服务端这一次登录的会话，也就是说在服务端看来，一个session id对应着一台电脑，并且它是唯一的。

首先登录的时候，服务端验证用户名密码正确，就生成一个唯一的session id，并把这个用户信息存在服务端里面，这个信息就对应着这个session id。与此同时这个session id就放在cookie里面发给客户端保存。

下次客户端访问服务端，就带着这个cookie，服务端利用里面的session id在服务端找到对应的用户信息即可进行验证。

因此下面示例我们都使用`session`机制进行。使用Spring Boot可以很轻松的实现`session`读写。

## 配置

1. pom.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>3.0.4</version>
      <relativePath/>
   </parent>
   <groupId>com.example</groupId>
   <artifactId>user-login</artifactId>
   <version>1.0.0</version>
   <name>UserLogin</name>
   <description>UserLogin</description>

   <properties>
      <java.version>17</java.version>
      <maven.compiler.source>${java.version}</maven.compiler.source>
      <maven.compiler.target>${java.version}</maven.compiler.target>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
   </properties>

   <dependencies>
      <!-- Jackson - Json注解 -->
      <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-annotations</artifactId>
         <version>2.12.4</version>
      </dependency>

      <!-- codec - 加密 -->
      <dependency>
         <groupId>commons-codec</groupId>
         <artifactId>commons-codec</artifactId>
      </dependency>

      <!-- commons-lang3 - 实用工具 -->
      <dependency>
         <groupId>org.apache.commons</groupId>
         <artifactId>commons-lang3</artifactId>
      </dependency>

      <!-- Spring Session -->
      <dependency>
         <groupId>org.springframework.session</groupId>
         <artifactId>spring-session-core</artifactId>
      </dependency>

      <!-- Spring Validation - 校验工具 -->
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-validation</artifactId>
      </dependency>

      <!-- Spring Web -->
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-web</artifactId>
      </dependency>

      <!-- MyBatis - 数据库框架 -->
      <dependency>
         <groupId>org.mybatis.spring.boot</groupId>
         <artifactId>mybatis-spring-boot-starter</artifactId>
         <version>3.0.1</version>
      </dependency>

      <!-- MySQL连接驱动 -->
      <dependency>
         <groupId>com.mysql</groupId>
         <artifactId>mysql-connector-j</artifactId>
         <version>8.0.32</version>
      </dependency>

      <!-- Lombok -->
      <dependency>
         <groupId>org.projectlombok</groupId>
         <artifactId>lombok</artifactId>
         <optional>true</optional>
      </dependency>

      <!-- Spring Test -->
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-test</artifactId>
         <scope>test</scope>
      </dependency>
   </dependencies>

   <build>
      <plugins>
         <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
               <excludes>
                  <exclude>
                     <groupId>org.projectlombok</groupId>
                     <artifactId>lombok</artifactId>
                  </exclude>
               </excludes>
            </configuration>
         </plugin>
      </plugins>
   </build>
</project>
```



2. application.properties文件

```properties
# 网站端口配置
server.port=8802

# JSON配置，设定不对未知字段和空值字段进行序列化节省流量
spring.jackson.deserialization.fail-on-unknown-properties=false
spring.jackson.default-property-inclusion=non_null

# MySQL数据库地址和账户配置（根据自己实际情况进行填写）
spring.datasource.url=jdbc:mysql://localhost:3306/miyakogame?serverTimezone=GMT%2B8
spring.datasource.username=swsk33
spring.datasource.password=dev-2333
```

对于Mybatis的Mapper XML文件，默认位于`src/main/resources/com/{xxx}/{xxx}/dao/`之下，即和你的dao包位置对应。例如我们项目的Mapper类一般会放在`com.example.userlogin.dao`下，那么Spring Boot就默认去这个地方扫描Mapper XML：`src/main/resources/com/example/userlogin/dao`，目录需手动创建。当然我们也可以进行指定：

```properties
mybatis.mapper-locations=file:Resources/mybatisMapper/*.xml
复制代码
```

这里我们的项目就不配置MyBatis的XML位置了，就使用默认位置。配置全部根据自己实际情况进行填写。

## 封装一个请求结果类

为了方便起见，我们通常封装一个结果类，里面主要是请求结果代码、消息、是否操作成功和数据体，可以根据自己需要进行修改。

新建软件包`model`，并在其中新建`Result<T>`内容如下：

```java
package com.example.userlogin.model;

import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

import java.io.Serializable;

/**
 * 请求结果类
 *
 * @param <T> 数据体类型
 */
@Setter
@Getter
@NoArgsConstructor
public class Result<T> implements Serializable {

	/**
	 * 消息
	 */
	private String message;

	/**
	 * 是否操作成功
	 */
	private boolean success;

	/**
	 * 返回的数据主体（返回的内容）
	 */
	private T data;

	/**
	 * 设定结果为成功
	 *
	 * @param msg 消息
	 */
	public void setResultSuccess(String msg) {
		this.message = msg;
		this.success = true;
		this.data = null;
	}

	/**
	 * 设定结果为成功
	 *
	 * @param msg  消息
	 * @param data 数据体
	 */
	public void setResultSuccess(String msg, T data) {
		this.message = msg;
		this.success = true;
		this.data = data;
	}

	/**
	 * 设定结果为失败
	 *
	 * @param msg 消息
	 */
	public void setResultFailed(String msg) {
		this.message = msg;
		this.success = false;
		this.data = null;
	}

}
```

一般都会返回给前端这个结果对象，更加便捷的传递是否成功以及操作消息等等。

注意前后端传递交互的数据都需要**实现序列化接口**并且要**有无参构造器**，这里使用了Lombok的注解，下面也一样。

## 建立用户类，并初始化数据库表

创建软件包`dataobject`，并在其中建立用户类`User`，实际根据业务需要不同，用户类可能有很多属性，这里我们只建立最简单的如下：

```java
package com.example.userlogin.dataobject;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;
import jakarta.validation.constraints.NotEmpty;
import jakarta.validation.constraints.Size;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;


import java.io.Serializable;

/**
 * 用户类
 */
@Setter
@Getter
@NoArgsConstructor
@JsonIgnoreProperties(value = {"password"}, allowSetters = true)
public class User implements Serializable {

   /**
    * 用户id
    */
   private Integer id;

   /**
    * 用户名
    */
   @NotEmpty(message = "用户名不能为空！")
   private String username;

   /**
    * 密码
    */
   @NotEmpty(message = "密码不能为空！")
   @Size(min = 8, message = "密码长度不能小于8！")
   private String password;

}
```

我们设定了用户的最基本属性，并对其设定了**校验规则**（不熟悉Spring Validation可以看看[这篇文章](https://juejin.cn/post/6991866086836666381)），密码为敏感信息，因此我们**使用Jackson注解设定密码字段为不允许序列化**（不会传给前端）。

创建了用户类，我们数据库表也需要对应起来，初始化一个用户表，我这里sql如下：

```sql
drop table if exists `user`;
create table `user`
(
	`id`	   int unsigned auto_increment,
	`username` varchar(16) not null,
	`password` varchar(32) not null,
	primary key (`id`)
) engine = InnoDB
  default charset = utf8mb4;
```

连接MySQL并`use`相应数据库，将这个sql文件使用`source`命令执行即可。

每个对象都有主键id，且一般设为自增无符号整数，这样有最高的数据库读写效率。密码一般使用MD5加密储存，因此固定32位长度。一般每个数据库表都有创建时间gmt_created和修改时间gmt_modified字段，这里简单起见省略。

## 创建数据服务层-DAO并配置Mapper XML

`DAO`层主要是Java的对于数据库操作的接口和实现类。MyBatis的强大之处，就是只需定义接口，就可以实现操作数据库。

创建软件包`dao`，新建接口`UserDAO`：

```java
package com.example.userlogin.dao;

import com.example.userlogin.dataobject.User;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserDAO {

   /**
    * 新增用户
    *
    * @param user 用户对象
    * @return 新增成功记录条数
    */
   int add(User user);

   /**
    * 修改用户信息
    *
    * @param user 用户对象
    * @return 修改成功记录条数
    */
   int update(User user);

   /**
    * 根据id获取用户
    *
    * @param id 用户id
    * @return 用户对象
    */
   User getById(Integer id);

   /**
    * 根据用户名获取用户
    *
    * @param username 用户名
    * @return 用户对象
    */
   User getByUsername(String username);

}
```

根据实际需要定义数据库增删改查方法，这里就定义这些。注意这里接口上面要打上`@Mapper`注解表示它是个数据持久层接口。且一般来说，**增删改**方法的返回值都是int，表示操作成功记录条数，**查**方法一般是返回相应对象或者对象的`List`。

然后编写Mapper XML文件，我们在项目文件夹下的`src/main/resources`目录下创建多级目录：`com/example/userlogin/dao`，在这个目录下存放XML文件。

创建`UserDAO.xml`内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.userlogin.dao.UserDAO">
	<resultMap id="userResultMap" type="com.example.userlogin.dataobject.User">
		<id column="id" property="id"/>
		<result column="username" property="username"/>
		<result column="password" property="password"/>
	</resultMap>

	<insert id="add" parameterType="com.example.userlogin.dataobject.User">
		insert into `user` (username, password)
		values (#{username}, #{password})
	</insert>

	<update id="update" parameterType="com.example.userlogin.dataobject.User">
		update `user`
		set password=#{password}
		where id = #{id}
	</update>

	<select id="getById" resultMap="userResultMap">
		select *
		from `user`
		where id = #{id}
	</select>

	<select id="getByUsername" resultMap="userResultMap">
		select *
		from `user`
		where username = #{username}
	</select>
</mapper>
```

这样，数据库操作层就完成了！

> 一般约定某一个对象（xxx）的数据库操作层接口一般命名为xxxDAO（有的企业也命名为xxxMapper），一个xxxDAO接口对应一个xxxDAO.xml文件。

## 创建用户服务层

现在就要进行正式的服务层逻辑了，完成我们的主要功能：用户注册、登录、信息修改。

其实，用户注册就是前端发送用户注册信息（封装为`User`对象），后端检验然后往数据库**增加一条用户记录**的过程；登录也是前端发送用户登录信息，同样封装为`User`对象，后端根据这个对象的`username`字段**从数据库取出用户**、进行比对最后设定session的过程；修改用户也是前端发送修改后的用户信息的`User`对象，后端进行比对，然后**修改数据库相应记录**的过程。

先新建包`service`，在其中添加用户服务接口`UserService`：

```java
package com.example.userlogin.service;

import com.example.userlogin.dataobject.User;
import com.example.userlogin.model.Result;
import jakarta.servlet.http.HttpSession;
import org.springframework.stereotype.Service;

@Service
public interface UserService {

   /**
    * 用户注册
    *
    * @param user 用户对象
    * @return 注册结果
    */
   Result<User> register(User user);

   /**
    * 用户登录
    *
    * @param user 用户对象
    * @return 登录结果
    */
   Result<User> login(User user);

   /**
    * 修改用户信息
    *
    * @param user 用户对象
    * @return 修改结果
    */
   Result<User> update(User user) throws Exception;

   /**
    * 判断用户是否登录（实际上就是从session取出用户id去数据库查询并比对）
    *
    * @param session 传入请求session
    * @return 返回结果，若用户已登录则返回用户信息
    */
   Result<User> isLogin(HttpSession session);

}
```

然后再在包`service`下建立包`impl`，然后在里面新建用户服务实现类`UserServiceImpl`：

用来存放接口的实现类，impl的全称为implement

```java
package com.example.userlogin.service.impl;

import com.example.userlogin.api.UserAPI;
import com.example.userlogin.dao.UserDAO;
import com.example.userlogin.dataobject.User;
import com.example.userlogin.model.Result;
import com.example.userlogin.service.UserService;
import com.example.userlogin.util.ClassExamine;
import jakarta.servlet.http.HttpSession;
import org.apache.commons.codec.digest.DigestUtils;
import org.apache.commons.lang3.StringUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;


@Component
public class UserServiceImpl implements UserService {

   @Autowired
   private UserDAO userDAO;

   @Override
   public Result<User> register(User user) {
      Result<User> result = new Result<>();
      // 先去数据库找用户名是否存在
      User getUser = userDAO.getByUsername(user.getUsername());
      if (getUser != null) {
         result.setResultFailed("该用户名已存在！");
         return result;
      }
      // 加密储存用户的密码
      user.setPassword(DigestUtils.md5Hex(user.getPassword()));
      // 存入数据库
      userDAO.add(user);
      // 返回成功消息
      result.setResultSuccess("注册用户成功！", user);
      return result;
   }

   @Override
   public Result<User> login(User user) {
      Result<User> result = new Result<>();
      // 去数据库查找用户
      User getUser = userDAO.getByUsername(user.getUsername());
      if (getUser == null) {
         result.setResultFailed("用户不存在！");
         return result;
      }
      // 比对密码（数据库取出用户的密码是加密的，因此要把前端传来的用户密码加密再比对）
      if (!getUser.getPassword().equals(DigestUtils.md5Hex(user.getPassword()))) {
         result.setResultFailed("用户名或者密码错误！");
         return result;
      }
      // 设定登录成功消息
      result.setResultSuccess("登录成功！", getUser);
      return result;
   }

   @Override
   public Result<User> update(User user) throws Exception {
      Result<User> result = new Result<>();
      // 去数据库查找用户
      User getUser = userDAO.getById(user.getId());
      if (getUser == null) {
         result.setResultFailed("用户不存在！");
         return result;
      }
      // 检测传来的对象里面字段值是否为空，若是就用数据库里面的对象相应字段值补上
      if (!StringUtils.isEmpty(user.getPassword())) {
         // 加密储存
         user.setPassword(DigestUtils.md5Hex(user.getPassword()));
      }
      // 对象互补
      ClassExamine.objectOverlap(user, getUser);
      // 存入数据库
      userDAO.update(user);
      result.setResultSuccess("修改用户成功！", user);
      return result;
   }

   @Override
   public Result<User> isLogin(HttpSession session) {
      Result<User> result = new Result<>();
      // 从session中取出用户信息
      User sessionUser = (User) session.getAttribute(UserAPI.SESSION_NAME);
      // 若session中没有用户信息这说明用户未登录
      if (sessionUser == null) {
         result.setResultFailed("用户未登录！");
         return result;
      }
      // 登录了则去数据库取出信息进行比对
      User getUser = userDAO.getById(sessionUser.getId());
      // 如果session用户找不到对应的数据库中的用户或者找出的用户密码和session中用户不一致则说明session中用户信息无效
      if (getUser == null || !getUser.getPassword().equals(sessionUser.getPassword())) {
         result.setResultFailed("用户信息无效！");
         return result;
      }
      result.setResultSuccess("用户已登录！", getUser);
      return result;
   }

}
```

需要注意的是服务接口需要有`@Service`注解，接口实现类要有`@Component`注解，还自动注入了DAO的实例进行数据库操作。

这里着重强调一下`update`操作。可以看见，在DAO层的`update`的SQL语句中并没有判断传入的用户对象的字段是否为空或者是否和原来一致，而是直接将传入的用户对象覆盖至数据库中相应的用户记录上了。因此，一般情况下，我们在`Service`层进行判断。一般来说，`Service`层`update`方法会先从数据库取出原始用户信息（上述名为`getUser`的对象），然后和传入的用户信息对象（上述名为`user`的对象）进行字段对比。**如果传入的`user`中某个字段为空，说明这个字段的信息是不需要修改的，这时使用原始用户对象的相应字段值填上去，如果不为空，则保留其值，最后再传入DAO层修改数据库**。也因此，前端在发起修改请求时，也会将不用修改的字段留空。这就是我们平时进行对象更新的逻辑。

不过当一个用户的字段多了，我们是不是要写很多个`if`来逐一判断呢？当然是不行的。所以这里我封装了一个方法，利用反射，检测被补全的对象（传入对象）和完整对象（从数据库取出的对象）的字段，如果被补全对象某个字段为空，这说明这个字段值是不用修改的，用完整对象对应的字段值补全，不为空说明被补全对象（传入对象）中这个字段值是新的值，是要修改的，因此保持其不变。

当然，密码是特殊的字段，因为需要加密储存，因此需要单独判断前端是否传入了新的密码，如果是就加密并赋给相应字段。在上述代码中我也进行了判断。

这里新建`util`包，新建`ClassExamine`类封装一个补全对象的方法，如下：

```java
package com.example.userlogin.util;

import org.apache.commons.lang3.StringUtils;

import java.lang.reflect.Field;

/**
 * 类检测实用类
 */
public class ClassExamine {

	/**
	 * 对象字段互补。传入一个同类型被补充对象和完整对象，如果被补充对象中有字段为null或者字符串为空，
	 * 就用完整对象对应的字段值补上去；如果被补充对象中某字段不为空则保留它自己的值。
	 *
	 * @param origin       被补充对象
	 * @param intactObject 完整对象
	 * @param <T>          传入对象类型
	 */
	public static <T> void objectOverlap(T origin, T intactObject) throws Exception {
		Field[] fields = origin.getClass().getDeclaredFields();
		for (Field field : fields) {
			field.setAccessible(true);
			if (field.getType() == String.class) {
				if (StringUtils.isEmpty((String) field.get(origin))) {
					field.set(origin, field.get(intactObject));
				}
			} else {
				if (field.get(origin) == null) {
					field.set(origin, field.get(intactObject));
				}
			}
		}
	}

}
```

以及上述判断用户登录的方法，其中的session是从Controller类中传来的，（因为Controller类可以获取请求中的session），这里先不要纠结session的问题，我们往下看。

## 创建用户登录API

服务层写完了，现在就是前后端交互的桥梁需要打通了-编写API，这样前端才能发送请求调用我们后端的服务。

我们的API要实现用户**登录**、**注册**、**判断用户是否登录**、**修改用户信息**、**用户登出**这几个功能。

新建包`api`，然后在里面新建类`UserAPI`：

```java
package com.example.userlogin.api;

import com.example.userlogin.dataobject.User;
import com.example.userlogin.model.Result;
import com.example.userlogin.service.UserService;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpSession;
import jakarta.validation.Valid;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.*;


@RestController
public class UserAPI {

   /**
    * session的字段名
    */
   public static final String SESSION_NAME = "userInfo";

   @Autowired
   private UserService userService;

   /**
    * 用户注册
    *
    * @param user    传入注册用户信息
    * @param errors  Validation的校验错误存放对象
    * @param request 请求对象，用于操作session
    * @return 注册结果
    */
   @PostMapping("/register")
   public Result<User> register(@RequestBody @Valid User user, BindingResult errors, HttpServletRequest request) {
      Result<User> result;
      // 如果校验有错，返回注册失败以及错误信息
      if (errors.hasErrors()) {
         result = new Result<>();
         result.setResultFailed(errors.getFieldError().getDefaultMessage());
         return result;
      }
      // 调用注册服务
      result = userService.register(user);
      return result;
   }

   /**
    * 用户登录
    *
    * @param user    传入登录用户信息
    * @param errors  Validation的校验错误存放对象
    * @param request 请求对象，用于操作session
    * @return 登录结果
    */
   @PostMapping("/login")
   public Result<User> login(@RequestBody @Valid User user, BindingResult errors, HttpServletRequest request) {
      Result<User> result;
      // 如果校验有错，返回登录失败以及错误信息
      if (errors.hasErrors()) {
         result = new Result<>();
         result.setResultFailed(errors.getFieldError().getDefaultMessage());
         return result;
      }
      // 调用登录服务
      result = userService.login(user);
      // 如果登录成功，则设定session
      if (result.isSuccess()) {
         request.getSession().setAttribute(SESSION_NAME, result.getData());
      }
      return result;
   }

   /**
    * 判断用户是否登录
    *
    * @param request 请求对象，从中获取session里面的用户信息以判断用户是否登录
    * @return 结果对象，已经登录则结果为成功，且数据体为用户信息；否则结果为失败，数据体为空
    */
   @GetMapping("/is-login")
   public Result<User> isLogin(HttpServletRequest request) {
      // 传入session到用户服务层
      return userService.isLogin(request.getSession());
   }

   /**
    * 用户信息修改
    *
    * @param user    修改后用户信息对象
    * @param request 请求对象，用于操作session
    * @return 修改结果
    */
   @PutMapping("/update")
   public Result<User> update(@RequestBody User user, HttpServletRequest request) throws Exception {
      Result<User> result = new Result<>();
      HttpSession session = request.getSession();
      // 检查session中的用户（即当前登录用户）是否和当前被修改用户一致
      User sessionUser = (User) session.getAttribute(SESSION_NAME);
      if (sessionUser.getId() != user.getId().intValue()) {
         result.setResultFailed("当前登录用户和被修改用户不一致，终止！");
         return result;
      }
      result = userService.update(user);
      // 修改成功则刷新session信息
      if (result.isSuccess()) {
         session.setAttribute(SESSION_NAME, result.getData());
      }
      return result;
   }

   /**
    * 用户登出
    *
    * @param request 请求，用于操作session
    * @return 结果对象
    */
   @GetMapping("/logout")
   public Result<Void> logout(HttpServletRequest request) {
      Result<Void> result = new Result<>();
      // 用户登出很简单，就是把session里面的用户信息设为null即可
      request.getSession().setAttribute(SESSION_NAME, null);
      result.setResultSuccess("用户退出登录成功！");
      return result;
   }

}
```

因为是API，所以使用`@RestController`标注类，可见注册是通过前端发送POST请求后端接收，修改用的则是PUT请求，且在各个方法中加入参数`HttpServletRequest request`，**这个名为`request`的对象就代表客户端这次对这个接口的访问的请求**。可以通过这个名为`request`对象对session进行获取。每一个不同的请求都会有一个唯一的`session`，每个`session`中的信息都是`key-value`形式储存，这里我们只在session里面储存用户信息，因此我们把用户信息的key设为一个固定的名字`userInfo`，通过建立个常量`SESSION_NAME`。

每个不同机器的请求都会生成独一无二的session，上面这段代码操作，我们可以理解为：登录/注册用户后，取得了用户信息，我们将每个不同机器的用户信息储存在了与它们相对应的session里面，并在其中设定用户信息的`key`为`userInfo`。

> 其实，session中内容储存形式和cookie是一样的，都是key-value的形式，我们可以理解为和Java中的Map对象是差不多的。这里session指的是session机制中储存在服务端的用户信息。

通过`HttpServletRequest`的`getSession`方法，即可获取这个请求对应的session对象，为`HttpSession`类型，其中`setAttribute`方法用于设定session中的键值对，`getAttribute`方法用于获取session中信息。

> 可见，session中的内容是由我们自定义的，只不过通常我们需要把用户对象存进去以验证是否登录。上述我们只存放了用户对象在session里面，实际开发中大家还可以存点别的东西进去。当然不建议存太多信息，否则会增加服务器负载。

这里我们还将session对象传入上述Service层的判断用户是否登录方法中，在其中对session中用户信息进行读取，然后利用session中用户对象的id去数据库中取出并进行比对判断用户是否有效登录。

用户没有登录，则session获取到的用户对象就一定是`null`。

对于这里API类的代码，建议大家可以联系上面的session机制示意图一起看，这样就更好理解。

## 配置cookie属性

其实到上面第7步，我们的功能基本上完整了，但是还有一些重要配置需要进行。

我们还要开启session功能，并配置cookie属性例如过期时间等等。

新建包`config`，在其中建立配置类`SessionConfig`：

```java
package com.example.userlogin.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.session.MapSessionRepository;
import org.springframework.session.config.annotation.web.http.EnableSpringHttpSession;
import org.springframework.session.web.http.CookieSerializer;
import org.springframework.session.web.http.DefaultCookieSerializer;

import java.util.concurrent.ConcurrentHashMap;

/**
 * session配置类
 */
@Configuration
@EnableSpringHttpSession
public class SessionConfig {

	/**
	 * 设定cookie序列化器的属性
	 */
	@Bean
	public CookieSerializer cookieSerializer() {
		DefaultCookieSerializer serializer = new DefaultCookieSerializer();
		serializer.setCookieName("JSESSIONID");
		// 用正则表达式配置匹配的域名，可以兼容 localhost、127.0.0.1 等各种场景
		serializer.setDomainNamePattern("^.+?\\.(\\w+\\.[a-z]+)$");
		// cookie生效路径
		serializer.setCookiePath("/");
		// 设置是否只能服务器修改，浏览器端不能修改
		serializer.setUseHttpOnlyCookie(false);
		// 最大生命周期的单位是分钟
		serializer.setCookieMaxAge(24 * 60 * 60);
		return serializer;
	}

	/**
	 * 注册序列化器
	 */
	@Bean
	public MapSessionRepository sessionRepository() {
		return new MapSessionRepository(new ConcurrentHashMap<>());
	}

}
```

注意配置类打上`@Configuration`注解，`@EnableSpringHttpSession`开启session。

## 配置拦截器

虽然我们有判断用户登录的API，但是如果我们页面很多，每一个都要判断登录，就会很麻烦。通过拦截器即可对指定的路径设定拦截点。

继续在`config`包下创建拦截器类`UserInterceptor`：

```java
package com.example.userlogin.config;

import com.example.userlogin.service.UserService;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;



/**
 * 拦截器
 */
public class UserInterceptor implements HandlerInterceptor {

	@Autowired
	private UserService userService;

	// Controller方法执行之前
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
		// 同样在这里调用用户服务传入session，判断用户是否登录或者有效
		// 未登录则重定向至主页（假设主页就是/）
		if (!userService.isLogin(request.getSession()).isSuccess()) {
			response.sendRedirect("/");
			return false;
		}
		return true;
	}

	// Controller方法执行之后
	@Override
	public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

	}

	// 整个请求完成后（包括Thymeleaf渲染完毕）
	@Override
	public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

	}

}
```

可见拦截器有三个方法，分别对应三个切入点。一般我们只需要修改在Controller执行之前的那个，它可以像API一样操作session，返回true时表示允许继续访问这个Controller，否则终止访问。通过`HttpServletResponse`的`sendRedirect`方法可以发送重定向。

拦截器类写好了，接下来就是注册拦截器了。在`config`类中创建配置类`InterceptorRegister`：

```java
package com.example.userlogin.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.ArrayList;
import java.util.List;

/**
 * 拦截器注册
 */
@Configuration
public class InterceptorRegister implements WebMvcConfigurer {

   /**
    * 把我们定义的拦截器类注册为Bean
    */
   @Bean
   public HandlerInterceptor getInterceptor() {
      return new UserInterceptor();
   }

   /**
    * 添加拦截器，并配置拦截地址
    */
   @Override
   public void addInterceptors(InterceptorRegistry registry) {
      List<String> pathPatterns = new ArrayList<>();
      pathPatterns.add("/update");
      registry.addInterceptor(getInterceptor()).addPathPatterns(pathPatterns);
   }

}
```

在`addInterceptors`方法里面，我们进行拦截器注册，并配置拦截地址。上述例子只添加拦截`/update`这个路径，其余不拦截。

还可以设定拦截全部，只排除`/login`，如下写`addInterceptors`方法：

```java
@Override
public void addInterceptors(InterceptorRegistry registry) {
	List<String> pathPatterns = new ArrayList<>();
	pathPatterns.add("/login");
	registry.addInterceptor(getInterceptor()).excludePathPatterns(pathPatterns);
}
```



## 体验一下

启动程序，并使用ApiPost软件来体验一下用户登录等服务接口的功能。

### (1) 注册一个用户

先访问`/register`注册一个用户：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b6d704aa8eea4d5d8018a4a79da7986a~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### (2) 测试判断登录接口

先不急着登录，访问`/is-login`看看：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a34a964de35645fc955a8b0995bf7daf~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

可见由于我们还未登录，所以请求中没有`sessionId`，后端也无法找到这个请求的session数据，因此返回未登录。

### (3) 登录然后再次判断

这次我们访问`/login`登录：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9c60a2ef21074999a8ad9fb569db06d7~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

然后再访问`/is-login`接口：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/95c35e0c789f4095850f758dcf0b8ade~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

可见这一次，我们的请求中就带着`sessionId`了！服务端也可以找到对应的`session`。

现在许多系统都是前后端分离的系统，因此这个判断登录`/is-login`接口也是很有必要的，除了判断这个请求是否已经登录之外，还可以直接拿取已登录的用户的信息，而无需传入`id`去查询。

## 总结

用户登录注册看起来要写的东西很多，但实际上流程很清晰，也不难理解。

我们发现，DAO层就是单纯操作数据库，返回用户对象；Service层基本上就是用于验证信息正确性，返回封装的`Result`对象；Controller层进一步设定session，也是返回封装`Result`对象。每个不同的部分各司其职，完成了我们整个业务逻辑。

其实不仅仅是用户登录系统，我们使用Spring Boot搭建任何系统，无外乎都是以下几个大步骤：

1. 构建数据模型：`dataobject`包的内容和`model`包的内容，`dataobject`一般是数据库中的对象，最好是画个类图
2. 编写DAO层：用于操作数据库，定义好需要用到的操作数据库的方法例如增删改查、根据用户名查找用户等等
3. 编写Service层：即为服务层，用于调用DAO层，一般来说我们会把大量的代码写在这里，这里包含了许多逻辑，一个网站有哪些服务（用户登录，注册等等），都定义在这层，这一层是操作DAO层并处理数据的一层，也包含了业务逻辑
4. 编写API层：即为接口，是处在最外面的一层了，调用Service层，是前后端交流的桥梁

- 示例程序仓库地址：[传送门](https://link.juejin.cn/?target=https%3A%2F%2Fgitee.com%2Fswsk33%2Fexamples-and-learning%2Ftree%2Fmaster%2FSpring%20Boot%E7%94%A8%E6%88%B7%E7%99%BB%E5%BD%95)
- ApiPost测试配置：[传送门](https://link.juejin.cn/?target=https%3A%2F%2Fconsole-docs.apipost.cn%2Fpreview%2Ff9e270de687f5013%2F4333094879904391)













































