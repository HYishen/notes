### 初始化 Spring Bean
@PostConstruct 标注方法
```
public class DefaultUserFactory {
    // 1. 基于 @PostConstruct 注解
    @PostConstruct
    public void init() {
        System.out.println("@PostConstruct : UserFactory 初始化中...");
    }
}
```

实现 InitializingBean 接口的 afterPropertiesSet() 方法
```
public class DefaultUserFactory implements InitializingBean {
    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("InitializingBean#afterPropertiesSet() : UserFactory 初始化中...");
    }
}
```

自定义初始化方法
```
XML 配置：<bean init-method=”init” ... />
Java 注解：@Bean(initMethod=”init”)
Java API：AbstractBeanDefinition#setInitMethodName(String)
```

### 延迟初始化 Spring Bean
```
XML 配置：<bean lazy-init=”true” ... />
Java 注解：@Lazy(true)
```

### 销毁 Spring Bean
@PreDestroy 标注方法
```
public class DefaultUserFactory {
    @PreDestroy
    public void preDestroy() {
        System.out.println("@PreDestroy : UserFactory 销毁中...");
    }
}
```

实现 DisposableBean 接口的 destroy() 方法
```
public class DefaultUserFactory implements DisposableBean {
    @Override
    public void destroy() throws Exception {
        System.out.println("DisposableBean#destroy() : UserFactory 销毁中...");
    }
}
```

自定义销毁方法
```
XML 配置：<bean destroy=”destroy” ... /> 

Java 注解：@Bean(destroy=”destroy”)

Java API：AbstractBeanDefinition#setDestroyMethodName(String)
```

### 初始化及销毁的顺序
初始化及销毁，都是按照注解标注、接口方法、自定义方法的顺序来执行
