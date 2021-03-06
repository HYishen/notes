### spring context
Spring容器

BeanFactory 是 IoC 底层容器

BeanFactory最根本的核心类:DefaultListableBeanFactory

FactoryBean 是 创建 Bean 的一种方式，帮助实现复杂的初始化逻辑

### BeanDefinition
Bean定义

BeanDefinitionBuilder可以用来构建BeanDefinition

而applicationContext.registerBeanDefinition可以用来注册Bean

凡是Bean的注册都是BeanDefinitionRegistry来进行注册，最后会存到基本上唯一的实现DefaultListableBeanFactory，然后做一个map进行保存，通过List的方式来存储它的名称

#### Java注解配置元信息

```
// 使用Java注解配置元信息时，方法参数可以通过org.springframework.beans.factory.annotation.Qualifier来指定入参的beanId
@Bean
public UserHolder userHolder(@Qualifier("user") User user) {
    UserHolder userHolder = new UserHolder();
    userHolder.setUser(user);
    return userHolder;
}
```

### AnnotationConfigUtils
org.springframework.context.annotation.AnnotationConfigUtils
// 在给定的注册表中注册所有相关的注释后处理器
org.springframework.context.annotation.AnnotationConfigUtils#registerAnnotationConfigProcessors(org.springframework.beans.factory.support.BeanDefinitionRegistry, java.lang.Object)
