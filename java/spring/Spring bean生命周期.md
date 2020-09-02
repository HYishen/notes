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
org.springframework.beans.factory.support.AbstractBeanFactory#doGetBean

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#createBean(java.lang.String, org.springframework.beans.factory.support.RootBeanDefinition, java.lang.Object[])

org.springframework.beans.factory.support.AbstractBeanFactory#resolveBeanClass

org.springframework.beans.factory.support.AbstractBeanFactory#doResolveBeanClass

org.springframework.beans.factory.support.AbstractBeanDefinition#resolveBeanClass
