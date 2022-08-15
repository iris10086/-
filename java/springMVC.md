# 快速入门

通过配置将pojo对象中的方法变为servlet的方法

## 步骤

首先配置DispatcherServlet到根映射

```xml
<servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <load-on-startup>1</load-on-startup> 
</servlet>
<servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

需要将pojo对象加载到核心容器，再给相应方法添加注解。

```java
@Component
public class userServlet {

    @RequestMapping("/get")
    public String get(){
        
    }
}
```

配置在web.xml中配置servlet时配置spring-mvc配置文件位置

```xml
<servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
```

# 组件解析

![image-20220207123540560](C:\Users\iris\AppData\Roaming\Typora\typora-user-images\image-20220207123540560.png)

可以通过spring-mvc.xml配置固定前缀后缀

# 注解解析

## @RequestMapping

映射地址

### 属性

value:映射地址

method：访问方式

params：访问参数条件

# 数据响应

## 页面跳转

### 直接返回字符串

直接返回字符串默认与前缀后缀拼接后转发到指定文件

### 内部资源解析器

通过配置内部资源解析器可以指定前后缀

```xml
<!--    配置资源解析器-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/pages/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```



### 通过ModeAndView对象返回

#### 返回ModelAndView对象

可以直接new ModelAndView对象，也可以使用参数请求ModelAndView对象

再设置视图名称setViewName（） 视图名称与直接返回字符串的字符串相同

再设置model（数据） addObject（） 将object添加到request域中

#### 使用Model对象传递数据，通过返回字符串进行页面跳转

通过参数请求一个Model对象，在Model对象中添加数据，setAttribute（）

返回字符串

## 回写数据

### 直接返回字符串

与页面跳转冲突，需要通过注解标记

注解：@ResponseBody

标记要回写字符串，返回的字符串将直接回写到response响应体内

### 返回对象或集合

配置messageConverter

```xml
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
    <property name="messageConverters">
        <list>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
        </list>
    </property>
</bean>
```

可以实现将返回的对象自动转换为json格式

通过配置mvc注解驱动也可以实现返回对象自动转为json

```xml
<mvc:annotation-driven/>
```

## 获得请求数据

### 获取普通参数

直接在方法参数列表中请求对应参数名称与类型

若参数列表不一致则无法访问

### 获取pojo类型

要求：请求参数名称与属性名一致

参数会自动映射

若参数列表不一致，则缺省的参数为null

### 获取数组类型

一般需要通过pojo接受

ajax请求传递json格式，contentType为application/json时可以直接请求数组格式，在参数中需要加@RequestBody注解

## 开放静态资源

框架接管所有的请求访问当做servlet访问寻找请求

但无法请求静态资源

指定文件夹开放

```xml
<mvc:resources mapping="/js/**" location="/js/"/>
```

框架寻找失败，交由服务器软件寻找资源

```xml
<mvc:default-servlet-handler/>
```

## 乱码问题

### 乱码过滤器

```xml
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/</url-pattern>
</filter-mapping>
```

## 参数绑定

### 注解@RequestParam（）

value：绑定第二名称给此参数

required：标识必须包含此参数，不然报错，默认开启

defaultValue：默认值

## restful风格

将参数添加到url地址后

例：pixiv ID

## 自定义类型转换器

1. 定义转换器类实现Converter接口

2. 在配置文件中声明转换器

   将自定义转换器类注入到ConversionServiceFactoryBean中

   ```xml
   <bean id="conversionService2" class="org.springframework.context.support.ConversionServiceFactoryBean">
       <property name="converters">
           <list>
               <bean class="Converter.myConverter"></bean>
           </list>
       </property>
   </bean>
   ```

3. 将ConversionServiceFactoryBean添加到注解驱动中

   ```xml
   <mvc:annotation-driven conversion-service="conversionService2"/>
   ```

# 获取servlet相关API

在函数参数中请求相关API就行

# 获取请求头

使用@RequestHeader（）

```java
@RequestMapping("/test9")
@ResponseBody
public void test9(@RequestHeader(value="User-Agent",required = false) String user_agent){
    System.out.println(user_agent);
}
```

## 获cookie头

使用@CookieValue（）

# 文件上传

1. 导入依赖坐标，fileupload与io坐标

   ```xml
   <!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
   <dependency>
       <groupId>commons-io</groupId>
       <artifactId>commons-io</artifactId>
       <version>2.4</version>
   </dependency>
   <!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
   <dependency>
       <groupId>commons-fileupload</groupId>
       <artifactId>commons-fileupload</artifactId>
       <version>1.3.3</version>
   </dependency>
   ```

2. 配置文件上传解析器

   ```xml
   <!--    配置文件上传解析器-->
   <bean id="multipartResolver" 必须取这名
         class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
       <!--        上传文件总大小-->
       <property name="maxUploadSize" value="5242800"/>
       <!--        上传单个文件大小-->
       <property name="maxUploadSizePerFile" value="5242800"/>
       <!--        上传文件编码类型-->
       <property name="defaultEncoding" value="UTF-8"/>
   </bean>
   ```

3. 编写代码

   框架会将上传的文件封装到MultipartFile对象中进行操作

   对象中有transferTo方法存储，getOriginalFilename方法获取文件名称

# 知识总结

## MVC获取请求数据方法

### 基本类型

名称一致

### POJO类型

属性一致

### 数组类型

参数名称和数组名称一致

### 集合类型

1. 一般情况，请求参数要封装到VO对象中
2. 传ajax请求，并且类型为json，就可直接请求list。但是要写注解

## 细节

### 乱码问题

设置全局filter

### @RequertParm和PathVariable

获取值

### 自定义类型转换器



### 获取servlet相关API



### RequestHeader和CookieValue



### 文件上传



# JdbcTemplate

1. 导入spring-tx与spring-jdbc

2. 创建数据表对应实体对象（属性要一致）

3. 创建jdbcTemplate

   可以使用配置文件注入

4. 执行数据库操作

   更新：update

   查询：query；queryForObject

## 步骤

1. 创建jdbcTamplate对象

2. 建立数据库链接
   1. 使用jdbcTemplate对象的setDataSource方法建立
   2. 需要创建DataSource对象
   3. 可以通过配置文件进行注入
3. 使用jdbcTemplate对象

​	

# 拦截器

与filter一样，在spring环境中常用拦截器 intercepter

继承HandlerIntercepter接口

## 方法

### preHandle

资源执行前执行的代码

返回true则放行。

返回false则拦截

### postHandle

资源获取后，视图返回前执行

### afterCompletion

视图返回后执行

## 配置

在springMVC中配置拦截器

```xml
<!--    配置springMVC拦截器-->
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**"/> // 配置拦截路径
        <mvc:exclude-mapping path="/css/"/> // 配置放行路径
        <mvc:exclude-mapping path="/js/"/>
        <bean class="com.wobuzhidao.intercepter.myIntercepter"/>
    </mvc:interceptor>
</mvc:interceptors>
```

# 异常处理机制

## 异常处理方法

向SimpleMappingExceptionResolve注入属性

```xml
<!--    配置异常处理-->
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <!--        注入默认error page-->
    <property name="defaultErrorView" value="404.jsp"/>
    <!--        指定不同异常类型跳转不同页面-->
    <property name="exceptionMappings">
        <map>
            <entry key="ClassCastException" value="403.jsp"/>
        </map>
    </property>
</bean>
```



## 自定义异常处理方法

1. 创建异常处理类实现HandlerExceptionResolve
2. 配置自定义异常处理方法到容器中
3. 编写页面

