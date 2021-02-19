### 什么是 BeanDefinition？
BeanDefinition 是 Spring Framework 中定义 Bean 的配置元信息接口，包含：
  - Bean 的类名
  - Bean 行为配置元素，如作用域、自动绑定的模式，生命周期回调等
  - 其他 Bean 引用，又可称作合作者（collaborators）或者依赖（dependencies）
  - 配置设置，比如 Bean 属性（Properties）
