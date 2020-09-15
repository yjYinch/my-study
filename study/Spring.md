# Spring

Spring的核心包：spring-core（core包是最基础的模块）、spring-beans（依赖core）、spring-expression、spring-context。



## 一、IOC

ioc：控制反转

### 1.1 原理

1. **xml配置文件解析**
2. **dom4j解析**
3. **工厂设计模式**
4. **反射**



比如：

（1）创建类的.xml配置文件，配置要创建的对象类

```xml
<bean id="userService" class="com.zyj.service.UserService"/></bean>
```

（2）创建工厂类，使用dom4j解析配置文件+反射回对象

```java
public static UserService getService(){
    //1.使用dom4j解析xml文档
    //2.根据id为userService得到对应的class
    String classValue = "class属性值";
    //3.使用反射创建类对象
    Class clazz = Class.forName(classValue);
    //4.创建类对象
    UserService service = clazz.newInstance();
    //5.返回对象
    return service;
}
```



## 三、 AOP

面向切面编程，它是一种编程范式，目的：提高开发效率。AOP实现的业务场景主要有以下：

* **日志管理**
* **事务控制**
* **权限验证**

### 3.1  AOP术语

目标对象（原始方法缺少东西，即被抽取出来的对象，不能正常执行）、代理对象（将目标方法结合通知类能够使目标对象正常执行）

* **JoinPoint**

  **连接点**。所谓连接点是指那些可能被拦截到的方法。通俗的说，连接点就是类中的方法。

* **PointCut**

  **切入点**。将方法中共性的模块抽取出来，这些被抽取了公共功能的方法称之为切入点。例如：数据库连接的获取与关闭，事务的开启与提交。

* **Advice**

  **通知**。通知就是抽取的共性功能组成的方法然后执行。通知方法有位置区分。例如：`@Before`、`@After`、`@AfterReturning`、`@AfterThrowing`、`@Around`

* **Weaving**

  **织入**，通过AOP代理，完成了PointCut和Advice的结合，组成完整的代码逻辑，将通知加入到pointCut对应位置的动作称之为织入。

  织入是一个动态过程，不对应任何代码，可理解为动态的运行过程。

* **target**

  **目标对象**。即需要被代理的类。例如UserService

* **proxy**

  **代理类**。（1）代理目标对象，（2）然后将通知加入到代理对象的方法中。

* **Aspect**

  **切面**，是一个设计概念，是指切入点pointCut和通知advice之间绑定。
  
* **Introduction**（了解）

  **引入**，将共享变量引入到切入点方法中。

### 3.2  常见通知Advice

* 前置通知 

  在执行目标方法之前运行（@Before）

* 后置通知

  在执行目标方法之后通知，不管有没有一场（@After）

* 返回通知

  在目标方法正常返回值后运行（@AfterReturning）

* 异常通知

  在目标方法出现异常后运行（@AfterThrowing）

* 环绕通知

  动态代理，需要手动执行joinPoint.procced()（其实就是执行我们的目标方法之前相当于前置通知，执行之后就相当于我们后置通知）（@Around）

### 3.3 AOP的工作流程和运行流程

#### 3.3.1 AOP工作流程

1. **制作目标类（功能类）**
2. **制作通知类**
3. **配置切面**

开发时，将**功能类**中的通用方法提取出来，制作成**通知类**，原始目标对象中的方法（切入点）不再有通用功能，也就是说不能够独立的执行，它必须结合通知类和切入点才能完成该方法（这一过程称之为**切面**）。



#### 3.3.2 AOP运行流程

1. Spring监控配置文件中切入点对应方法执行
2. 对应方法被执行，立即生成切入点所在类对象的代理对象
3. 代理对象将通知和切入点方法融合并执行（该融合过程称之为织入）

Spring监控方法是否被执行，一旦被执行就生成代理对象（JDK动态代理）。



### 3.4 AOP底层原理

Spring AOP的底层都是通过代理来实现的

1. **JDK动态代理**：需要实现某个接口，(AOP默认使用JDK动态代理)
2. **cglib动态代理**：目标类的子类（不需要类实现任何接口，cglib包Spring core）

例如，一个UserService类如果不实现某个接口，则其不能使用JDK动态代理，但是可以使用cglib动态代理。



### 3.5 切面表达式
execution(修饰符 **返回值** 包名 **方法名(参数)** 方法抛出异常)。加粗的部分不能省略
例如：

```java
@Pointcut("execution(public * com.zyj.springaop.controller.*Controller.*(..))")
```