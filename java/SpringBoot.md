# Hello

1. 添加项目的父项目(依赖版本管理)

   ```xml
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>2.3.4.RELEASE</version>
   </parent>
   ```

2. 写MainApplication.class启动类

   必须放在项目坐标目录下

   ![image-20220320223019919](C:\Users\iris\AppData\Roaming\Typora\typora-user-images\image-20220320223019919.png)

   ```java
   /**
    * 添加注释，标注是springboot工程
    */
   @SpringBootApplication
   public class MainApplication {
       public static void main(String[] args) {
           //  启动springboot工程
           SpringApplication.run(MainApplication.class,args);
       }
   }
   ```

   

3. 编写业务代码

# 配置

版本管理通过父类继承进行版本控制

## 修改版本号

1. 查看父类控制版本的properties
2. 在子类重写

```xml
<properties>
	<mysql.version>8.0.27</mysql.version>
</properties>
```

### 场景

spring-boot-starter-* springboot官方的某种场景

*-spring-boot-start  表示第三方的starter

自动引入该场景对应的所有依赖



所有场景启动器最底层的依赖都会依赖spring-boot-starter

## 自动配置

* 自动配置tomcat
	* 引入tomcat依赖
	* 配置tomcat
* 自动配置springMVC
	* 引入springMVC依赖
	* 自动配置springMVC常用配置

# 底层注解

## 指定配置类@Configuration注解

在类上声明该类与ApplicationContext.xml作用相同。

使用@Bean注解在方法上表示该方法返回一个Bean

### proxyBeanMethods属性

默认为true，称为full模式

false时，称为lite模式

作用：为了保障Bean的单实例。

​			对Bean的方法进行代理，调用配置对象的组件定义方法，也只能获取IOC容器中的实例对象。

原理：

​			在调用方法时,Spring会检查IOC容器中是否有该方法的返回值存在。
​			若为False,则不会检查。
​			属性为False，spring启动会变快。


解决组件依赖。在配置类中调用时，也会遵循以上原则。并且实现组件依赖。

若该属性为True,在配置类中调用时也会执行代理方法，从容器中获取对象。

若为False,则在执行调用时


### @Bean注解

方法名字为ID，返回相应的对象

```java
/**
 * 1. 表示该类为配置类
 * 2. 配置类也是一个组件
 * 3. proxyBeanMethods = true(default)Bean方法代理默认开启。
 *      为了是实现Bean对象的唯一性，对配置类中的Bean方法进行代理。
 *      外部无论对配置类调用多少次组件注册方法，获取的Bean对象都是注册的单实例对象
 */
@Configuration
public class MyConfig {

    /**
     * 配置IOC的Bean，默认单实例
     * 默认ID为方法名字
     * 可以在Bean中添加参数，手动设置ID
     * @return
     */
    @Bean
    public User user(){
        return new User(1,"zhangsan",18);
    }
}
```



## 类导入@Import

向IOC容器中引入类。

这个注解接受Class对象的数组，会调用无参构造，ID为全类名创建Bean



## 条件配置@Conditional

使用其子注解。

如@ConditionalOnBean(name="name")

表示条件成立时才装配其注解的配置

## 配置导入@ImportResource

导入配置文件。

@ImprotResource("classpath:beans.xml")

## 配置绑定

### @ConfigurationProperties

在类中添加

并需要将类添加到IOC容器中


### @EnableConfigurationProperties(User.class)

在配置类中添加，并指定类

作用：

* 类中启动配置绑定
* 将指定类注入容器中

# 自动配置源码分析

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {}
```

## @SpringBootConfiguration

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration  // 表示配置类
public @interface SpringBootConfiguration 
```

## @ComponentScan

指定扫描

## @EnableAutoConfiguration

```java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {}
```

### @AutoConfigurationPackage

自动配置包

利用Register将容器的包下的所有组件添加到容器中

### @Import({AutoConfigurationImportSelector.class})

# 小节

## 自动装配原理

利用Register将容器的包下的所有组件添加到容器中

## 全部导入，按需加载

springboot启动会默认将所有场景的starter加载。

但是在每个starter中，大量利用@Conditional系列注解做到按需加载

## 修改默认配置

特性一：

​		每个spring-boot自动配置的Bean，都会以用户定义的Bean为主。

用户定义之后，spring就不会再创建Bean。

方法一：

​		手动向容器中装载自定义Bean。



特性二：

​		每个spring-boot自动配置的Bean，都会链接到XXXProperties.class中，而XXXProperties.class都会通过配置绑定绑定到核心配置文件的某个前缀。

​		XXXAutoConfiguration ---> 配置组件 ---> 组件的属性来源于XXXproperties.class ---> XXXproperties.class数据绑定到核心配置文件的某个前缀

方法二：

​		通过查询官方文档，或者翻源码分析该Bean所关联的前缀。修改核心配置文件来修改配置

# 简化开发小工具

## Lombok简化开发

lombok插件会自动在编译时生成getter，setter方法。

### 使用流程

1. 导入依赖

   ​	<dependency>
   ​    <groupId>org.projectlombok</groupId>
   ​    <artifactId>lombok</artifactId>
   ​    <version>1.18.22</version>
   ​	</dependency>

2. 安装Lombok插件

   ​		idea2021版本已经集成Lombok插件

### 使用方法

使用@Data注解添加getter，setter方法

使用@ToString注解添加toString方法

使用@AllArgsConstructor和@NoArgsConstructor添加构造方法

## DevTools

热部署

### 导入依赖

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>

​	<optional>true</optional>

</dependency>

### 使用方法

修改代码之后使用ctrl+F9重新build项目就会自动部署

可以热部署静态页，代码重部署就是restart

## spring initializr

