# 安装

绿色免安装

## 配置环境变量

配置MAVEN_HOME

添加MAVEN_HOMG\bin到path

# 目录结构

## src

### main

#### java

代码

#### resources

资源

#### (webapp)

web项目才有

### test

#### java

测试代码

## pom.xml

配置文件，添加依赖需要在这里添加

# 常用命令

## compile编译

编译为class文件方在target目录下

## clean清除

删除target目录

## package打包

压缩target目录为packaging配置的文件

# 生命周期



# 坐标

确定唯一依赖的三个值

```xml
<groupId>org.apache.tomcat.maven</groupId>
<artifactId>tomcat7-maven-plugin</artifactId>
<version>2.2</version>
```



# 导入依赖

在<dependencies>标签中的<dependency>配置依赖的坐标

```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.tomcat</groupId>
            <artifactId>tomcat</artifactId>
            <version>9.0.56</version>
            <type>pom</type>
        </dependency>
    </dependencies>
```



# 设置插件

在<build>标签中的<plugins>下的<plugin>配置插件的坐标

```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <port>80</port>
                    <path>/</path>
                </configuration>
            </plugin>
        </plugins>
    </build>
```



# 依赖范围

配置依赖中同时配置

三个作用范围：

1. 编译环境

   在写代码阶段

2. 测试环境

   在test测试目录下

3. 运行时

   运行阶段

## compile

默认，在编译、测试、运行时都能使用

## test

只在测试环境使用

例如：juint

## provided

在编译和测试环境下使用，运行时后不使用

例如：servlet

解释：servlet运行在tomcat中，tomcat自带servlet.jar包，再导入就会冲突

## runtime

仅在运行时使用

例如：jdbc驱动

解释：jdbc驱动仅在运行时调用，在编译时不调用且不报错

# 聚合

创建一个聚合模块管理分割开来的所有模块

## 定义

```xml
<!--定义该模块为聚合模块-->
<packing>pom</packing>
<!-- 管理的工程列表-->
<modules>
	<module>../ssm_pojo</module>
    <module>../ssm_service</module>
    <module>../ssm_dao</module>
    <module>../ssm_controller</module>
</modules>
```



# 继承

进行子包的依赖版本管理

在聚合模块中导入依赖及版本后，子模块不需要再指定依赖版本。

```xml
<dependencyManagement></dependencyManagement>
    <dependencies>
        <dependency>
			坐标
            版本
        </dependency>
    </dependencies>
</dependencyManagement>
```

但是在子模块中需要指定父工程

```xml
<parent>
	坐标
    <relativePath>聚合模块的pom文件</relativePath>
</parent>
```

插件管理

```xml
<pulginManagement>
	...
</pulginManagement>
```



# 属性

## 定义

```xml
<properties>
    <elementName>value</elementName>
    <spring.version>5.1.9</spring.version>
</properties>
...
${spring.version}
```

使用EL表达式获取

# 版本管理

release 正式发布版本

snapshot 抢先体验版

# 管理其他资源文件



```xml
<!-- 加载配置资源文件信息 -->
<resources>
	<resource>
        <!-- 设定资源文件夹路径 -->
    	<directory>${project.basedir}/src/main/resources</directory>
       	<!--开启管理-->
        <filtering>true</filtering>
    </resource>
</resources>
```

使用同样使用pom中定义的属性

# 多环境

创建多环境

```xml
<!-- 创建多环境 -->
<profiles>
	<profile>
    	<id>Uid1</id>
        <properties>
        	<elementName>value</elementName>
        </properties>
        <!-- 设置默认 -->
        <activation>
        	<activeByDefault>true</activeByDefault>
        </activation>
    </profile>
    <profile>
    	<id>Uid2</id>
        <properties>
        	<elementName>value</elementName>
        </properties>
    </profile>
</profiles>
```

使用时通过

mvn 指令 -P 环境ID

# 私服

## nexus

### 安装

免安装

### 启动

指令启动

nexus.exe /run nexus

### 访问

localhost:8081

### 配置

安装路径下etc目录中的nexus-default.properties文件有基础信息

bin下的nexus.cmoptions有服务器启动的配置信息 

## 本地仓库访问私服

### 配置本地仓库访问权限

setting.xml下配置

```xml
<servers>
	<server>
    	<id>仓库ID</id>
        <username>admin</username>
        <password>hejieze2684</password>
    </server>
</servers>
```

### 下载

配置maven镜像

在maven的setting.xml中的mirrors下

```xml
<mirror>
    <id>maven-snapshots</id>
    <name>my-maven-snapshots</name>
    <mirrorOf>*</mirrorOf>
    <url>http://localhost:8081/repository/maven-snapshots/</url>
</mirror>
```

### 上传

配置项目pom.xml

根标签下

```xml
    <distributionManagement>
        <repository>
            <id>maven-releases</id>
            <url>http://localhost:8081/repository/maven-releases/</url>
        </repository>
        <snapshotRepository>
            <id>test</id>
            <name>test</name>
            <url>http://localhost:8081/repository/my-snapshots/</url>
        </snapshotRepository>
    </distributionManagement>
```

