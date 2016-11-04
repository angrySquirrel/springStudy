# Spring IoC 容器的设计

>容器将创建对象，把它们连接在一起，配置它们，并管理他们的整个生命周期(从创建到销毁)。

## IoC容器中的主要接口设计
![IoC容器接口的设计图](./ioc容器.png)

### 第一条设计主线
从接口BeanFactory到HierarchyBeanFactory再到ConfigurationBeanFactory，是主要的BeanFactory设计路径。

BeanFactory 提供了最基本的IoC容器功能
通过`&`可以获得FactoryBean本身，可以区分它产生产生的对象
FactoryBean 不是简单的Bean，而是一个能产生或修饰对象生成的工厂Bean
#### 简单的IoC容器
下面以XmlBeanFactory为例，它是一个可以读取以XML文件方式定义的BeanDefinition的IoC容器。
构造XmlBeanFactory这个容器时，需要指定BeanDefinition信息来源(封装成Spring中的Resource类)

``` java
<!-- 再作为参数传递给XmlBeanFactory构造函数
public class XmlBeanFactory extends DafaultListableBeanFactory{

private final XmlBeanDefinitionReader 
reader = new XmlBeanDefinitionReader(this);
public XmlBeanFactory(Resource resource) throws BeansException {
	this(resource,null);


} -->

// 创建IoC配置文件抽象资源
ClassPathResource res = new ClassPathResource('beans.xml');
// 创建一个BeanFactory
DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
// 创建一个载入BeanDefinition的读取器
XmlBeanDefinitionReader reader = 
new XmlBeanDefinitionReader(factory);
// 从定义好的资源位置读入配置信息
reader.loadBeanDefinition(res);
```


### 第二条设计主线
以ApplicationContext应用上下文接口为核心：
从BeanFactory到ListableBeanFactory 再到 ApplicationContext 再到 WebApplicationContext 或 ConfigurableApplicationContext

在ListableBeanFactory接口中，细化了许多BeanFactory的接口功能，比如定义了getBeanDefinitionNames()接口方法

ApplicationContext 通过继承MessageSoruce和ResourceLoader, ApplicationEventPublisher 接口，在BeanFactory 简单容器的基础上添加了许多对高级容器的特性的支持。


WebApplicationContext是在Web环境中使用的ApplicationContext


#### ApplicationContext容器



---
### reference
Spring技术内幕：深入解析Spring

