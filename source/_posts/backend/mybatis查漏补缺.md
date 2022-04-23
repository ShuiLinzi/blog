---
title: mybatis查漏补缺
date: 2022-4-20
cover: https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/mybatis.webp
---
# 面向接口编程保持两个一致
:::info
mybatis核心配置文件：数据库信息和mapper.xml
在application.yml中配置，不需要额外配置了
:::
MyBatis面向接口编程的两个一致：
1. 映射文件的homespace要和napper接口的全类名保持一致
2. 映射文件中SQL语句的id要和mapper接口中的方法名一致
## 从 XML 中构建 SqlSessionFactory
每个基于 MyBatis 的应用都是以一个 SqlSessionFactory 的实例为核心的。SqlSessionFactory 的实例可以通过 SqlSessionFactoryBuilder 获得。而 SqlSessionFactoryBuilder 则可以从 XML 配置文件或一个预先配置的 Configuration 实例来构建出 SqlSessionFactory 实例。

从 XML 文件中构建 SqlSessionFactory 的实例非常简单，建议使用类路径下的资源文件进行配置。 但也可以使用任意的输入流（InputStream）实例，比如用文件路径字符串或 file:// URL 构造的输入流。MyBatis 包含一个名叫 Resources 的工具类，它包含一些实用方法，使得从类路径或其它位置加载资源文件更加容易。

```java
String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```


## 打印sql语句
使用log4j的jar包，+一个配置文件

# xml里面的SQL语句
## resultMap
查询功能必须设置resultType或者resultMap
resultType：设置默认的映射关系
resultMap：设置自定义映射关系

```xml
    <select id="listRolesByUserInfoId" resultType="java.lang.String">
        SELECT
         role_label
        FROM
         tb_role r,
         tb_user_role ur
         WHERE
         r.id = ur.role_id
        AND ur.user_id = #{userInfoId}
    </select>
```
## 占位符
1. mapper接口方法的参数为单个的字面量类型
可以通过`${}`,`#{}`以任意的名称获取参数值，但是需要注意`${}`的单引号问题
2. mapper接口方法的参数为多个时,此时MyBatis会将这些参数放在一个map集合中，以两种方式进行存储
   - 以arg0，arg1..为键进行传递
   - 以param1，param2..为键进行传递

3. 使用Param,注解命名参数此MyBatis会将这些参数放在一个map集合中，以两种方式进行存储以@Param注解的值为键，以参数为值因此只需要通过#{}和${}以键的方式访问值即可，但是需要注意${}的单引号问题

## 特殊sql
### 模糊查询
```xml 方式一
<select id="findByName" parameterType="string" resultType="com.sc.domain.User">
 select * from user where username like '%${value}%'
</select>
```

```xml 方式二推荐
<select id="findByName" parameterType="string" resultType="com.sc.domain.User">
 select * from user where username like "%"#{value}"%"
</select>
```

### 驼峰映射
全局配置文件中有关于驼峰的配置，默认为false，可查看官网


## resultMap
设置自定义映射关系，主要用于**多对一**或者**一对多**的联表查询
- id：唯一标识，不能重复
- type：设置映射关系中的实体类的类型

### 多对一联表查询
1. 级联属性查询
2. association查询
3. association的分布查询

### 一对多联表查询
collection
```xml
<!-- property="tagDTOList"对应的属性字段 -->
 <!-- ofType="com.minzheng.blog.dto.TagDTO" 里面具体的值 -->
  <collection property="tagDTOList" ofType="com.minzheng.blog.dto.TagDTO">
            <id column="tag_id" property="id"/>
            <result column="tag_name" property="tagName"/>
        </collection>
```
分布查询

# 分页查询
1. 导入pageHelper的jar包
2. 在mybatis的核心配置文件中配置pageHelper的插件配置

limit index,pageSize
- index:当前页的起始索引
- pageSize:每页显示的条数
- pageNum:当前页的页码


1. 在查询之前开启分页
2. 在查询之后获取分页相关信息   