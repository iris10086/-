# spring体系结构

<img src="https://atts.w3cschool.cn/attachments/image/wk/wkspring/arch1.png" alt="Spring 体系结构" style="zoom:150%;" />

核心容器-->AOP...-->...

# 开发步骤

设计接口，写好实现类

设置配置文件

使用实现类时，通过spring框架的对象获取对象

（默认使用无参构造创建对象）

1. 导入坐标
2. 创建bean
3. 创建applicationContext.xml

# 配置文件详解

## bean

#### id

唯一值

### scope

作用范围

singleton 唯一的对象

prototype	多例对象

web:request，session，globalsession

### ini-method

初始化方法

### destroy-method

销毁方法

（applicationContext对象没有close方法，只有它的实现类有）

### bean的生命周期

当scope为singleton时：

spring核心文件被加载时创建。

当scope为prototype时：

当getBean方法调用时创建

## 实例化的三种方法

### 无参构造

默认方法，使用bean就行

### 静态工厂实例化

通过factory-method方法指定静态工厂方法

```xml
<bean id="userDao" class="com.wobuzhidao.factory.StaticFactory" factory-method="getUserDao"/>
```

### 工厂实例方法

先配置工厂实例，将工厂对象创建

在通过工厂对象调用方法创建对象



通过factory-bean指定工厂名

```xml
<bean id="factory" class="com.wobuzhidao.factory.DynamicFactory"/>
<bean id="userDao" factory-bean="factory" factory-method="getUserDao"/>
```

# 依赖注入

1. 通过属性注入
2. 通过有参构造注入

## 属性注入

通过property标签注入

```xml
<bean id="userService" class="com.wobuzhidao.service.Impl.UserServiceImpl" >
<property name="userDao" ref="userDao"/>
</bean>
```

ref：对象引用注入（注入对象）

value：普通参数注入

p命名空间注入

## 有参构造注入

使用constructor-arg标签

```xml
<bean id="userService" class="com.wobuzhidao.service.Impl.UserServiceImpl">
    <constructor-arg name="userDao" ref="userDao"/>
</bean>
```

## 注入list，map，set类型

使用property中添加list,map,set标签注入

```xml
<property name="list">
    <list>
        <value>aaa</value>
        <value>bbb</value>
        <value>ccc</value>
    </list>
</property>
<property name="map">
    <map>
        <entry key="hello" value="word"></entry>
        <entry key="hello1" value="word1"></entry>
        <entry key="hello2" value="word2"></entry>
    </map>
</property>
<property name="set">
    <set>
        <value>aaa</value>
        <value>aaa</value>
        <value>ccc</value>
    </set>
</property>
```

## 引入其他配置文件

```xml
<import resource="applicationContext-dao.xml"/>
```

# spring相关API

## applicationContext继承体系

### 实现类ClassPathXmlApplicationContext

从类的根目录加载

### 实现类FileSystemXmlApplicationContext

从磁盘路径加载

### 实现类AnnotationConfigApplicationContext

读取注解

## getbean

创建对象的两种方式：

1. id
2. class

class只有一个的话可以使用class获取，若有多个只能用id

# spring配置数据源

==>配置jar包中的第三方类到spring容器中

通过属性注入

## 抽取配置文件

需要context命名空间引入外部配置文件

再通过属性注入配置文件

```xml
<comtext:property-placeholder location="classpath:jdbc.properties"/> 引入配置文件
```

# 注解开发

提高开发速度

## 原始注解

主要是bean中的内容

![image-20220205224731012](C:\Users\iris\AppData\Roaming\Typora\typora-user-images\image-20220205224731012.png)

### 需要配置组件扫描

可以使用EL表达式

## spring新注解

![image-20220206140208312](C:\Users\iris\AppData\Roaming\Typora\typora-user-images\image-20220206140208312.png)

# 集成juint

导包spring-test与juint包

test类中添加@RunWith(SpringJUnit4ClassRunner.class)注解

指定配置文件

在test类中注入再测试就行

# web环境开发

web.xml中配置全局变量指向spring配置文件

```xml
<context-param>
    <param-name>springconfig</param-name>
    <param-value>applicationContext.xml</param-value>
</context-param>
```

监听器中通过servletContext获取配置参数

```java
ServletContext context = servletContextEvent.getServletContext();
String springconfig = context.getInitParameter("springconfig");
ApplicationContext app = new ClassPathXmlApplicationContext(springconfig);
```

## 配置

spring内部有监听器

spring集成web配置步骤：

1. 配置监听器

   ```xml
   <!--    spring加载监听器-->
   <listener>
       <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
   </listener>
   ```

   

2. 配置spring配置文件

   ```xml
   <!--    配置文件变量-->
   <context-param>
       <param-name>contextConfigLocation</param-name>
       <param-value>classpath:applicationContext.xml</param-value>
   </context-param>
   ```

   
