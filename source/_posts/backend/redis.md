---
title: redis缓存使用问题及解决办法
date: 2021-09-30
tags: 
- Java
categories:
- "后端"
---
[[toc]]
## 哪些数据适合放到缓存
- 即时性，数据一致性要求不高的
- 访问量大且更新频率不高的(读多，写少)
![缓存流程](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/缓存流程.2mcj5cis3se0.png)
## 本地缓存
项目是单体应用，很方便，很快，但是如果是分布式集群环境下，一旦对数据进行修改，比如对一号机器修改，但是由于负载均衡到二号三号机器的数据无法修改，也就是说本地缓存会产生数据一致性的问题。解决办法，使用服务器击群使用一个公共的共同的缓存中间件，redis能实现此功能
![分布式缓存](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/分布式缓存.3wivdp10sts.png)

## redis缓存
- 给缓存中放json字符串，json跨语言，跨平台兼容，拿出来的时候也是字符串，这样可以保持格式的统一

## 内存问题
### 堆外溢出异常
- springboot2.0以后默认使用lettcue作为操作redis的客户端。它使用netty进行网络通信
- lettuce的bug导致netty堆外内存溢出
### 解决方案
调大堆外内存并不能从根本上解决问题，迟早会出现这个问题<br/>
redisTemplate:lettcue,jedis都是操控redis的底层客户端。spring再次封装成了redisTemplate
- 升级lettuce客户端
- 切换使用jedis客户端(我使用了此种方法，因为视频里面用的是这个，2333)

## 高并发缓存失效问题
### 缓存穿透
缓存穿透指查询一个不存在的数据，由于缓存不命中，将去查询数据库，但是数据库也无此记录，我们没有将这次查询的null写入缓存，这将导致这个不存在的数据每次都要请求到存储层去查询，失去了缓存的意义<br><br>
风险:利用不存在的数据进行攻击，数据库瞬间压力增大，最终导致崩溃<br><br>
解决:将null结果缓存，并加入短暂的过期时间

### 缓存雪崩
缓存雪崩是指我们设置缓存时key采用了相同的过期时间，导致缓存在某一个时刻同时失效，请求全部转发到数据库，数据库瞬时压力过重雪崩<br><br>
解决:原有的失效时间基础上增加一个随机值，比如 1-5 分钟随机，这样每一个缓存的过期时间的
重复率就会降低，就很难引发集体失效的事件
### 缓存击穿
对于一些设置了过期时间的 key，如果这些 key 可能会在某些时间点被超高并发地访问，是一种非常热点的数据这个时候，需要考虑一个问题：如果这个 key 在大量请求同时进来前正好失效，那么所
有对这个 key 的数据查询都落到 db，我们称为缓存击穿。<br><br>
解决:加锁<br><br>
(个人感觉：上述问题不用缓存也会存在吧，比如没有缓存高并发直接访问数据库，不也会炸吗)
## 加锁
### 单机模式加锁
单机模式加锁，使用synchronized(this)进行加锁
### 集群模式加锁
由于本地锁只能锁住当前进程，所以我们需要分布式锁
- 问题1: setnx占好了位，业务代码异常或者程序在页面过程中宕机，没有执行删除锁的逻辑，就会造成死锁问题
- 问题2: 如果业务时间很长。锁自己过期了，我们直接删除，有可能把别人正在持有的锁给删除   
- 解决: 加锁删锁都必须保证原子性，给锁名字加上一个uuid避免重复，使用redis+lua脚本完成原子性删除<br>
关于续期问题，直接把缓存时间放长一点进行解决

## 整合redisson
引入依赖，配置redisson
以后现用现查吧..

## 使用spring cache
 ### 简介
 Spring 从 3.1 开始定义了 org.springframework.cache.Cache
和 org.springframework.cache.CacheManager 接口来统一不同的缓存技术；
并支持使用 JCache（JSR-107）注解简化我们开发
- 每次调用需要缓存功能的方法时，Spring 会检查检查指定参数的指定的目标方法是否已
经被调用过；如果有就直接从缓存中获取方法调用后的结果，如果没有就调用方法并缓
存结果后返回给用户。下次调用直接从缓存中获取
- 使用Spring 缓存抽象时我们需要关注以下两点
  - 1、确定方法需要被缓存以及他们的缓存策略
  - 2、从缓存中读取之前缓存存储的数据
### 整合spring cache简化缓存开发
- 引入依赖 spring-boot-stater-catch
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```
- 写配置
   - boot自动配置好了缓存管理器RediscacheManager
   - 手动配置redis作为缓存
```xml
spring.cache.type=redis

#spring.cache.cache-names=qq,毫秒为单位
spring.cache.redis.time-to-live=3600000

#如果指定了前缀就用我们指定的前缀，如果没有就默认使用缓存的名字作为前缀
#spring.cache.redis.key-prefix=CACHE_
spring.cache.redis.use-key-prefix=true

#是否缓存空值，防止缓存穿透
spring.cache.redis.cache-null-values=true
```
- 使用缓存注解
    -  @Cacheable  ：触发将数据保存到缓存的操作；
    -  @CacheEvict  : 触发将数据从缓存删除的操作；
    -  @CachePut ：不影响方法执行更新缓存；
    -  @Cacheing：组合以上多个操作；
    -  @CacheConfig：在类级别共享缓存的相同配置；
    1）开启缓存功能@EnableCaching
    2）只需要使用注解就能完成缓存操作
### @Cacheable
1.每个需要缓存的数据我们都来指定要放到那个名字的缓存，缓存分区(可以按照业务类型分)<br><br>
2.默认行为
- 如果缓存中有，方法不调用
- key默认自动生成
- 缓存的value值，默认使用jdk序列化机制，然后会把序列化后的数据存到redis中
- 默认ttl(生存时间time to live)为-1(永不过期)<br><br>

3.自定义
- 指定生成缓存使用的key，key属性指定，接受一个SpEL表达式(SpEL（Spring Expression Language），即Spring表达式语言)
- 指定缓存数据的存活时间，配置文件中修改ttl
- 将数据保存为json格式(改变序列化机制)

### spring cache的不足
1）、读模式

缓存穿透：查询一个null数据。解决方案：缓存空数据，可通过spring.cache.redis.cache-null-values=true
缓存击穿：大量并发进来同时查询一个正好过期的数据。解决方案：加锁 ? 默认是无加锁的;
使用sync = true来解决击穿问题
缓存雪崩：大量的key同时过期。解决：加随机时间。<br>
2）、写模式：（缓存与数据库一致）

读写加锁。
引入Canal，感知到MySQL的更新去更新Redis
读多写多，直接去数据库查询就行<br>
3）、总结：

常规数据（读多写少，即时性，一致性要求不高的数据，完全可以使用Spring-Cache）：

写模式(只要缓存的数据有过期时间就足够了)

特殊数据：特殊设计
