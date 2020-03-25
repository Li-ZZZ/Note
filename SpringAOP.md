<link rel="stylesheet" type="text/css" href="mkcss.css">

# Spring AOP
AOP(Aspect Oriented Programming) 面向切面程序设计   
OOP(Object Oriented Programming) 面向对象程序设计

面向切面程序设计：基于OOP基础上的编程思想  
指在**程序运行期间**，将某段代码**动态的切入**到**指定方法**的**指定位置**进行运行的编程方式

场景：计算器运行计算方法时进行日志记录  
加日志记录：
1. 直接写在方法内部：不推荐、维护修改麻烦  
   日志记录：系统辅助功能  
   业务逻辑：核心功能  
   **耦合**  
2. 我们希望：  
   业务逻辑（核心功能）  
   日志模块：在核心功能运行期间，自己动态加入

```java
计算器类
class Calculator{
    public int add(int i,int j)//加法
    {
        int result=i+j;
        return result;
    }
    //减法...
    //乘法...
    // 除法...
}
```
## jdk的动态代理：  
缺点：难写复杂，而且被代理的类必须要实现接口

## Spring的AOP：
底层就是动态代理  
实现简单，没有强制要求目标对象必须实现接口

# AOP专业术语
- 切面类；包含输出日志记录方法的类
- 通知方法：输出日志记录的方法
- 横切关注点
- 连接点：每一个方法的每一个位置都是一个连接点
- 切入点：我们真正要执行日志记录的地方
- 切入点表达式：在众多连接点中选出什么我们感兴趣的地方
  
# AOP的简单配置
如何将切面类中的通知方法动态的在目标方法的各个位置切入
步骤：
## 导包  
基础包：  
- commons-logging-1.1.3.jar  
- spring-aop-4.0.0.RELEASSE.jar  
- spring-context-4.0.0.RELEASE.jar  
- spring-core-4.0.0.RELEASE.jar  
- spring-expression-4.0.0.RELEASE.jar  

面向切面包 基础版  
- sping-aspects-4.0.0.RELEASE.jar：  

加强版(即使目标对象没有实现接口也可以创建动态代理)  
- com.springsource.net.sf.cglib.2.2.0.jar
- com.springsource.org.aopalliance-1.0.0.jar
- com.springsource.org.aspectj.weaver-1.1.0.jar
## 写配置
1. 先把目标类和切面类（封装了日志方法）加入到ioc容器中
2. 告诉Spring哪个是切面类：@Aspect
3. 告诉Spring切面类中的通知方法在何时何地运行
4. 开始基于注解的AOP模式
```java
@Aspect //标记切面类
@Component
class LogUtils{
    /*
    5个通知注解
    @Before:在目标方法之前运行       前置通知
    @After：在目标方法结束后运行     后置通知
    @AfterReturning：在目标方法正常返回之后运行 返回通知
    @AfterThrowing：在目标方法抛出异常之后运行  异常通知
    @Around：环绕                   环绕通知
    */
    //写切入点表达式
    //execution（访问权限符 返回值类型 方法签名）
    @Befour("execution(public int com.phk.Calculator.add(int,int))")
    public static void logStart{
        Sysout("[xxx]方法开始执行"); 
    }
    //*表示所有方法
    @After("execution(public int com.phk.Calculator.*(int,int))")
    public static void logEnd{
        Sysout("[xxx]方法执行结束");
    }

}
@Service
interface Calculator{
    public int add(int i,int j);
}
class MyCalculator implements Calculator{
     public int add(int i,int j)//加法
    {
        int result=i+j;
        return result;
    }
}
```
```xml
<!--扫描包-->
<context:component-scan base-package="com.phk"></context:component-scan>
<!--开启基于注解的AOP模式：导入AOP命名空间-->
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```
测试
```java
@Test
public void test()
{
    //从ioc容器拿对象，如果要用类型获取，一定用他的借口类型，不能用本类
    Calculator bean=ioc.getBean(Calulator.class);
    bean.add(2,1);
    Sysout(bean); //com.phk.MyCalculator
    Sysout(bean.getClass());//com.sun.proxy.$Proxy12
    //Aop的底层原理就是动态代理，容器中保存的组件是它的代理对象，所以不能拿本类去获取对象
    方法2://用id获取
    Calculator bean2=(Calculator) ioc.getBean("myCalculator");
    
    //如果没有实现接口，cglib帮我们创建好代理对象，利用内部内
    MyCalculator bean=ioc.getBean(MyCalculator.class);
    //com.phk.MyCalCulator$EnhancerByCGLIB$$
    Sysout(bean.getClass());
}
```

### 细节二
切入点表达式写法：  
固定格式：execution(访问权限符 返回值类型 方法全类名(参数表))  
通配符：  
```
*:
1.匹配一个或多个字符
2.不能表示任意访问权限，留空就是表示任意权限

匹配所有方法
execution(public int com.phk.Calculator.*(int,int)) 
匹配int 和一个任意类型的参数
execution(public int com.*.Calculator.*(int,*))

..:
1.匹配任意多个参数
2.匹配任意层路径

参数为任意个任意类型
execution(public int com.*.Calculator.*(..)) 
路径为com下任意层路径的Calculator类的任意方法
execution(public int com..Calculator.*(int,int))
最模糊写法
execution(* *.*(..))

&&：与
两个表达式成立时匹配
execution(public int com.phk.Calculator.*(int,int)) &&execution(public int com.*.Calculator.*(int,*))

||：或
任意一个表达式成立时匹配

！：非
取反
！execution(public int com.*.Calculator.*(int,*))
```
### 细节三
通知方法的执行顺序：  
正常执行：@Before(前置)->@After(后置)->@AfterRetur  ning(正常返回)  
异常执行：@Before(前置)->@After(后置)->@AfterThrowing(异常返回)  
### 细节四
```java
@Befour("execution(public int com.phk.Calculator.add(int,int))")
//获得目标方法的运行时参数
public static void logStart(JoinPoint joinPoint){
    //获取到目标方法运行时使用的参数
    Object[] args=joinPoint.getArgs();
    //获取到方法签名
    Signature signature=joinPoint.getSignature();
    Stirng name =signature.getName();//获取方法名
    Sysout("["+name+"]方法开始执行,参数为"+Arrays.asList(args)); 
}

//获取返回值，或者异常
//返回值
@Befour(value="execution(public int com.phk.Calculator.add(int,int))",returning="result")
public static void logStart(JoinPoint joinPoint,Object result){
        Sysout("结果="+result);
}
//异常
@Befour(value="execution(public int com.phk.Calculator.add(int,int))",throwing="exception")
public static void logStart(JoinPoint joinPoint,Exception exception){
        Sysout("结果="+result);
}

接受的返回值和异常类型往大的写
如果是int返回值 写double会接收不到 所以写Object
```
### 细节五
```java
抽取可重用的切入点表达式
1.随便声明一个没有实现的返回值void的方法
2.给方法标注@Pointcut注解
@Pointcut("execution(public int com.phk.Calculator.add(int,int))")
public void haha(){};

//方法名引用
@Befour("haha()")
public static void logStart{
        Sysout("[xxx]方法开始执行"); 
}
```