### 注册 Spring Bean
BeanDefinition 注册有三种方式
1. XML 配置元信息
2. Java 注解配置元信息
3. Java API 配置元信息

#### XML 配置元信息
```
<bean name=”...” ... />
```

#### Java 注解配置元信息
```
@Bean
@Component
@Import

// 1. 通过 @Bean 方式定义
/**
* 通过 Java 注解的方式，定义了一个 Bean
*/
@Bean(name = {"user", "yishen-user"})
public User user() {
   User user = new User();
   user.setId(1L);
   user.setName("Yishen");
   return user;
}

// 2. 通过 @Component 方式
@Component // 定义当前类作为 Spring Bean（组件）
public class Config {
}

// 3. 通过 @Import 来进行导入
@Import(AnnotationBeanDefinitionDemo.Config.class)
public class AnnotationBeanDefinitionDemo {
}
```

#### Java API 配置元信息
命名方式:BeanDefinitionRegistry#registerBeanDefinition(String,BeanDefinition)

非命名方式:BeanDefinitionReaderUtils#registerWithGeneratedName(AbstractBeanDefinition,BeanDefinitionRegistry)

配置类方式:AnnotatedBeanDefinitionReader#register(Class...)
