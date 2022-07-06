Spring 提供了两种 IoC 容器，分别为 BeanFactory 和 ApplicationContext

BeanFactory 是基础类型的 IoC 容器，提供了完整的 IoC 服务支持。简单来说，BeanFactory 就是一个管理 Bean 的工厂，它主要负责初始化各种 Bean，并调用它们的生命周期方法。

ApplicationContext 是 BeanFactory 的子接口，也被称为应用上下文。它不仅提供了 BeanFactory 的所有功能，还添加了对 i18n（国际化）、资源访问、事件传播等方面的良好支持。

他俩的主要区别在于，如果 Bean 的某一个属性没有注入，则使用 BeanFacotry 加载后，在第一次调用 getBean() 方法时会抛出异常，但是呢ApplicationContext 会在初始化时自检，这样有利于检查所依赖的属性是否注入。

因此，在实际开发中，通常都选择使用 ApplicationContext