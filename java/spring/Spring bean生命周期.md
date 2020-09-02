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
