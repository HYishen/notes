### spring context
Spring容器

BeanFactory 是 IoC 底层容器

FactoryBean 是 创建 Bean 的一种方式，帮助实现复杂的初始化逻辑

### BeanDefinition
Bean定义

BeanDefinitionBuilder可以用来构建BeanDefinition

而applicationContext.registerBeanDefinition可以用来注册Bean

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
