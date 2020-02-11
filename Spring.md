<link rel="stylesheet" type="text/css" href="mkcss.css">

# Spring 
主要有IOC和AOP

# IOC(Inversion Of Control)反转控制
从主动的new资源变成被动的接受资源  
**控制**：资源的获取方式 
- 主动式  
要什么资源自己创建即可  
简单对象的创建比较易用，但复杂对象的创建是个庞大的工程
- 被动式  
资源的获取不是自己创建，交给容器来创建和设置  

**容器**：管理所有的组件(有功能的类)，容器可以自动地探查出那些组件(类)需要用到另一些组件(类)

**DI**(Dependency Injection)依赖注入：  
容器能知道哪个组件(类)运行的时候，需要另一个类(组件)；容器通过**反射**的形式，将容器中准备好的对象注入(利用反射给属性赋值)  

> 只要容器管理的组件，都能使用容器提供的强大功能

# 给容器注册组件

框架编写流程：
1. 导包
```
核心容器
spring-beans-4.0.0.RELEASE.jar
spring-context-4.0.0.RELEASE.jar
spring-core-4.0.0.RELEASE.jar
spring-expression-4.0.0.RELEASE.jar
commons-logging-1.1.3.jar
```
2. 写配置
src目录下创建的Spring Bean Configuration(Spring配置文件)  
```java
//写一个person类，给这个类注册
class person{
    char lastname;
    int age;
}
```
```xml
<beans>
    <!--
    一个Bean标签可以注册一个组件(对象，类)
    class：写要注册的全类名
    id：这个对象唯一标识
    -->
    <bean id="01" class="com.phk.person">
        <!--使用property标签给person对象的属性赋值-->
        <property name="last" value="John"></property>
        <property name="age" value="18"></property>
    </bean>
</beans>
```
1. 测试  