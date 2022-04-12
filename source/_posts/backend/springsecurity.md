---
title: springsecurity
---
## 1.快速开始
### 1.引入spring security
```xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-security -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
    <version>2.6.6</version>
</dependency>
```
引入之后访问任何接口都会跳转到spring security提供的登陆页面，默认用户名user，密码控制台会输出

![security](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/security.webp)
## 2.认证
### 2.1登录校验流程
![jwt流程](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/jwt流程.webp)

### 2.2spring security完整流程
SpringSecurity的原理其实就是一个过滤器链，内部包含了提供各种功能的过滤器
![过滤器链](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/过滤器链.webp)

图中只展示了核心过滤器，其他非核心过滤器没有展示
- UsernamePasswordAuthenticationFilter：此过滤器是最常用的身份验证过滤器，也是最常定制的过滤器，负责处理在登录页面填写的用户名密码
- ExceptionTranslationFilter：处理过滤器中抛出的AccessDeniedException或AuthenticationException异常
- FilterSecurityInterceptor：负责权限校验的过滤器

我们可以debug查看过滤器链的先后顺序，一共有十五个
![spring调试](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/spring调试.webp)

### 认证流程详解
![认证流程](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/认证流程.webp)
Authentication接口：它的实现类，表示当前访问系统的用户，封装了用户相关信息。
AuthenticationManager接口：定义了认证Authenticationl的方法
UserDetailsService接口：加载用户特定数据的核心接口。里面定义了一个根据用户名查询用户信息的方法。
UserDetails接口：提供核心用户信息。通过UserDetailsService根据用户名获取处理的用户信息要封装成UserDetails对象返回。然后将这些信息封装到Authentication对象中。

### 思路分析
![流程2](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/流程2.webp)
登录：
    1. 自定义登录接口
         调用ProviderManager的方法进行认证 如果认证通过生成jwt
         把用户信息存入redis中
    2. 自定义UserDetailService
        在这个实现类中查询数据库
检验：
    1. 定义jwt认证过滤器
        获取token
        解析token获取userid
        从redis中获取信息
        存入SecurityContextHolder中

## 自定义UserDetailService在这个实现类中查询数据库
我们可以自定义一个UserDetailsService，让springSecurity使用我们的UserDetailService，我们自己的UserDetailService可以从数据库中查询数据库用户名密码
创建一个类实现UserDetailsService接口，重写其中的方法。更加用户名从数据库中查询用户信息

```java

@Service
public class UserDetailsServiceImpl implements UserDetailsService {

    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        //根据用户名查询用户信息
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(User::getUserName,username);
        User user = userMapper.selectOne(wrapper);
        //如果查询不到数据就通过抛出异常来给出提示
        if(Objects.isNull(user)){
            throw new RuntimeException("用户名或密码错误");
        }
        //TODO 根据用户查询权限信息 添加到LoginUser中
        
        //封装成UserDetails对象返回 
        return new LoginUser(user);
    }
}
```

因为UserDetailsService方法的返回值是UserDetails类型，所以需要定义一个类，实现该接口，把用户信息封装在其中。
```java
/**
 * @Author 三更  B站： https://space.bilibili.com/663528522
 */
@Data
@NoArgsConstructor
@AllArgsConstructor
public class LoginUser implements UserDetails {

    private User user;


    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return null;
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getUserName();
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```