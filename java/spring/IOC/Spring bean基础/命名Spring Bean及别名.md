### 命名 Spring Bean
#### Bean 的名称
每个 Bean 拥有一个或多个标识符（identifiers），这些标识符在 Bean 所在的容器（不是应用）必须是唯一的。通常，一个 Bean 仅有一个标识符，如果需要额外的，可考虑使用别名（Alias）来扩充。

在基于 XML 的配置元信息中，开发人员可用 id 或者 name 属性来规定 Bean 的 标识符。通常Bean 的 标识符由字母组成，允许出现特殊字符。如果要想引入 Bean 的别名的话，可在name 属性使用半角逗号（“,”）或分号（“;”) 来间隔。

Bean 的 id 或 name 属性并非必须制定，如果留空的话，容器会为 Bean 自动生成一个唯一的名称。Bean 的命名尽管没有限制，不过官方建议采用驼峰的方式，更符合 Java 的命名约定。

#### Bean 名称生成器（BeanNameGenerator）
Bean 名称生成器（BeanNameGenerator）由 Spring Framework 2.0.3 引入，框架內建两种实现：

DefaultBeanNameGenerator：默认通用 BeanNameGenerator 实现

AnnotationBeanNameGenerator：基于注解扫描的 BeanNameGenerator 实现，起始于Spring Framework 2.5

### Spring Bean 的别名
Bean 别名（Alias）的价值：
1. 复用现有的 BeanDefinition
2. 更具有场景化的命名方法，比如：
```
<alias name="myApp-dataSource" alias="subsystemA-dataSource"/>
<alias name="myApp-dataSource" alias="subsystemB-dataSource"/>
```
