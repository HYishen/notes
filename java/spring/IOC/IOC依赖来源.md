Spring IOC依赖来源不仅限于我们自定义的Bean

Spring IOC依赖来源主要有：

- 自定义 Bean

- 容器內建 Bean 对象
```
Spring容器内部帮我们初始化了一些bean，例如Environment

// 依赖来源三：容器內建 Bean
Environment environment = applicationContext.getBean(Environment.class);
```

- 容器內建依赖
```
有些对象，通过applicationContext.getBean，是获取不到的，但是，通过依赖注入却可以获取得到

如：
private BeanFactory beanFactory; // 內建非 Bean 对象（依赖）
```

- 外部单体对象
```
外部单体对象的生命周期并不由Spring容器进行管理，但是也可以被Spring IOC容器来托管。

注册外部单体对象接口：
org.springframework.beans.factory.config.SingletonBeanRegistry
```
