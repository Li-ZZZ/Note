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

**DI**：(Dependency Injection)依赖注入：  
容器能知道哪个组件(类)运行的时候，需要另一个类(组件)，容器通过**反射**的形式，将容器中准备好的对象注入(利用反射给属性赋值)  

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
3. 测试  
-  实验1：根据id从容器中获取对象
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

- 实验2：根据bean类型从容器获取对象
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
- 实验3：利用有参构造器来给属性赋值
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
- 实验4：正确的为各种属性赋值
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

- 实验5：通过继承实现bean配置信息的重用
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
- 实验6：测试bean的作用域，单实例和多实例bean