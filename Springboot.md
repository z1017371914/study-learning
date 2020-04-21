# Springboot

## dependencies 和dependencyManagement的区别

* dependencies 子moudle 会继承父的
* dependecyManagement 只会继承版本号

## 日期配置

```properties
//
date: 2019-12-01
  

spring:
	jackson:
 		data-format: yyyy-MM-dd
```

## 属性注入

```java
@ConfigurationProperties("prefix = "student")
@value
                         
@Value("#{${a}+${b}}")
                         
                         
@PropertySource(value = {"classpath:person.properties"})
                         
```



## 加载bean.xml配置文件

```
@ImportResource(locations = {"classpath:beans.xml"})
```



## 配置文件优先级

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdx64kjp9qj30ki07ntb6.jpg)

```
1.命令行参数
所有的配置都可以在命令行上进行指定
java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --server.port=8087 --server.context-path=/abc 多个配置用空格分开; --配置项=值
2.来自java:comp/env的JNDI属性 3.Java系统属性(System.getProperties()) 4.操作系统环境变量 5.RandomValuePropertySource配置的random.*属性值
由jar包外向jar包内进行寻找;
优先加载带profile 
6.jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件 
7.jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件
再来加载不带profile 
8.jar包外部的application.properties或application.yml(不带spring.profile)配置文件 
9.jar包内部的application.properties或application.yml(不带spring.profile)配置文件
10.@Configuration注解类上的@PropertySource 
11.通过SpringApplication.setDefaultProperties指定的默认属性
```

## Conditional的子类

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gdx790nc5pj30vi0kp4ci.jpg)