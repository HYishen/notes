### 什么是Servlet
Servlet 是基于 Java 技术的 web 组件

### 容器与组件
- 组件是什么?
符合规范，实现特定功能，并且可以部署在容器上的软件模块。
- 容器是什么？
符合规范，为组件提供运行环境，并且管理组件的生命周期(将组件实例化，调用其方法、 销毁组件的过程)的软件程序。

- 采用容器与组件这种编程模型的优势：
容器负责大量的基础服务(包括浏览器与服务器之间的网络通讯、多线程、参数传递等等)。而组件只需要处理业务逡辑。另外，组件的运行不依赖于特定的容器。

```
Servlet是组件 Tomcat是容器
```