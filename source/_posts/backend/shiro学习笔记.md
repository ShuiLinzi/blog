# shiro学习笔记

# 认证

## shiro中认证的关键对象

- Subject：主体
  访问系统的用户，主体可以是用户、程序等，进行认证的都称为主体；

- Principal：身份信息
  是主体（subject）进行身份认证的标识，标识必须具有唯一性，如用户名、手机号、邮箱地址等，一个主体可以有多个身份，但是必须有一个主身份（Primary Principal）。

- credential：凭证信息
  是只有主体自己知道的安全信息，如密码、证书等。

## 使用shiro

### 引入依赖

```xml
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-core</artifactId>
  <version>1.5.3</version>
</dependency>
```

### 自定义Realm

>认证：1.最终执行用户名比较是 在`SimpleAccountRealm`类 的 `doGetAuthenticationInfo `方法中完成用户名校验
>
>2.最终密码校验是在 `AuthenticatingRealm`类 的` assertCredentialsMatch`方法 中
>
>总结：
>
>`AuthenticatingRealm` 认证realm` doGetAuthenticationInfo`
>
>`AuthorizingRealm `授权realm `doGetAuthorizationInfo`

重写`AuthorizingRealm`里面的授权和认证方法，实现自定义认证和授权

````java
package com.lut.realm;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;

/**
 * 自定义Realm
 */
public class CustomerRealm extends AuthorizingRealm {
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        System.out.println("==================");
        return null;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        //在token中获取 用户名
        String principal = (String) token.getPrincipal();
        System.out.println(principal);

        //实际开发中应当 根据身份信息使用jdbc mybatis查询相关数据库
        //在这里只做简单的演示
        //假设username,password是从数据库获得的信息
        String username="zhangsan";
        String password="123456";
        if(username.equals(principal)){
            //参数1:返回数据库中正确的用户名
            //参数2:返回数据库中正确密码
            //参数3:提供当前realm的名字 this.getName();
            SimpleAuthenticationInfo simpleAuthenticationInfo = new SimpleAuthenticationInfo(principal,password,this.getName());
            return simpleAuthenticationInfo;
        }
        return null;
    }
}
````

### 简单的测试demo

```java
package com.lut.test;

import com.lut.realm.CustomerRealm;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;

/**
 * 测试自定义的Realm
 */
public class TestAuthenticatorCusttomerRealm {

    public static void main(String[] args) {
        //1.创建安全管理对象 securityManager
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();

        //2.给安全管理器设置realm（设置为自定义realm获取认证数据）
        defaultSecurityManager.setRealm(new CustomerRealm());
        //IniRealm realm = new IniRealm("classpath:shiro.ini");

        //3.给安装工具类中设置默认安全管理器
        SecurityUtils.setSecurityManager(defaultSecurityManager);

        //4.获取主体对象subject
        Subject subject = SecurityUtils.getSubject();

        //5.创建token令牌
        UsernamePasswordToken token = new UsernamePasswordToken("zhangsan", "123");
        try {
            subject.login(token);//用户登录
            System.out.println("登录成功~~");
        } catch (UnknownAccountException e) {
            e.printStackTrace();
            System.out.println("用户名错误!!");
        }catch (IncorrectCredentialsException e){
            e.printStackTrace();
            System.out.println("密码错误!!!");
        }

    }
}
```

### 密码加密

1. 自定义md5+salt的realm

自定义继承`AuthorizingRealm`类

```java
package com.lut.realm;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;

/**
 * 使用自定义realm 加入md5 + salt +hash
 */
public class CustomerMd5Realm extends AuthorizingRealm {

    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        return null;
    }

    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        //获取 token中的 用户名
        String principal = (String) token.getPrincipal();

        //假设这是从数据库查询到的信息
        String username="zhangsan";
        String password="7268f6d32ec8d6f4c305ae92395b00e8";//加密后

        //根据用户名查询数据库
        if (username.equals(principal)) {
            //参数1:数据库用户名
            //参数2:数据库md5+salt之后的密码
            //参数3:注册时的随机盐
            //参数4:realm的名字
            return new SimpleAuthenticationInfo(principal,
                    password,
                    ByteSource.Util.bytes("@#$*&QU7O0!"),
                    this.getName());
        }
        return null;
    }
}
```

消费者：

```java
package com.lut.test;

import com.lut.realm.CustomerMd5Realm;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;

import java.util.Arrays;

public class TestCustomerMd5RealmAuthenicator {

    public static void main(String[] args) {

        //1.创建安全管理器
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();

        //2.注入realm
        CustomerMd5Realm realm = new CustomerMd5Realm();

        //3.设置realm使用hash凭证匹配器
        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
        //声明：使用的算法
        credentialsMatcher.setHashAlgorithmName("md5");
        //声明：散列次数
        credentialsMatcher.setHashIterations(1024);
        realm.setCredentialsMatcher(credentialsMatcher);
        defaultSecurityManager.setRealm(realm);

        //4.将安全管理器注入安全工具
        SecurityUtils.setSecurityManager(defaultSecurityManager);

        //5.通过安全工具类获取subject
        Subject subject = SecurityUtils.getSubject();

        //6.认证
        UsernamePasswordToken token = new UsernamePasswordToken("zhangsan", "123");

        try {
            subject.login(token);
            System.out.println("登录成功");
        } catch (UnknownAccountException e) {
            e.printStackTrace();
            System.out.println("用户名错误");
        }catch (IncorrectCredentialsException e){
            e.printStackTrace();
            System.out.println("密码错误");
        }



    }
}


```

