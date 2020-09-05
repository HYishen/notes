### 元信息配置阶段

### 元信息解析阶段

### 注册阶段
- BeanDefinition 注册接口
  - BeanDefinitionRegistry

org.springframework.beans.factory.support.DefaultListableBeanFactory#registerBeanDefinition

### 合并阶段
- BeanDefinition 合并
  - 父子 BeanDefinition 合并
    - 当前 BeanFactory 查找
    - 层次性 BeanFactory 查找

org.springframework.beans.factory.support.AbstractBeanFactory#getMergedBeanDefinition(java.lang.String)

### Class 加载阶段
```
org.springframework.beans.factory.support.AbstractBeanFactory#doGetBean

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#createBean(java.lang.String, org.springframework.beans.factory.support.RootBeanDefinition, java.lang.Object[])

org.springframework.beans.factory.support.AbstractBeanFactory#resolveBeanClass

org.springframework.beans.factory.support.AbstractBeanFactory#doResolveBeanClass

org.springframework.beans.factory.support.AbstractBeanDefinition#resolveBeanClass
```

### 实例化前阶段
- 非主流生命周期 - Bean 实例化前阶段
  - InstantiationAwareBeanPostProcessor#postProcessBeforeInstantiation

```
org.springframework.beans.factory.config.InstantiationAwareBeanPostProcessor#postProcessBeforeInstantiation

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#applyBeanPostProcessorsBeforeInstantiation

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#resolveBeforeInstantiation

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#createBean(java.lang.String, org.springframework.beans.factory.support.RootBeanDefinition, java.lang.Object[])
```

### 实例化阶段
- 实例化方式
  - 传统实例化方式
    - 实例化策略 - InstantiationStrategy
  - 构造器依赖注入
    - 构造器注入按照类型注入，类型注入实际上是使用依赖注入来操作DefaultListableBeanFactory#resolveDependencyresolveDependency

```
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#createBean(java.lang.String, org.springframework.beans.factory.support.RootBeanDefinition, java.lang.Object[])

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#doCreateBean

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#createBeanInstance

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#instantiateBean

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#getInstantiationStrategy

org.springframework.beans.factory.support.SimpleInstantiationStrategy#instantiate(org.springframework.beans.factory.support.RootBeanDefinition, java.lang.String, org.springframework.beans.factory.BeanFactory)
```

### 实例化后阶段
- Bean 属性赋值（Populate）判断
  - InstantiationAwareBeanPostProcessor#postProcessAfterInstantiation

如果postProcessAfterInstantiation方法返回false，postProcessAfterInstantiation方法中对bean进行的操作（如赋值）会生效，但是之后的属性赋值操作将会被跳过。

```
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#doCreateBean

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#populateBean

org.springframework.beans.factory.config.InstantiationAwareBeanPostProcessor#postProcessAfterInstantiation
```

### 属性赋值前阶段
- Bean 属性值元信息
  - PropertyValues

- Bean 属性赋值前回调
  - Spring 1.2 - 5.0：InstantiationAwareBeanPostProcessor#postProcessPropertyValues
  - Spring 5.1：InstantiationAwareBeanPostProcessor#postProcessProperties

```
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#doCreateBean

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#populateBean

org.springframework.beans.factory.config.InstantiationAwareBeanPostProcessor#postProcessProperties
```

### Aware 接口回调阶段
- Spring Aware 接口(按顺序回调)
  - BeanNameAware
  - BeanClassLoaderAware
  - BeanFactoryAware
  - EnvironmentAware
  - EmbeddedValueResolverAware
  - ResourceLoaderAware
  - ApplicationEventPublisherAware
  - MessageSourceAware
  - ApplicationContextAware

```
// BeanFactory ---------------------------------------------------------------
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#doCreateBean

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#initializeBean(java.lang.String, java.lang.Object, org.springframework.beans.factory.support.RootBeanDefinition)

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#invokeAwareMethods

// Application ---------------------------------------------------------------
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#initializeBean(java.lang.String, java.lang.Object, org.springframework.beans.factory.support.RootBeanDefinition)

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#applyBeanPostProcessorsBeforeInitialization

org.springframework.context.support.ApplicationContextAwareProcessor#postProcessBeforeInitialization

org.springframework.context.support.ApplicationContextAwareProcessor#invokeAwareInterfaces
```
