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
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#createBean(java.lang.String, org.springframework.beans.factory.support.RootBeanDefinition, java.lang.Object[])

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#resolveBeforeInstantiation

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#applyBeanPostProcessorsBeforeInstantiation

org.springframework.beans.factory.config.InstantiationAwareBeanPostProcessor#postProcessBeforeInstantiation
```

### 实例化阶段
- 实例化方式
  - 传统实例化方式
    - 实例化策略 - InstantiationStrategy
  - 构造器依赖注入
    - 构造器注入按照类型注入，类型注入实际上是使用依赖注入来操作DefaultListableBeanFactory#resolveDependency

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
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#createBean(java.lang.String, org.springframework.beans.factory.support.RootBeanDefinition, java.lang.Object[])

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

### 初始化前阶段
- 已完成
  - Bean 实例化（实例化前、实例化、实例化后）
  - Bean 属性赋值
  - Bean Aware 接口回调

- 方法回调
  - BeanPostProcessor#postProcessBeforeInitialization

```
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#createBean(java.lang.String, org.springframework.beans.factory.support.RootBeanDefinition, java.lang.Object[])

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#doCreateBean

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#initializeBean(java.lang.String, java.lang.Object, org.springframework.beans.factory.support.RootBeanDefinition)

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#applyBeanPostProcessorsBeforeInitialization

org.springframework.beans.factory.config.BeanPostProcessor#postProcessBeforeInitialization
```

### 初始化阶段
- Bean 初始化（Initialization）
  - @PostConstruct 标注方法
  - 实现 InitializingBean 接口的 afterPropertiesSet() 方法
  - 自定义初始化方法

```
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#initializeBean(java.lang.String, java.lang.Object, org.springframework.beans.factory.support.RootBeanDefinition)

// 1.CommonAnnotationBeanPostProcessor对@PostConstruct 标注方法执行
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#applyBeanPostProcessorsBeforeInitialization

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#invokeInitMethods

// 2.afterPropertiesSet回调
org.springframework.beans.factory.InitializingBean#afterPropertiesSet

// 3.执行自定义的init-method
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#invokeCustomInitMethod
```

### 初始化后阶段
- BeanPostProcessor#postProcessAfterInitialization

```
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#initializeBean(java.lang.String, java.lang.Object, org.springframework.beans.factory.support.RootBeanDefinition)

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#applyBeanPostProcessorsAfterInitialization

org.springframework.beans.factory.config.BeanPostProcessor#postProcessAfterInitialization
```

### 初始化完成阶段
- 方法回调
  - Spring 4.1 +：SmartInitializingSingleton#afterSingletonsInstantiated
  - DefaultListableBeanFactory#preInstantiateSingletons
> SmartInitializingSingleton 通常在 Spring ApplicationContext 场景使用
> preInstantiateSingletons 将已注册的 BeanDefinition 初始化成 Spring Bean

### 销毁前阶段
- 方法回调
  - DestructionAwareBeanPostProcessor#postProcessBeforeDestruction

> 执行 Bean 销毁（容器内）
> Bean 销毁并不意味着 Bean 垃圾回收了

```
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#destroyBean

org.springframework.beans.factory.support.DisposableBeanAdapter#destroy

org.springframework.beans.factory.annotation.InitDestroyAnnotationBeanPostProcessor#postProcessBeforeDestruction
```

### 销毁阶段
- Bean 销毁（Destroy）
  - @PreDestroy 标注方法
  - 实现 DisposableBean 接口的 destroy() 方法
  - 自定义销毁方法

```
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#destroyBean

org.springframework.beans.factory.support.DisposableBeanAdapter#destroy

// 1.@PreDestroy 标注方法执行
org.springframework.beans.factory.annotation.InitDestroyAnnotationBeanPostProcessor#postProcessBeforeDestruction

// 2.实现 DisposableBean 接口的 destroy() 方法执行。((DisposableBean) this.bean).destroy();
org.springframework.beans.factory.DisposableBean#destroy

// 3.自定义销毁方法
org.springframework.beans.factory.support.DisposableBeanAdapter#invokeCustomDestroyMethod
```

### 垃圾收集
- Bean 垃圾回收（GC）
  - 关闭 Spring 容器（应用上下文）
  - 执行 GC
  - Spring Bean 覆盖的 finalize() 方法被回调
