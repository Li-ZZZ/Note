<link rel="stylesheet" type="text/css" href="mkcss.css">

- [Spring IOC(Inversion Of Control)反转控制](#spring-iocinversion-of-control%e5%8f%8d%e8%bd%ac%e6%8e%a7%e5%88%b6)
- [给容器注册组件](#%e7%bb%99%e5%ae%b9%e5%99%a8%e6%b3%a8%e5%86%8c%e7%bb%84%e4%bb%b6)
  - [1.导包](#1%e5%af%bc%e5%8c%85)
  - [2. 写配置](#2-%e5%86%99%e9%85%8d%e7%bd%ae)
  - [3.实验](#3%e5%ae%9e%e9%aa%8c)
    - [实验1：根据id从容器中获取对象](#%e5%ae%9e%e9%aa%8c1%e6%a0%b9%e6%8d%aeid%e4%bb%8e%e5%ae%b9%e5%99%a8%e4%b8%ad%e8%8e%b7%e5%8f%96%e5%af%b9%e8%b1%a1)
    - [实验2：根据bean类型从容器获取对象](#%e5%ae%9e%e9%aa%8c2%e6%a0%b9%e6%8d%aebean%e7%b1%bb%e5%9e%8b%e4%bb%8e%e5%ae%b9%e5%99%a8%e8%8e%b7%e5%8f%96%e5%af%b9%e8%b1%a1)
    - [实验3：利用有参构造器来给属性赋值](#%e5%ae%9e%e9%aa%8c3%e5%88%a9%e7%94%a8%e6%9c%89%e5%8f%82%e6%9e%84%e9%80%a0%e5%99%a8%e6%9d%a5%e7%bb%99%e5%b1%9e%e6%80%a7%e8%b5%8b%e5%80%bc)
    - [实验4：正确的为各种属性赋值](#%e5%ae%9e%e9%aa%8c4%e6%ad%a3%e7%a1%ae%e7%9a%84%e4%b8%ba%e5%90%84%e7%a7%8d%e5%b1%9e%e6%80%a7%e8%b5%8b%e5%80%bc)
    - [实验5：通过继承实现bean配置信息的重用](#%e5%ae%9e%e9%aa%8c5%e9%80%9a%e8%bf%87%e7%bb%a7%e6%89%bf%e5%ae%9e%e7%8e%b0bean%e9%85%8d%e7%bd%ae%e4%bf%a1%e6%81%af%e7%9a%84%e9%87%8d%e7%94%a8)
    - [实验6：测试bean的作用域，单实例和多实例bean](#%e5%ae%9e%e9%aa%8c6%e6%b5%8b%e8%af%95bean%e7%9a%84%e4%bd%9c%e7%94%a8%e5%9f%9f%e5%8d%95%e5%ae%9e%e4%be%8b%e5%92%8c%e5%a4%9a%e5%ae%9e%e4%be%8bbean)
    - [实验7：配置配置静态工厂方法创建bean、实例工厂方法创建的bean、FactoryBean ※](#%e5%ae%9e%e9%aa%8c7%e9%85%8d%e7%bd%ae%e9%85%8d%e7%bd%ae%e9%9d%99%e6%80%81%e5%b7%a5%e5%8e%82%e6%96%b9%e6%b3%95%e5%88%9b%e5%bb%babean%e5%ae%9e%e4%be%8b%e5%b7%a5%e5%8e%82%e6%96%b9%e6%b3%95%e5%88%9b%e5%bb%ba%e7%9a%84beanfactorybean-%e2%80%bb)
    - [实验8：创建带有生命周期方法的bean](#%e5%ae%9e%e9%aa%8c8%e5%88%9b%e5%bb%ba%e5%b8%a6%e6%9c%89%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f%e6%96%b9%e6%b3%95%e7%9a%84bean)
    - [实验9：后置处理器](#%e5%ae%9e%e9%aa%8c9%e5%90%8e%e7%bd%ae%e5%a4%84%e7%90%86%e5%99%a8)
    - [实验10：xml的自动装配](#%e5%ae%9e%e9%aa%8c10xml%e7%9a%84%e8%87%aa%e5%8a%a8%e8%a3%85%e9%85%8d)
    - [实验11：SqEL测试 (Spring Expression Language)](#%e5%ae%9e%e9%aa%8c11sqel%e6%b5%8b%e8%af%95-spring-expression-language)
    - [实验12:通过注解分别创建Dao、service、Controller](#%e5%ae%9e%e9%aa%8c12%e9%80%9a%e8%bf%87%e6%b3%a8%e8%a7%a3%e5%88%86%e5%88%ab%e5%88%9b%e5%bb%badaoservicecontroller)
    - [实验13:指定扫描包时包括或不包括的类](#%e5%ae%9e%e9%aa%8c13%e6%8c%87%e5%ae%9a%e6%89%ab%e6%8f%8f%e5%8c%85%e6%97%b6%e5%8c%85%e6%8b%ac%e6%88%96%e4%b8%8d%e5%8c%85%e6%8b%ac%e7%9a%84%e7%b1%bb)
    - [实验14 使用@Autowired注解自动赋值](#%e5%ae%9e%e9%aa%8c14-%e4%bd%bf%e7%94%a8autowired%e6%b3%a8%e8%a7%a3%e8%87%aa%e5%8a%a8%e8%b5%8b%e5%80%bc)
    - [实验15 使用spring的单元测试](#%e5%ae%9e%e9%aa%8c15-%e4%bd%bf%e7%94%a8spring%e7%9a%84%e5%8d%95%e5%85%83%e6%b5%8b%e8%af%95)
    - [实验16 泛型依赖注入](#%e5%ae%9e%e9%aa%8c16-%e6%b3%9b%e5%9e%8b%e4%be%9d%e8%b5%96%e6%b3%a8%e5%85%a5)


# Spring IOC(Inversion Of Control)反转控制
从主动的new资源变成被动的接受资源  
**控制**：资源的获取方式 
- 主动式  
要什么资源自己创建即可  
简单对象的创建比较易用，但复杂对象的创建是个庞大的工程
- 被动式  
资源的获取不是自己创建，交给容器来创建和设置  

**容器**：管理所有的组件(有功能的类)，容器可以自动地探查出那些组件(类)需要用到另一些组件(类)

**DI**：(Dependency Injection)依赖注入：  
容器能知道哪个组件(类)运行的时候，需要另一个类(组件)，容器通过**反射**的形式，将容器中准备好的对象注入(利用反射给属性赋值)  

> 只要容器管理的组件，都能使用容器提供的强大功能

# 给容器注册组件

框架编写流程：
## 1.导包
核心容器
>spring-beans-4.0.0.RELEASE.jar  
spring-context-4.0.0.RELEASE.jar  
spring-core-4.0.0.RELEASE.jar  
spring-expression-4.0.0.RELEASE.jar  
commons-logging-1.1.3.jar  

## 2. 写配置
src目录下创建的Spring Bean Configuration(Spring配置文件)  
src源码包为类路径的开始，所有src里面的东西都会被合并放到bin目录下  
java项目:/bin/  
web项目:/WEB-INF/classes/  
```java
//写一个Car类 放在person中
class Car{
    private String carname;
    private String color;
}
//写一个book类 放在person中的list
class Book{
    private String bookName;
    private String author;
}
//写一个person类，给这个类注册
class person{
    //基本类型直接使用<property>来赋值，自动进行类型转换
    private String lastname;
    private int age;
    //
    private Car car;
    private List<Book> books;
    private Map<String,Object>map;
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
## 3.实验  
### 实验1：根据id从容器中获取对象
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

### 实验2：根据bean类型从容器获取对象
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
### 实验3：利用有参构造器来给属性赋值
```xml
<bean id="03" class="com.phk.person">
    <!--调用有参构造器来给属性赋值-->
    <!--可省略name属性，但要严格按照构造器参数的顺序来写-->
    <!-- index="0",为参数指定索引，从0开始-->
    <!-- 构造器重载的情况下type可以指定参数的类型-->
    <constructor-arg name="lastname" value="barry" index="0"></constructor-arg>
    <constructor-arg name="age" value="12" type="java.lang.Integer" index="1"></constructor-arg>
</bean> 
<!--通过p名称空间为bean赋值-->
<!--名称空间：在xml中名称空间用来防止标签重复-->
<!--1.导入p命名空间 2.写带前缀的标签
    Example:两个name标签的区分
    <book>
        <b:name>西游记</b:name>
        <author>
            <a:name>吴承恩</a:name>
            <gender>男</gender>
        </author>
    </book>
    <bean id="06" class="com.phk.person" p:age="18"></bean>
-->
```
### 实验4：正确的为各种属性赋值
```xml
<bean id="car01" class="com.phk.car">
    <property name="carname" value="宝马">
    <property name="color" value="黑色">
</bean>

<bean id="01" class="com.phk.person">
    <property name="lastname">
        <!--进行复杂的赋值,赋值为null-->
        <null/>
    </property>

    <property name="car" ref="car01">
    <!--ref:代表引用外面的一个值,填id-->
    <!-- 相当于: car=ioc.getBean("car01");  -->
    </property>

    <property name="car" >
    <!-- 用bean标签创建相当于: car=new Car(); 内部bean不能被获取 只能内部使用 -->
        <bean class="com.phk.car">
            <property name="carName" value="Yellow"></property>
        </bean>
    </property>
</bean>

<bean id="02" class="com.phk.person">
    <property name="books">
         <!--相当于:books=new ArrayList<Book>();-->
        <list>
            <!--给list标签添加每一个元素-->
            <bean class="com.phk.Book" p:bookName="西游记"></bean>
            <!--引用外部的一个元素-->
            <ref bean="book01"/>
        </list>
    </property>

    <!--Map<String,Object>maps;-->
    <property name="maps">
        <!--相当于:maps=new LinkedHashMap<>();-->
        <map>
            <!--一个entry表示一个键值对 -->
            <entry key="key01" value="hello"></entry>
            <entry key="key02" value="19"></entry>
            <entry key="key03" value-ref="book01"></entry>
            <entry key="key04">
                <bean ...></bean>
            </entry>
            <entry key="key05">
                <map></map>
            </entry>
        </map>
    </property>
</bean>

<!-- util名称空间创建集合类型的bean，方便别人引用 -->
<!-- 先导入util名词空间 -->
<!--相当于：new LinkedHashMap,必须要有id,list同理-->
<util:map id="map01">
    <entry ...></entry>
</util:map>

<bean id="01" class="com.phk.person">
    <!--引用已经设置好的map-->
    <property name="maps" ref="map01">
    </property>
</bean>

<!-- 级联属性赋值：级联属性：属性的属性-->
<bean id="person04" class="com.phk.person">
    <!--为car赋值时改变car的价格-->
    <property name="car" ref="car01"></property>
    <!--级联属性可以修改属性的属性，但注意原来的bean的值可能被修改，因为是ref引用地址 -->
    <property name="car.price" val="900000"></property>
</bean>
```

### 实验5：通过继承实现bean配置信息的重用
```xml
<!--abstract="true" ：这个bean的配置是抽象的，不能获取实例，只能用来被别人继承-->
<bean id="person01" class="com.phk.person" abstract="true">
    <property name="lastname" value="herry"></property>
    <property name="age" value="16"></property>
    <property name="email" value="phk.com"></property>
</bean>
<!-- parent:指定当前的bean的配置信息继承于哪个-->
<bean id="person02" class="com.phk.person" parent="person01" >
    <property name="lastname" value="jerry"></property>
</bean>

```
### 实验6：测试bean的作用域，单实例和多实例bean
```xml
<!--
    bean的作用域：指定bean是否为单实例，默认：单实例
    使用scope标签
    prototype：多实例的
        1、容器启动默认不会去创建多实例bean
        2、获取的时候创建这个bean
        3、每次获取都会创建一个新的对象
    singleton：单实例
        1、在容器启动之前已经创建好对象，保存在容器中了
        2、任何时候获取都是获取之前创建好的那个对象
    request：在web环境下，同一次请求创建一个bean实例(没用)
    Session：在web环境下，同一次会话创建一个bean实例(没用)
-->
<bean id="Book" class="com.phk.Book" scope="prototype" ></bean>
```
### 实验7：配置配置静态工厂方法创建bean、实例工厂方法创建的bean、FactoryBean ※
飞机类
```java
class AirPlane{
    String captain;
    String personNumber;
}
```
bean的创建默认是框架利用**反射机制**new出来的实例  
工厂模式：工厂帮我们创建对象,有一个专门创建对象的**类**，就是工厂类  
```java
import AirPlane;
AirPlane ap=AirPlaneFactory.getAirPlane(String name);
```
静态工厂：工厂本身不需要创建对象，通过调用类的**静态方法** 对象=工厂类.get();  
动态工程：工厂本身**需要创建对象**  
    工厂类 工厂对象=new 工厂类();  
    工厂对象.get();  

静态工厂
```java
import com.phk.AirPlane;
class AirPlaneStaticFactory{
    //静态方法
    public static AirPlane getAirPlane(String name)
    {
        AirPlane airplane=new Airplane();
        airplane.setCaptain(name);
        return airplane;
    }
}
```
实例工厂
```java
import com.phk.AirPlane;
class AirPlaneStaticFactory{
    //普通方法
    public AirPlane getAirPlane(String name)
    {
        AirPlane airplane=new Airplane();
        airplane.setCaptain(name);
        return airplane;
    }
}
```
```xml
<!--静态工厂  factory-method指定工厂方法-->
<bean id="plane01" class="com.phk.AirPlaneStaticFactory" factory-method="getAirPlane" >
    <!--静态方法传参数-->
    <constructor-arg  name="name" value="Jerry"></constructor-arg>
</bean>

<!--实例工厂-->
<bean id="planefactory" class="com.phk.AirPlaneStaticFactory"></bean>

<!--
    factory-bean:指定工厂的id
    factory-method:指定工厂方法
-->
<bean id="plane02" class="com.phk.AirPlane" factory-bean="planefactory" factory-method="getAirPlane">
    <constructor-arg name="name" value="Harry"></constructor-arg>
</bean>
```
FactoryBean接口  
实现了**FactoryBean**接口的类是Spring可以认识的工厂类  
Spring会自动调用工厂方法创建实例  
1. 编写一个FactoryBean的实现类  

```java
public class MyFactoryBean implements FactoryBean<Book>{
    /*
    getObject：工厂方法
    返回创建对象
    */
    public Book getObject()
    {
        Book book=new book();
        book.setName("西游记");
        return book;
    }
    /*
    返回创建对象的类型
    Spring会自动调用这个方法来确认创建的对象是什么类型
    */
    public Class<?> getObjectType(){
        return Book.class;
    }
    /*
    isSingleton:是单例吗？
    */
    public boolean isSingleton()
    {
        return false;
    }
}
```
2. 在spring配置文件中进行注册  
```xml
<!--注册实现类-->
<!--
    这个bean就会返回创建好相应的对象 Book对象
    1、ioc容器启动的时候不会创建实例
    2、FactoryBean：获取的时候才创建对象
-->
<bean id="MyFactory" class="com.phk.MyFactoryBean"></bean>
```
### 实验8：创建带有生命周期方法的bean
生命周期：bean的创建和销毁  
自定义一些生命周期方法，spring在创建或销毁就会调用生命周期方法
```xml
<!--
    destroy-method：指定类中的销毁方法
    init-method：指定类中的初始化方法
    两个方法都不能带参数
    多实例的bean不会调用销毁方法
-->
<bean id="book01" class="com.phk.Book" destroy-method="destroy" init-method="init"></bean>
```
### 实验9：后置处理器
MyBeanPostProcessor实现**BeanPostProcessor**接口  
会在bean创建对象的前后调用两个方法
```java
public class MyBeanPostProcessor implements BeanPostProcessor
{
    /*
    初始化前调用
    */
    public Object postProcessBeforeInitialization(Object bean,Stirng name)
    {
        sysout("before执行");
        return bean;
    }
    /*
    初始化后调用
    */
    public Object postProcessAfterInitialization(Object bean,Stirng name)
    {
        sysout("After执行");
        return bean;
    }
}
```
注册后置处理器
```xml
<!--注册后置处理器-->
<bean id="beanPostProcessor" class="com.phk.MyBeanPostProcess"></bean>
<!--
    创建bean
    before->init->after
    即使bean没有设置init方法，后置处理器也会照样工作
-->
<bean id="car01" class="com.phk.car" ></bean>
```
### 实验10：xml的自动装配
自动装配(自动赋值 仅限于**自定义**类型)
```xml
<bean id="car" class="com.phk.car">
    <property name="color" value="白色"></property>
</bean>
<!--
    autowire="default/no":不自动装配
    autowire="byName":按照名字
        private Car car; 以属性名作为id在容器中找到一个组件给他赋值
        car = ioc.getBean("car");
    autowire="byType":按照属性的类型，如果容器中有多个该类型的对象，会异常
        car = ioc.getBean(Car.class);
    autowire="constructor": 按照有参构造器
        public Person(Car car)
            按照构造器赋值：
            1、先按照构造器参数的类型进行装配，没有就null
            2、如果找到多个对象，则按参数的名作为id进行匹配，找不到就null

    List<Book> books 容器会把所有的book装配到list中
-->
<bean id="person01" class="com.phk.person" autowire="byType">
</bean>
```
### 实验11：SqEL测试 (Spring Expression Language)
#{} 括号中的为EL
```xml
<bean id="person04" class="com.phk.person">
    <property name="salary" value="#{123.456*12}"></property>
    <!--引用某个bean的属性值-->
    <property name="lastname" value="#{book01.bookname}"></property>
    <!--引用其他bean 相当于ref-->
    <property name="car" value="#{car}">
    <!--
        调用静态方法：UUID.randomUUID().toString()
        #{T(全类名).静态方法名(arg0,arg1)}
    -->
    <property name="email" value="#{T(java.util.UUID).randomUUID().toString().subString(0,1)}"></property>
    <!--调用非静态方法 对象名.方法名-->
    <property name="email" value="#{book01.getBookName()}"></property>
</bean>
```
### 实验12:通过注解分别创建Dao、service、Controller
Spring有4个注解：
某个类上添加任何一个注解都能快速的将这个组件加入到ioc容器管理中  
- @Controller 控制器：给控制层(servlet包下的)组件添加注解  
- @Service 业务逻辑：给业务逻辑层的组件添加注解 bookservice  
- @Repository 给数据层(持久化层,dao层)的组件添加注解  
- @Component 给不属于以上几层的组件添加注解  
注解可以随便加，spring底层不会去验证这个组件是否是对应层  
但推荐给各自层加各自的注解  

步骤:
- 给组件添加注解
- 告诉spring自动扫描注解，依赖context名称空间
- 一定要导入aop包,支持加注解模式
```xml
<!--
    context：component-scan：自动组件扫描
    base-package：指定扫描的基础包，把包下所有加了注解的类扫描进ioc容器
-->
<context:component-scan base-package="com.phk"></context:component-scan>
```
```java
@Service
class BookServlet{

}

@Test
public void test(){
    Object o1=ioc.getBean("bookServlet");
    Object o2=ioc.getBean("bookServlet");
    sysout(o1==o2)// true
    
}
/*
使用注解加入容器的组件和使用配置加入的组件默认行为都是一样的  
1. 组件id，默认就是组件的类名首字母小写  
2. 组件的作用域默认是单例的  
*/

@Service("book") //给id改名字改成book
@Scope(value="prototype") //给组件设置为多实例
class BookServlet{

}
@Test
public void test(){
    Object o1=ioc.getBean("book");
    Object o2=ioc.getBean("book");
    sysout(o1==o2)// false
}
```
### 实验13:指定扫描包时包括或不包括的类
```xml
<context:component-scan base-package="com.phk">
    <!--
        exclude=-filter 排除过滤器
        type="annotation" ：指定排除规则，表示按注解排除
        expression="" ：注解的全类名

        type="assignable" : 按照具体的类进行排除
        expression="" ：全类名
    -->
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller">
    </contextl:exclude-filter>
</context:component-scan>

<!--use-default-filters 默认全扫描，设置为false 全部不扫描-->
<context:component-scan base-package="com.phk" use-default-filters="false">
    <!--include 使用方法与exclude相同-->
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller">
    </contextl:include-filter>
</context:component-scan>
```
### 实验14 使用@Autowired注解自动赋值
```xml
<!--先把注解的类扫描进容器-->
<context:component-scan base-package="com.phk"></context:component-scan>
```
```java
@Service
class A{

}
@Component
class B{
    @Qualifier("abc")//指定id去ioc寻找组件
    @Autowired //使用这个注解 自动在容器中给寻找A的对象 给该变量赋值
    private A a;
}
```
@Autowired原理  
1. 先按类型去容器中找对应组件：a=ioc.getBean(A.class);
   1. 找到一个就赋值
   2. 没找到抛出异常
   3. 找到多个（父类、子类）
      1. 按照变量名作为id继续匹配
         1. 变量名没有匹配成功过报错
2. Autowired一定会装配上，否则报错，不会NULL
3. Autowired(required=false)，该属性表示如果装配失败就赋值NULL

@Qualifier：指定id去ioc容器中寻找组件，不使用变量名
```java
//给方法中的参数自动装配
//该方法在容器启动的时候自动运行
@Autowired 
public void haha(B b,@Qualifiter("a")A aaa) //Qualifiter指定id去容器寻找
{
    sysout(b+"-----"+a);
}
```
@Autowired、@Resource、@Inject：都是自动装配的注解  
@Autowired:功能最强大，spring自己的注解  
@Resource:j2ee的注解，java的标准  
区别：@Resource扩展性更强，如果切换其他容器框架@Resource还可以使用，@Autowired不行

### 实验15 使用spring的单元测试
1. 导包：spring单元测试包spring-test-4.0.0.0.RELEASE.jar
2. @ContextConfiguration(locations="")指定spring的配置文件
3. @RunWith指定用哪种驱动进行单元测试，默认junit
   1. @RunWith(SpringJunit4ClassRunner.class)
   使用spring的单元测试模块来执行@test的测试方法
4. 不需要ioc.getBeans()获取对象
```java
@ContextConfiguration(locations="classpath:applicationContext.xml")
@RunWith(SpringJunit4ClassRunner.class)
public class IOCTest{
    @Autowired
    A a;
    @Test
    public void test01{
        sysout(a);
    }
}
```

### 实验16 泛型依赖注入
```java
public class BaseService<T>{
    @Autowired
    private BaseDao<T> baseDao;
    public save(){
        sysout(baseDao);
    }
}
@Service
public class BookService extends BaseService<Book>
{
/*
继承了BaseService 相当于拥有一个成员变量：
@Autowired  可以使用自动装配，寻找BaseDao<Book>类型的组件
BaseDao<Book> baseDao 虽然是private，但子类一样拥有，只是不能访问
*/
}

public abstract class BaseDao<T>{
    public abstract void save();
}
@Repository
public class BookDao extends BaseDao<Book>{
    //该组件类型就是BaseDao<Book>
}
```
IOC结束 2020.3.20