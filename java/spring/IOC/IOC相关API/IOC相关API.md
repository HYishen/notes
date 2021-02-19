### Bean定义读取器
```
org.springframework.beans.factory.support.BeanDefinitionReader

org.springframework.beans.factory.support.AbstractBeanDefinitionReader

org.springframework.beans.factory.xml.XmlBeanDefinitionReader
```

代码例子
```
// 创建 BeanFactory 容器
DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();
XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(beanFactory);
// XML 配置文件 ClassPath 路径
String location = "classpath:/META-INF/dependency-lookup-context.xml";
// 加载配置
int beanDefinitionsCount = reader.loadBeanDefinitions(location);
System.out.println("Bean 定义加载的数量：" + beanDefinitionsCount);

// 使用BeanFactory
if (beanFactory instanceof ListableBeanFactory) {
    ListableBeanFactory listableBeanFactory = (ListableBeanFactory) beanFactory;
    int beanDefinitionCount = listableBeanFactory.getBeanDefinitionCount();
    // 输出bean定义数量，输出值与reader.loadBeanDefinitions一致
    System.out.println(beanDefinitionCount);
}
```
