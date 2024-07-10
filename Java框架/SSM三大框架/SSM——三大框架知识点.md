# Maven入门和进阶

## Maven简介和快速入门

### Maven 介绍

- Maven 是一款为 Java 项目构建管理、依赖管理的工具（软件），使用 Maven 可以自动化构建、测试、打包和发布项目，大大提高了开发效率和质量

### Maven主要作用理解

#### 场景概念

- 场景一
  - 例如我们项目需要第三方库（依赖），如Druid连接池、MySQL数据库驱动和Jackson等。那么我们可以将需要的依赖项的信息编写到Maven工程的配置文件，Maven软件就会自动下载并复制这些依赖项到项目中，也会自动下载依赖需要的依赖！确保依赖版本正确无冲突和依赖完整
- 场景二
  - 项目开发完成后，想要将项目打成.war文件，并部署到服务器中运行，使用Maven软件，我们可以通过一行构建命令（mvn package）快速项目构建和打包！节省大量时间

#### 依赖管理

- Maven 可以管理项目的依赖，包括自动下载所需依赖库、自动下载依赖需要的依赖并且保证版本没有冲突、依赖版本管理等。通过 Maven，我们可以方便地维护项目所依赖的外部库，而我们仅仅需要编写配置即可

#### 构建管理

- Maven 可以管理项目的编译、测试、打包、部署等构建过程。通过实现标准的构建生命周期，Maven 可以确保每一个构建过程都遵循同样的规则和最佳实践。同时，Maven 的插件机制也使得开发者可以对构建过程进行扩展和定制。主动触发构建，只需要简单的命令操作即可

![](../SSM——三大框架知识点_img/Maven入门与进阶/构建管理.png)

> IDEA 和 Ecplise 两者构建的格式不同，导致项目不能很好的兼容，此时我们就可以用Maven 来很好的解决这个问题，用IDEA 或 Ecplise 来进行代码提示，Maven进行项目的构建

 

## 基于IDEA 的Maven 工程创建

### 梳理Maven 工程GAVP 属性

> Maven工程相对之前的工程，多出一组gavp属性，gav需要我们在创建项目的时指定，p有默认值，后期通过配置文件修改

- Maven 中的 GAVP 是指 GroupId、ArtifactId、Version、Packaging 等四个属性的缩写，其中前三个是必要的，而 Packaging 属性为可选项。这四个属性主要为每个项目在maven仓库总做一个标识，类似人的《姓-名》。有了具体标识，方便maven软件对项目进行管理和互相引用

#### GAV遵循以下规则

- **GroupID 格式**：com.{公司/BU }.业务线.[子业务线]，最多 4 级

```例如
说明：{公司/BU} 
例如：alibaba/taobao/tmall/aliexpress 等 BU 一级；
子业务线可选。

正例：
com.taobao.tddl 
com.alibaba.sourcing.multilang  
com.atguigu.java
```

- **ArtifactID 格式**：产品线名-模块名。语义不重复不遗漏，先到仓库中心去查证一下

```例如
正例：tc-client / uic-api / tair-tool / bookstore
```

- **Version版本号格式推荐**：主版本号.次版本号.修订号 1.0.0

  - 主版本号：当做了不兼容的 API 修改，或者增加了能改变产品方向的新功能


  - 次版本号：当做了向下兼容的功能性新增（新增类、接口等）


  - 修订号：修复 bug，没有修改方法签名的功能加强，保持 API 兼容性

```例如
例如： 初始→1.0.0  修改bug → 1.0.1  功能调整 → 1.1.1等
```

#### Packaging 定义规则

- 指示将项目打包为什么类型的文件，idea根据packaging值，识别maven项目类型


- packaging 属性为 jar（默认值），代表普通的Java工程，打包以后是.jar结尾的文件


- packaging 属性为 war，代表Java的web工程，打包以后.war结尾的文件


- packaging 属性为 pom，代表不会打包，用来做继承的父工程

### Maven工程项目结构说明

- Maven 是一个强大的构建工具，它提供一种标准化的项目结构，可以帮助开发者更容易地管理项目的依赖、构建、测试和发布等任务。以下是 Maven Web 程序的文件结构及每个文件的作用：

```xml
|-- pom.xml                               # Maven 项目管理文件 
|-- src
    |-- main                              # 项目主要代码
    |   |-- java                          # Java 源代码目录
    |   |   `-- com/example/myapp         # 开发者代码主目录
    |   |       |-- controller            # 存放 Controller 层代码的目录
    |   |       |-- service               # 存放 Service 层代码的目录
    |   |       |-- dao                   # 存放 DAO 层代码的目录
    |   |       `-- model                 # 存放数据模型的目录
    |   |-- resources                     # 资源目录，存放配置文件、静态资源等
    |   |   |-- log4j.properties          # 日志配置文件
    |   |   |-- spring-mybatis.xml        # Spring Mybatis 配置文件
    |   |   `-- static                    # 存放静态资源的目录
    |   |       |-- css                   # 存放 CSS 文件的目录
    |   |       |-- js                    # 存放 JavaScript 文件的目录
    |   |       `-- images                # 存放图片资源的目录
    |   `-- webapp                        # 存放 WEB 相关配置和资源
    |       |-- WEB-INF                   # 存放 WEB 应用配置文件
    |       |   |-- web.xml               # Web 应用的部署描述文件
    |       |   `-- classes               # 存放编译后的 class 文件
    |       `-- index.html                # Web 应用入口页面
    `-- test                              # 项目测试代码
        |-- java                          # 单元测试目录
        `-- resources                     # 测试资源目录
```
```

- pom.xml：Maven 项目管理文件，用于描述项目的依赖和构建配置等信息
- src/main/java：存放项目的 Java 源代码
- src/main/resources：存放项目的资源文件，如配置文件、静态资源等
- src/main/webapp/WEB-INF：存放 Web 应用的配置文件
- src/main/webapp/index.html：Web 应用的入口页面
- src/test/java：存放项目的测试代码
- src/test/resources：存放测试相关的资源文件，如测试配置文件等

 s

### Maven 核心功能依赖和构建管理

#### 依赖管理和配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>


    <!--!gav属性 -->
    <!-- 不会改变-->
    <groupId>com.JandyLi</groupId>
    <artifactId>maven-javaee-project-02</artifactId>
    <!--构建过程-部署-修改 -->
    <version>1.0.0</version>
    <!--maven 工程的打包方式 java jar[默认值] web war 不打包 pom  -->
    <packaging>war</packaging>

    <!--声明版本号-->
    <properties>
        <!--
            声明一个变量！声明完变量以后，在其他位置可以引用${jackson.version}
            注意：声明的标签建议两层以上命名！（避免冲突）version 1.15.2
                 推荐：技术名.version
        -->
        <jackson.version>2.15.2</jackson.version>

    </properties>

    <!--
        第三方依赖信息声明
        dependencies - 项目依赖的集合
        dependency - 每一个依赖项
        gav - 依赖的信息就是其他的maven工程[jar]

        第三方依赖信息怎么知道？
        1.maven 提供的查询官网 http://mvnrepository.com
        2.maven 插件 maven-search

        扩展
        1.提取版本号统一管理
        2.可选属性 scope
            scope 引入依赖的作用域
            默认值是：comile main test 打包和运行
                     test  test            junit@Test
                     runtime main 不会 test 不会 打包和运行 mysql Class.forName(com.mysql.cj.jdbc.Driver)
                     provided  main test 打包和运行不会 servlet HttpServlet tomcat提供servlet

    -->
    <dependencies>
        <!--定位信息 gav 三个属性是必须的-->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>${jackson.version}</version>
            <scope>compile</scope>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>${jackson.version}</version>
            <scope>compile</scope>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>


    </dependencies>


</project>
```

#### 依赖传递和冲突

##### 依赖传递

- 指的是当一个模块或库 A 依赖于另一个模块或库 B，而 B又依赖于模块或库 C，那么 A 会间接依赖于 C。这种依赖传递结构可以形成一个依赖树。当我们引入一个库或框架时，构建工具（如 Maven、Gradle）会自动解析和加载其所有的直接和间接依赖，确保这些依赖都可用

##### 依赖传递的作用

- 减少重复依赖：当多个项目依赖同一个库时，Maven 可以自动下载并且只下载一次该库。这样可以减少项目的构建时间和磁盘空间
- 自动管理依赖: Maven 可以自动管理依赖项，使用依赖传递，简化了依赖项的管理，使项目构建更加可靠和一致
- 确保依赖版本正确性：通过依赖传递的依赖，之间都不会存在版本兼容性问题，确实依赖的版本正确性

##### 依赖冲突

- 当直接引用或者间接引用出现了相同的jar包! 这时呢，一个项目就会出现相同的重复jar包，这就算作冲突！依赖冲突避免出现重复依赖，并且终止依赖传递

![](../SSM——三大框架知识点_img/Maven入门与进阶/依赖冲突.png)

- 当直接引用或者间接引用出现了相同的jar包! 这时呢，一个项目就会出现相同的重复jar包，这就算作冲突！依赖冲突避免

- 解决依赖冲突（如何选择重复依赖）方式：
  - 自动选择原则 

      - 短路优先原则（第一原则）

      ```
      A—>B—>C—>D—>E—>X(version 0.0.1)
      
      A—>F—>X(version 0.0.2)
      
      则A依赖于X(version 0.0.2)
      ```

      - 依赖路径长度相同情况下，则“先声明优先”（第二原则）

      ```
      A—>E—>X(version 0.0.1)
      
      A—>F—>X(version 0.0.2)
      
      在<depencies></depencies>中，先声明的，路径相同，会优先选择！
      ```

      > 只要发生冲突了，后续的依赖传递全部终止



#### 依赖导入失败场景和解决方案

在使用 Maven 构建项目时，可能会发生依赖项下载错误的情况，主要原因有以下几种：

- 下载依赖时出现网络故障或仓库服务器宕机等原因，导致无法连接至 Maven 仓库，从而无法下载依赖
- 依赖项的版本号或配置文件中的版本号错误，或者依赖项没有正确定义，导致 Maven 下载的依赖项与实际需要的不一致，从而引发错误
- 本地 Maven 仓库或缓存被污染或损坏，导致 Maven 无法正确地使用现有的依赖项，并且也无法重新下载

解决方案：

- 检查网络连接和 Maven 仓库服务器状态
- 确保依赖项的版本号与项目对应的版本号匹配，并检查 POM 文件中的依赖项是否正确
- 清除本地 Maven 仓库缓存（lastUpdated 文件），因为只要存在lastupdated缓存文件，刷新也不会重新下载。本地仓库中，根据依赖的gav属性依次向下查找文件夹，最终删除内部的文件，刷新重新下载即可  



#### 扩展构建管理和插件配置

##### 构建概念

- 项目构建是指将源代码、依赖库和资源文件等转换成可执行或可部署的应用程序的过程，在这个过程中包括编译源代码、链接依赖库、打包和部署等多个步骤

![](../SSM——三大框架知识点_img/Maven入门与进阶/构建管理.png)

##### 主动出发场景

- 重新编译 : 编译不充分, 部分文件没有被编译!
- 打包 : 独立部署到外部服务器软件,打包部署
- 部署本地或者私服仓库 : maven工程加入到本地或者私服仓库,供其他工程使用

##### 命令方式构建

- 语法
  - mvn 构建命令 构建命令……

| 命令        | 描述                                        |
| ----------- | ------------------------------------------- |
| mvn clean   | 清理编译或打包后的项目结构,删除target文件夹 |
| mvn compile | 编译项目，生成target文件                    |
| mvn test    | 执行测试源码 (测试)                         |
| mvn site    | 生成一个项目依赖信息的展示页面              |
| mvn package | 打包项目，生成war / jar 文件                |
| mvn install | 打包后上传到maven本地仓库(本地部署)         |
| mvn deploy  | 只打包，上传到maven私服仓库(私服部署)       |

> 命令执行需要我们进入项目的根路径
>
> 部署必须是jar包形式

##### 构建命令周期

- 构建生命周期可以理解成是一组固定构建命令的有序集合，触发周期后的命令，会自动触发周期前的命令！也是一种简化构建的思路!
- 清理周期：
    - 主要是对项目编译生成文件进行清理
    - 包含命令：clean

- 默认周期：

    - 定义了真正构件时所需要执行的所有步骤，它是生命周期中最核心的部分

    - 包含命令：compile - test - package - install / deploy
- 报告周期

    - 包含命令：site

    - 打包: mvn clean package 本地仓库: mvn clean install

##### 最佳使用方案

```
打包: mvn clean package
重新编译: mvn clean compile
本地部署: mvn clean install 
```



##### 周期、命令、插件

- 周期→包含若干命令→包含若干插件!

  使用周期命令构建，简化构建过程！

  最终进行构建的是插件！



## Maven继承和聚合特性

### Maven工程继承关系

- Maven 继承是指在 Maven 的项目中，让一个项目从另一个项目中继承配置信息的机制。继承可以让我们在多个项目中共享同一配置信息，简化项目的管理和维护工作

![](../SSM——三大框架知识点_img/Maven入门与进阶/继承关系.png)

### 继承作用

- 作用：在父工程中统一管理项目中的依赖信息,进行统一版本管理


- 它的背景是：

  - 对一个比较大型的项目进行了模块拆分

  - 一个 project 下面，创建了很多个module

  - 每一个 module 都需要配置自己的依赖信息


- 它背后的需求是：

  - 多个模块要使用同一个框架，它们应该是同一个版本，所以整个项目中使用的框架版本需要统一管理

  - 使用框架时所需要的 jar 包组合（或者说依赖信息组合）需要经过长期摸索和反复调试，最终确定一个可用组合。这个耗费很大精力总结出来的方案不应该在新的项目中重新摸索


- 通过在父工程中为整个项目维护依赖信息的组合既保证了整个项目使用规范、准确的 jar 包；又能够将以往的经验沉淀下来，节约时间和精力

### 继承语法

#### 父工程

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.JandyLi</groupId>
    <artifactId>maven-pom-parent-03</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!--父工程不打包，也不写代码-->
    <packaging>pom</packaging>
    
    <!-- 这是聚合，统一管理那些子工程的artifactId-->
    <modules>
        <module>shop-user</module>
    </modules>


    <!--声明版本信息-->
    <!--导入依赖！此处导入，所有子工程都有相应的依赖！-->
    <dependencies></dependencies>
    <!--声明依赖，不会下载依赖，可以被子工程继承版本号！-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.30</version>
            </dependency>

            <dependency>
                <groupId>com.fasterxml.jackson.core</groupId>
                <artifactId>jackson-core</artifactId>
                <version>2.15.2</version>
            </dependency>
        </dependencies>

    </dependencyManagement>


</project>
```

#### 子工程

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!--parent 标签就是指定继承父工程的gav属性-->
    <parent>
        <groupId>com.JandyLi</groupId>
        <artifactId>maven-pom-parent-03</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <!--g v属性也继承父工程，也可以自己定义-->
    <artifactId>shop-user</artifactId>

    <dependencies>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
    </dependencies>


</project>
```

> 当子工程自己定义gav时，则会覆盖父工程的gav信息



### Maven工程聚合关系

#### 聚合概念

- Maven 聚合是指将多个项目组织到一个父级项目中，通过触发父工程的构建,统一按顺序触发子工程构建的过程

#### 聚合作用

- 统一管理子项目构建：通过聚合，可以将多个子项目组织在一起，方便管理和维护
- 优化构建顺序：通过聚合，可以对多个项目进行顺序控制，避免出现构建依赖混乱导致构建失败的情况 

#### 聚合语法

- 父项目中包含的子项目列表

```xml
<project>
  <groupId>com.example</groupId>
  <artifactId>parent-project</artifactId>
  <packaging>pom</packaging>
  <version>1.0.0</version>
  <modules>
    <module>child-project1</module>
    <module>child-project2</module>
  </modules>
</project>
```





# SpringFramework实战指南

