# Spring学习笔记

[toc]

## BeanFactory和applicationContext的本质区别

> 司马老师说 前者是懒加载，后者是非懒加载（可以指定为非懒加载）是前者的扩张。

## 什么是控制反转

> 就是依赖倒置原则，生产汽车的例子

> 核心思想:资源不由使用资源的双方管理，而由不使用资源的第三方管理，这可以带来很多好处。
>
> 第一，资源集中管理，实现资源的可配置和易管理。第二，降低了使用资源双方的依赖程度，也就是我们说的耦合度。

```java
@Bean
@Lazy
@Scope(scopeName = "prototype")
```

@Conditional注解 springboot 大量使用 onmissing onclass

```java
@Bean
@Conditional(value = TulingCondition.class)
public TulingLog tulingLog(){
  return new TulingLog();
}

public class TulingCondition implements Condition{
  public boolean matches(ConditionContext context,AnnotatedTypedMetadata metadata){
    if(context.getBeanFactory().containsBean("tulingAspect")){
      return true;
    }
    return false;
  }
}
```



## 往ioc容器添加组件

* @Bean

* @CompentScan + @Controller + @Service + @ Compent @ Repository

* @Import

  ```java
  @Import(value={Person.class,TulingImportSelector.class，ImportBeanDefinitionRegistrar.class})
  
  public class TulingImportSelector implements ImportSelector{
    public String[] selectImports(AnnotationMetadata importingClassMetadata){
      return new String[]{"com.tuling.testimport.compent.Dog"};
    }
  }
  
  public class TulingBeanDefinitionRegister implements ImportBeanDefinitionRegistrar{
    public void registerBeanDefinitions(...){
      RootBeandefinition rootBeanDefinition = new RootBeanDefinition(Cat.class);
    }
  }
  ```

* ImportBeanDefinitionRegister

*  通过实现factoryBean接口来实现注册 组件

  ```java
  public class CarFactoryBean implements FactoryBean<Car>{
     public Car getObject(){
       return new Car();
     }
     public Class<?> getObjectType(){
       return Car.class;
     } 
    
    public boolean isSingleton(){
      return true;
    }
      
  }
  ```



## Bean的生命周期

* 创建 之前

  * 初始化容器
  * 加载BeanFactoryPostProcessor实现类
  * 执行BeanFactoryPostProcessor.postProcessBeanFactory方法（主要是用来自定义修改持有的beandefinition）

* 创建 和 初始化 

  * Aware接口相关
  * BeanPostProcessor.postProcessBeforeInitialization
  * InitializingBean接口 afterPropertiesSet()方法
  * 指定的init方法
  * JSR250 @PostConstruct

  * BeanPostProcessor.postProcessAfterInitialization

* 销毁  ( 如果是prototype 模式，那么不受spring容器管理）

  (1)实现 disposableBean接口

  (2)指定的desotry方法

  (3) JSR250 @ProDestory

## Bean 和 BeanDefinition的区别

> Bean 根据BeanDefinition生成

## BeanDefinitionRegistryPostProcossor

> 继承自 BeanFactoryPostProcessor
>
> ​	postProcessBeanDefinitionRegistry: beanDefinition 加载之前 ，但是有7个(6+1)框架自己的beanDefinition
>
> ​    postProcessBeanFactory: beanDefinition加载之后

## filter 拦截器 AOP

* filter 是依赖servlet，只能获取url

* 拦截器 url 方法名称 但是不能参数名称
* AOP 都可以

## resources 文件夹下面的template 和static文件夹的加载顺序

> DefaultErrorViewResolver.java 先找     template/error/404没有再找静态资源

* 静态资源包含四个路径
  * /META-INF/resources/
  * /resources/
  * /static/
  * /public/