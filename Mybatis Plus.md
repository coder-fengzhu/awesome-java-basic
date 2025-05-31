## 简介
MyBatis Plus 是在 MyBatis 的基础上开发的增强工具包，旨在简化开发，提高效率。它提供了 CRUD 操作、条件构造器、分页插件、代码生成器等多种功能。

## 环境配置
### Maven 依赖
首先，在你的 `pom.xml` 文件中添加 MyBatis Plus 和相关依赖。



```xml
<dependencies>
    <!-- MyBatis Plus 依赖 -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.5.1</version>
    </dependency>
    <!-- MySQL 驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.23</version>
    </dependency>
    <!-- Spring Boot Starter Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

  	<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<scope>provided</scope>
		</dependency>
    <!-- 其他依赖 -->
    <!-- ... -->
</dependencies>
```



### Spring Boot 配置
在 `application.yml` 或 `application.properties` 文件中进行数据库配置。



```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/your_database?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
    username: your_username
    password: your_password
    driver-class-name: com.mysql.cj.jdbc.Driver

mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl //sql打印到控制台
```



## 快速开始
### 创建数据库表
以用户表 `user` 为例：



```sql
CREATE TABLE `user` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `name` varchar(30) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci DEFAULT NULL COMMENT '姓名',
  `age` int DEFAULT NULL COMMENT '年龄',
  `email` varchar(50) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci DEFAULT NULL COMMENT '邮箱',
  `version` bigint NOT NULL DEFAULT '0',
  `create_time` bigint NOT NULL,
  `update_time` bigint NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=24 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci
```



### 编写实体类


在 `com.example.demo.entity` 包下创建 `User` 类。



```java
package com.example.demo.entity;

import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.Data;

@Data
@TableName(value = "user")
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class User {

    @TableId(type = IdType.AUTO)
    private Long id;

    private String name;

    private Integer age;

    private String email;

    // @TableField(fill = FieldFill.INSERT, value = "create_time")
    // private Long createTime;

    // @TableField(fill = FieldFill.INSERT_UPDATE, value = "update_time")
    // private Long updateTime;

    // @Version
    // private Long version;
}

```



### 编写 Mapper 接口


在 `com.example.demo.mapper` 包下创建 `UserMapper` 接口。



```java
package com.example.demo.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.example.demo.entity.User;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserMapper extends BaseMapper<User> {
}
```

### 添加注解MapperScan
在Application类上添加`@MapperScan`注解并指定mapper的包路径



## 常用功能
### CRUD 操作
MyBatis Plus 提供了基础的 CRUD 方法，包括 `insert`、`delete`、`update`、`select` 等。



### 条件构造器
条件构造器主要涉及到3个类，`AbstractWrapper`，`QueryWrapper`，`UpdateWrapper`

在AbstractWrapper中提供了非常多的方法用于构建WHERE条件，而QueryWrapper针对SELECT语句，提供了select()方法，可自定义需要查询的列，而UpdateWrapper针对UPDATE语句，提供了set()方法，用于构造set语句，条件构造器也支持lambda表达式。

AbstractWrapper中用于构建SQL语句中的WHERE条件的方法进行部分列举

+ eq：equals，等于
+ allEq：all equals，全等于
+ ne：not equals，不等于
+ gt：greater than ，大于 >
+ ge：greater than or equals，大于等于≥
+ lt：less than，小于<
+ le：less than or equals，小于等于≤
+ between：相当于SQL中的BETWEEN
+ notBetween
+ like：模糊匹配。like("name","黄")，相当于SQL的name like '%黄%'
+ likeRight：模糊匹配右半边。likeRight("name","黄")，相当于SQL的name like '黄%'
+ likeLeft：模糊匹配左半边。likeLeft("name","黄")，相当于SQL的name like '%黄'
+ notLike：notLike("name","黄")，相当于SQL的name not like '%黄%'
+ isNull
+ isNotNull
+ in
+ and：SQL连接符AND
+ or：SQL连接符OR
+ apply：用于拼接SQL，该方法可用于数据库函数，并可以动态传参





```java
// 条件查询
QueryWrapper<User> queryWrapper = new QueryWrapper<>();
queryWrapper.eq("name", "Tom");
List<User> users = userMapper.selectList(queryWrapper);

// 条件更新
UpdateWrapper<User> updateWrapper = new UpdateWrapper<>();
updateWrapper.eq("name", "Tom").set("age", 27);
userMapper.update(updateWrapper);
```

<font style="color:rgb(37, 41, 51);"></font>

### <font style="color:rgb(37, 41, 51);">Condition</font>
条件构造器的诸多方法中，均可以指定一个boolean类型的参数condition，用来决定该条件是否加入最后生成的WHERE语句中，比如

```java
String name = "黄"; // 假设name变量是一个外部传入的参数
QueryWrapper<User> wrapper = new QueryWrapper<>();
wrapper.like(StringUtils.hasText(name), "name", name);
// 仅当 StringUtils.hasText(name) 为 true 时, 会拼接这个like语句到WHERE中
// 其实就是对下面代码的简化
if (StringUtils.hasText(name)) {
	wrapper.like("name", name);
}
```



### <font style="color:rgb(37, 41, 51);">实体对象作为条件</font>
调用构造函数创建一个Wrapper对象时，可以传入一个实体对象。后续使用这个Wrapper时，会以实体对象中的非空属性，构建WHERE条件（默认构建等值匹配的WHERE条件，这个行为可以通过实体类里各个字段上的@TableField注解中的condition属性进行改变）

```java
@Test
public void test3() {
    User user = new User();
    user.setName("abc");
    user.setAge(28);
    QueryWrapper<User> wrapper = new QueryWrapper<>(user);
    List<User> users = userMapper.selectList(wrapper);
    users.forEach(System.out::println);
}

```

<font style="color:rgb(37, 41, 51);"></font>

### <font style="color:rgb(37, 41, 51);">allEq方法</font>
allEq方法传入一个map，用来做等值匹配

当allEq方法传入的Map中有value为null的元素时，默认会设置为is null

若想忽略map中value为null的元素，可以在调用allEq时，设置参数boolean null2IsNull为false

```java
@Test
public void test3() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    Map<String, Object> param = new HashMap<>();
    param.put("age", 40);
    param.put("name", null);
    wrapper.allEq(param);
    List<User> users = userMapper.selectList(wrapper);
    users.forEach(System.out::println);
}


```

<font style="color:rgb(37, 41, 51);"></font>

### <font style="color:rgb(37, 41, 51);">lambda条件构造器</font>
lambda条件构造器，支持lambda表达式，可以不必像普通条件构造器一样，以字符串形式指定列名，它可以直接以实体类的方法引用来指定列。

像普通的条件构造器，列名是用字符串的形式指定，无法在编译期进行列名合法性的检查，这就不如lambda条件构造器来的优雅。

```java
@Test
public void testLambda() {
    LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
    wrapper.like(User::getName, "黄").lt(User::getAge, 30);
    List<User> users = userMapper.selectList(wrapper);
    users.forEach(System.out::println);
}

```



### 更新操作
+ updateById(T entity)

根据入参entity的id（主键）进行更新，对于entity中非空的属性，会出现在UPDATE语句的SET后面，即entity中非空的属性，会被更新到数据库，示例如下



+ update(T entity, Wrapper<T> wrapper)

根据实体entity和条件构造器wrapper进行更新，示例如下

```java
	@Test
	public void testUpdate2() {
		User user = new User();
		user.setName("abc");
		LambdaUpdateWrapper<User> wrapper = new LambdaUpdateWrapper<>();
		wrapper.between(User::getAge, 26,31).likeRight(User::getName,"吴");
		userMapper.update(user, wrapper);
	}

```



### 删除操作
BaseMapper一共提供了如下几个用于删除的方法

+ deleteById  根据主键id进行删除
+ deleteBatchIds  根据主键id进行批量删除
+ deleteByMap  根据Map进行删除（Map中的key为列名，value为值，根据列和值进行等值匹配）
+ delete(Wrapper<T> wrapper)  根据条件构造器Wrapper进行删除



### 自定义sql
+ 注解

```java
	@Select("select * from user")
	List<User> selectRaw();
```

+ xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.mp.mappers.UserMapper">
	<select id="selectRaw" resultType="com.example.mp.po.User">
        SELECT * FROM user
    </select>
</mapper>


public interface UserMapper extends BaseMapper<User> {
	List<User> selectRaw();
}
```



### 分页插件


配置分页插件，在 `MybatisPlusConfig` 类中添加分页插件配置。



```java
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL)); // 如果配置多个插件, 切记分页最后添加
        // 如果有多数据源可以不配具体类型, 否则都建议配上具体的 DbType
        return interceptor;
    }
```



在查询时使用 `Page` 对象进行分页。



```java
    @Test
    public void testPage() {
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.ge(User::getAge, 28);
        // 设置分页信息, 查第3页, 每页2条数据
        Page<User> page = new Page<>(3, 2);
        // 执行分页查询
        Page<User> userPage = userMapper.selectPage(page, wrapper);
        System.out.println("总记录数 = " + userPage.getTotal());
        System.out.println("总页数 = " + userPage.getPages());
        System.out.println("当前页码 = " + userPage.getCurrent());
        // 获取分页查询结果
        List<User> records = userPage.getRecords();
        records.forEach(System.out::println);
    }

```



### 代码生成器


MyBatis Plus 提供了代码生成器，可以根据数据库表自动生成实体类、Mapper 接口、Service 类和 Controller 类。



```java
public class Generator {

    public static void main(String[] args) {
        FastAutoGenerator.create("jdbc:mysql://localhost:3306/test?characterEncoding=utf-8",
                        "root", "")
                .globalConfig(builder -> builder
                        .author("aa")
                        .outputDir("your-path")
                )
                .packageConfig(builder -> builder
                        .parent("your-package")
                        .entity("entity")
                        .mapper("mapper")
                        .service("service")
                        .serviceImpl("service.impl")
                        .xml("mapper.xml")
                )
                .strategyConfig(builder -> builder
                        .entityBuilder()
                        .enableLombok()
                )
                .templateEngine(new FreemarkerTemplateEngine())
                .execute();
    }
}
```



## 高级功能
### 自动填充
create_time, update_time等字段需要自动填充，

```java
	@TableField(fill = FieldFill.INSERT) // 插入时自动填充
	private long createTime;
	@TableField(fill = FieldFill.INSERT_UPDATE) // 插入/更新时自动填充
	private long updateTime;

```



实现 MetaObjectHandler

```java
@Slf4j
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("开始插入填充...");
        this.strictInsertFill(metaObject, "createTime", LocalDateTime.class, LocalDateTime.now());
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("开始更新填充...");
        this.strictUpdateFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());
    }
}
```

### 乐观锁
乐观锁是一种并发控制机制，用于确保在更新记录时，该记录未被其他事务修改。MyBatis-Plus 提供了 OptimisticLockerInnerInterceptor 插件，使得在应用中实现乐观锁变得简单。



乐观锁的实现原理

乐观锁的实现通常包括以下步骤：

1. 读取记录时，获取当前的版本号（version）。
2. 在更新记录时，将这个版本号一同传递。
3. 执行更新操作时，设置 version = newVersion 的条件为 version = oldVersion。
4. 如果版本号不匹配，则更新失败。



#### 配置乐观锁插件
```java
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    }
```



#### Entity对象version字段加上`@version`注解
```java
import com.baomidou.mybatisplus.annotation.Version;

@Data
public class User {
    @TableId
    private Long id;
    private String name;
    private Integer age;
    private String email;
    @Version
    private Integer version;
}
```





