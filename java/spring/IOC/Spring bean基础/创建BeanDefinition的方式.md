### BeanDefinition 构建
#### 通过 BeanDefinitionBuilder
```
// 1.通过 BeanDefinitionBuilder 构建
BeanDefinitionBuilder beanDefinitionBuilder = BeanDefinitionBuilder.genericBeanDefinition(User.class);
// 通过属性设置
beanDefinitionBuilder
        .addPropertyValue("id", 1)
        .addPropertyValue("name", "小马哥");
// 获取 BeanDefinition 实例
BeanDefinition beanDefinition = beanDefinitionBuilder.getBeanDefinition();
```

#### 通过 AbstractBeanDefinition 以及派生类
```
// 2. 通过 AbstractBeanDefinition 以及派生类
GenericBeanDefinition genericBeanDefinition = new GenericBeanDefinition();
// 设置 Bean 类型
genericBeanDefinition.setBeanClass(User.class);
// 通过 MutablePropertyValues 批量操作属性
MutablePropertyValues propertyValues = new MutablePropertyValues();
propertyValues
        .add("id", 1)
        .add("name", "小马哥");
// 通过 set MutablePropertyValues 批量操作属性
genericBeanDefinition.setPropertyValues(propertyValues);
```