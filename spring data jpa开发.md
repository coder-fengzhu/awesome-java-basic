## 什么是JPA及Spring Data JPA
### JPA
<font style="color:rgb(36, 41, 46);">JPA（Java Persistence API）是 Java 标准中的一套ORM规范（提供了一些编程的 API 接口，具体实现由 ORM 厂商实现，如Hiernate、TopLink 、Eclipselink等都是 JPA 的具体实现），借助 JPA 技术可以通过注解或者XML描述【对象-关系表】之间的映射关系，并将实体对象持久化到数据库中（即Object Model与Data Model间的映射）。</font>

<font style="color:rgb(36, 41, 46);">JPA是Java持久层API，由Sun公司开发，希望规范、简化Java对象的持久化工作，整合ORM技术，整合第三方ORM框架，建立一种标准的方式，目前也是在按照这个方向发展，但是还没能完全实现。在ORM框架中，Hibernate框架做了较好的 JPA 实现，已获得Sun的兼容认证。</font>

<font style="color:rgb(36, 41, 46);"></font>

### <font style="color:rgb(36, 41, 46);">ORM</font>
关系型数据库

![](https://cdn.nlark.com/yuque/0/2024/png/26411187/1715611653702-9adf6ca3-90c6-4060-bbfb-2d12c13ee6c6.png)



Java对象

![](https://cdn.nlark.com/yuque/0/2024/png/26411187/1715611689975-dfb60feb-1e5c-4d3f-88a7-f47295e82aec.png)

### <font style="color:rgb(36, 41, 46);">Spring Data JPA</font>
<font style="color:rgb(36, 41, 46);">Spring Data JPA是在实现了 JPA 规范的基础上封装的一套 JPA 应用框架（Criteria API还是有些复杂）。虽然ORM框架都实现了 JPA 规范，但是在不同的ORM框架之间切换仍然需要编写不同的代码，而使用Spring Data JPA能够方便的在不同的ORM框架之间进行切换而不需要更改代码。Spring Data JPA旨在通过统一ORM框架的访问持久层的操作，来提高开发人的效率。</font>

<font style="color:rgb(36, 41, 46);">Spring Data JPA是一个 JPA 数据访问抽象。也就是说Spring Data JPA不是一个实现或 JPA 提供的程序，它只是一个抽象层，主要用于减少为各种持久层存储实现数据访问层所需的样板代码量。但是它还是需要JPA提供实现程序，其实Spring Data JPA底层就是使用的 Hibernate实现。</font>

<font style="color:rgb(36, 41, 46);"></font>

![](https://cdn.nlark.com/yuque/0/2024/png/26411187/1715006292877-6d50babe-1d99-49f6-ac91-5e697fb9b5a8.png)



## 查询的方式
### 直接通过方法名查询
**<font style="color:rgb(36, 41, 46);">查询关键字</font>**

<font style="color:rgb(36, 41, 46);">在查询时，通常需要同时根据多个属性进行查询，且查询的条件也格式各样（大于某个值、在某个范围等等），SpringDataJPA 为此提供了一些表达条件查询的关键字，官方文档如下：</font>

| **<font style="color:rgb(36, 41, 46);">keyword</font>** | **<font style="color:rgb(36, 41, 46);">Sample</font>** | **<font style="color:rgb(36, 41, 46);">JPQL snippet</font>** |
| --- | --- | --- |
| <font style="color:rgb(36, 41, 46);">Distinct</font> | <font style="color:rgb(36, 41, 46);">findDistinctByLastnameAndFirstname</font> | <font style="color:rgb(36, 41, 46);">select distinct … where x.lastname = ?1 and x.firstname = ?2</font> |
| <font style="color:rgb(36, 41, 46);">And</font> | <font style="color:rgb(36, 41, 46);">findByLastnameAndFirstname</font> | <font style="color:rgb(36, 41, 46);">… where x.lastname = ?1 and x.firstname = ?2</font> |
| <font style="color:rgb(36, 41, 46);">Or</font> | <font style="color:rgb(36, 41, 46);">findByLastnameOrFirstname</font> | <font style="color:rgb(36, 41, 46);">… where x.lastname = ?1 or x.firstname = ?2</font> |
| <font style="color:rgb(36, 41, 46);">Is, Equals</font> | <font style="color:rgb(36, 41, 46);">findByFirstname   </font><font style="color:rgb(36, 41, 46);">findByFirstnameIs   </font><font style="color:rgb(36, 41, 46);">findByFirstnameEquals</font> | <font style="color:rgb(36, 41, 46);">… where x.firstname = ?1</font> |
| <font style="color:rgb(36, 41, 46);">Between</font> | <font style="color:rgb(36, 41, 46);">findByStartDateBetween</font> | <font style="color:rgb(36, 41, 46);">… where x.startDate between ?1 and ?2</font> |
| <font style="color:rgb(36, 41, 46);">LessThan</font> | <font style="color:rgb(36, 41, 46);">findByAgeLessThan</font> | <font style="color:rgb(36, 41, 46);">… where x.age < ?1</font> |
| <font style="color:rgb(36, 41, 46);">LessThanEqual</font> | <font style="color:rgb(36, 41, 46);">findByAgeLessThanEqual</font> | <font style="color:rgb(36, 41, 46);">… where x.age <= ?1</font> |
| <font style="color:rgb(36, 41, 46);">GreaterThan</font> | <font style="color:rgb(36, 41, 46);">findByAgeGreaterThan</font> | <font style="color:rgb(36, 41, 46);">… where x.age > ?1</font> |
| <font style="color:rgb(36, 41, 46);">GreaterThanEqual</font> | <font style="color:rgb(36, 41, 46);">findByAgeGreaterThanEqual</font> | <font style="color:rgb(36, 41, 46);">… where x.age >= ?1</font> |
| <font style="color:rgb(36, 41, 46);">After</font> | <font style="color:rgb(36, 41, 46);">findByStartDateAfter</font> | <font style="color:rgb(36, 41, 46);">… where x.startDate > ?1</font> |
| <font style="color:rgb(36, 41, 46);">Before</font> | <font style="color:rgb(36, 41, 46);">findByStartDateBefore</font> | <font style="color:rgb(36, 41, 46);">… where x.startDate < ?1</font> |
| <font style="color:rgb(36, 41, 46);">IsNull, Null</font> | <font style="color:rgb(36, 41, 46);">findByAge(Is)Null()</font> | <font style="color:rgb(36, 41, 46);">… where x.age is null</font> |
| <font style="color:rgb(36, 41, 46);">IsNotNull, NotNull</font> | <font style="color:rgb(36, 41, 46);">findByAge(Is)NotNull()</font> | <font style="color:rgb(36, 41, 46);">… where x.age not null</font> |
| <font style="color:rgb(36, 41, 46);">Like</font> | <font style="color:rgb(36, 41, 46);">findByFirstnameLike</font> | <font style="color:rgb(36, 41, 46);">… where x.firstname like ?1</font> |
| <font style="color:rgb(36, 41, 46);">NotLike</font> | <font style="color:rgb(36, 41, 46);">findByFirstnameNotLike</font> | <font style="color:rgb(36, 41, 46);">… where x.firstname not like ?1</font> |
| <font style="color:rgb(36, 41, 46);">StartingWith</font> | <font style="color:rgb(36, 41, 46);">findByFirstnameStartingWith</font> | <font style="color:rgb(36, 41, 46);">… where x.firstname like ?1   </font><font style="color:rgb(36, 41, 46);">(parameter bound with appended %)</font> |
| <font style="color:rgb(36, 41, 46);">EndingWith</font> | <font style="color:rgb(36, 41, 46);">findByFirstnameEndingWith</font> | <font style="color:rgb(36, 41, 46);">… where x.firstname like ?1   </font><font style="color:rgb(36, 41, 46);">(parameter bound with prepended %)</font> |
| <font style="color:rgb(36, 41, 46);">Containing</font> | <font style="color:rgb(36, 41, 46);">findByFirstnameContaining</font> | <font style="color:rgb(36, 41, 46);">… where x.firstname like ?1   </font><font style="color:rgb(36, 41, 46);">(parameter bound wrapped in %)</font> |
| <font style="color:rgb(36, 41, 46);">OrderBy</font> | <font style="color:rgb(36, 41, 46);">findByAgeOrderByLastnameDesc</font> | <font style="color:rgb(36, 41, 46);">… where x.age = ?1 order by x.lastname desc</font> |
| <font style="color:rgb(36, 41, 46);">Not</font> | <font style="color:rgb(36, 41, 46);">findByLastnameNot</font> | <font style="color:rgb(36, 41, 46);">… where x.lastname <> ?1</font> |
| <font style="color:rgb(36, 41, 46);">In</font> | <font style="color:rgb(36, 41, 46);">findByAgeIn(Collection ages)</font> | <font style="color:rgb(36, 41, 46);">… where x.age in ?1</font> |
| <font style="color:rgb(36, 41, 46);">NotIn</font> | <font style="color:rgb(36, 41, 46);">findByAgeNotIn(Collection ages)</font> | <font style="color:rgb(36, 41, 46);">… where x.age not in ?1</font> |
| <font style="color:rgb(36, 41, 46);">True</font> | <font style="color:rgb(36, 41, 46);">findByActiveTrue()</font> | <font style="color:rgb(36, 41, 46);">… where x.active = true</font> |
| <font style="color:rgb(36, 41, 46);">False</font> | <font style="color:rgb(36, 41, 46);">findByActiveFalse()</font> | <font style="color:rgb(36, 41, 46);">… where x.active = false</font> |
| <font style="color:rgb(36, 41, 46);">IgnoreCase</font> | <font style="color:rgb(36, 41, 46);">findByFirstnameIgnoreCase</font> | <font style="color:rgb(36, 41, 46);">… where UPPER(x.firstname) = UPPER(?1)</font> |


### 写原生sql查询


---

<font style="color:rgb(24, 25, 28);">大家学习Java碰到问题欢迎加入知识星球「风筑的编程圈子」，答疑所有碰到的编程问题，面试问题以及职业发展问题，欢迎加入</font>

<font style="color:rgb(24, 25, 28);"></font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/26411187/1719459784632-b665952c-7260-492c-af91-faa772aa4c88.jpeg)

<font style="color:rgb(24, 25, 28);"></font>



