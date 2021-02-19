官网链接：

[https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-introduction](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-introduction)


In short, the BeanFactory provides the configuration framework and basic functionality, and the ApplicationContext adds more enterprise-specific functionality. The ApplicationContext is a complete superset of the BeanFactory and is used exclusively in this chapter in descriptions of Spring’s IoC container.

大概意思是说，BeanFactory提供了一个可配置的框架和基础的功能。而ApplicationContext在此基础上添加了跟多的企业级应用功能。ApplicationContext是BeanFactory的一个超集。

实际上，AbstractApplicationContext是利用组合（composition）、接口、委托（delegation）三个技术手段来实现BeanFactory的功能的。

```
例如
org.springframework.context.support.AbstractApplicationContext#getBean(java.lang.Class<T>)

@Override
public <T> T getBean(Class<T> requiredType) throws BeansException {
    assertBeanFactoryActive();
    return getBeanFactory().getBean(requiredType);
}
```

结论：
```
BeanFactory是Spring底层IOC容器

而ApplicationContext是具备应用特性的BeanFactory超集
```