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
src源码包为类路径的开始，所有src里面的东西都会被合并放到bin目录下  
java项目:/bin/  
web项目:/WEB-INF/classes/  
```java
//写一个person类，给这个类注册
class person{
    String lastname;
    int age;
    person(String lastname,int age)
    {
        this.lastname=lastname;
        this.age=age;
    }
    void setLastname(String lastname)
    {
        this.lastname=lastname;
    }
    String getLastname()
    {
        return this.lastname;
    }
}
```
```xml
<!--ioc.xml 配置文件-->
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
    <bean id="02" class="com.phk.person">
        <!--使用property标签给person对象的属性赋值：调用getter/setter-->
        <property name="last" value="Marry"></property>
        <property name="age" value="22"></property>
    </bean>
</beans>
```
3. 测试  
- 实验1：根据id从容器中获取对象
```java
public class Test
{
    @Test
    public void test01()
    {
    //ApplicationContext:代表ioc容器
    //ClassPathXmlApplicationContext:当前应用的xml配置文件
    //根据spring配置文件得到ioc容器对象，在类路径下寻找
    ApplicationContext ioc= new ClassPathXmlApplicationContext("ioc.xml");
    //在磁盘路径下寻找ioc.xml
    //ApplicationContext ioc= new FileSystemXmlApplicationContext("F://ioc.xml");
    //容器帮我们创建了对象并赋值，调用setter方法赋值
    Person bean=(Person) ioc.getBean("01");//id
    System.out.println(bean);
    }
}
```
> **注意**:  
    1. 容器中对象的创建在容器创建完成时就已经创建好了  
    2. 同一个组件在ioc容器中是单实例的，即获取两次也是同一份对象  
    3. 如果获取不存在的组件报异常：NoSuchBeanDefinitionException  
    4. ioc容器在创建组件对象的时候，会利用setter方法为javaBean的属性赋值,property标签的作用  
    5. javaBean的属性名是由setter方法来决定的  
    例如:setLastname，即属性名为lastname  
    所以所有的setter/getter都自动生产，不要改动

实验2：根据bean类型从容器获取对象
```java
@Test
public void test02(){
    //如果ioc容器中这个class的类型有多个bean，查找就会报错
    Person bean=ioc.getBean(Person.class);
    System.out.println(bean);
    //id+class查找不需要强制类型转换
    Person bean2=ioc.getBean("02",Person.class);
}
```
实验3：利用有参构造器来给属性赋值
```xml
<bean id="03" class="com.phk.person">
    <!--调用有参构造器来给属性赋值-->
    <!--可省略name属性，但要严格按照构造器参数的顺序来写-->
    <constructor-arg name="lastname" value="barry"></constructor-arg>
    <constructor-arg name="age" value="12"></constructor-arg>
</bean> 
```