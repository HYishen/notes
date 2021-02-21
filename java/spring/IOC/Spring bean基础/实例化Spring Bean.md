### 实例化 Spring Bean
Bean 实例化（Instantiation）
- 常规方式
  - 通过构造器（配置元信息：XML、Java 注解和 Java API ）
  ```
    BeanDefinitionBuilder beanDefinitionBuilder = genericBeanDefinition(User.class);
    beanDefinitionBuilder
            .addPropertyValue("id", 1L)
            .addPropertyValue("name", "小马哥");
  ```
  - 通过静态工厂方法（配置元信息：XML 和 Java API ）
  - 通过 Bean 工厂方法（配置元信息：XML和 Java API ）
  - 通过 FactoryBean（配置元信息：XML、Java 注解和 Java API ）

```
<!-- 静态方法实例化 Bean，在User类里面创建一个createUser静态方法 -->
<bean id="user-by-static-method" class="org.geekbang.thinking.in.spring.ioc.overview.domain.User"
      factory-method="createUser"/>

<!-- 通过 Bean 工厂方法实例化 Bean -->
<bean id="user-by-instance-method" factory-bean="userFactory" factory-method="createUser"/>
<bean id="userFactory" class="org.geekbang.thinking.in.spring.bean.factory.DefaultUserFactory"/>

<!-- FactoryBean实例化 Bean，创建一个实现org.springframework.beans.factory.FactoryBean接口的类 -->
<bean id="user-by-factory-bean" class="org.geekbang.thinking.in.spring.bean.factory.UserFactoryBean" />
```

- 特殊方式
   - 通过 ServiceLoaderFactoryBean（配置元信息：XML、Java 注解和 Java API ）
   - 通过 AutowireCapableBeanFactory#createBean(java.lang.Class, int, boolean)
   - 通过 BeanDefinitionRegistry#registerBeanDefinition(String,BeanDefinition)
