<link rel="stylesheet" type="text/css" href="mkcss.css">

- [背景](#%e8%83%8c%e6%99%af)
  - [构件工具演变](#%e6%9e%84%e4%bb%b6%e5%b7%a5%e5%85%b7%e6%bc%94%e5%8f%98)
  - [配置Maven相关环境变量](#%e9%85%8d%e7%bd%aemaven%e7%9b%b8%e5%85%b3%e7%8e%af%e5%a2%83%e5%8f%98%e9%87%8f)
  - [Maven相关知识](#maven%e7%9b%b8%e5%85%b3%e7%9f%a5%e8%af%86)
- [构建](#%e6%9e%84%e5%bb%ba)
- [Maven目录结构](#maven%e7%9b%ae%e5%bd%95%e7%bb%93%e6%9e%84)
- [常用Maven命令](#%e5%b8%b8%e7%94%a8maven%e5%91%bd%e4%bb%a4)
- [Maven坐标](#maven%e5%9d%90%e6%a0%87)
- [Maven仓库](#maven%e4%bb%93%e5%ba%93)
  - [仓库的分类](#%e4%bb%93%e5%ba%93%e7%9a%84%e5%88%86%e7%b1%bb)
  - [仓库保存的内容：Maven工程](#%e4%bb%93%e5%ba%93%e4%bf%9d%e5%ad%98%e7%9a%84%e5%86%85%e5%ae%b9maven%e5%b7%a5%e7%a8%8b)
- [Maven依赖](#maven%e4%be%9d%e8%b5%96)
  - [依赖范围\<Scope>](#%e4%be%9d%e8%b5%96%e8%8c%83%e5%9b%b4scope)
  - [依赖传递](#%e4%be%9d%e8%b5%96%e4%bc%a0%e9%80%92)
  - [依赖排除](#%e4%be%9d%e8%b5%96%e6%8e%92%e9%99%a4)
  - [依赖冲突](#%e4%be%9d%e8%b5%96%e5%86%b2%e7%aa%81)
  - [统一管理依赖版本号](#%e7%bb%9f%e4%b8%80%e7%ae%a1%e7%90%86%e4%be%9d%e8%b5%96%e7%89%88%e6%9c%ac%e5%8f%b7)
  - [依赖继承](#%e4%be%9d%e8%b5%96%e7%bb%a7%e6%89%bf)
- [聚合](#%e8%81%9a%e5%90%88)
- [Maven生命周期](#maven%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f)
- [依赖查找网站](#%e4%be%9d%e8%b5%96%e6%9f%a5%e6%89%be%e7%bd%91%e7%ab%99)

# 背景
## 构件工具演变
  **Maven**服务于**Java**平台的自动构件工具  
  构件工具演变：  
  >***Make->Ant->Maven->Gradle***  
## 配置Maven相关环境变量  
  - MAVEN_HOME 到bin上一级目录 不带bin
    > E:\Maven\apache-maven-3.6.2
  - path 通常到bin目录 带bin
    > E:\Maven\apache-maven-3.6.2\bin  
  - 配置好后验证版本**cmd**里运行
    ```
    mvn -v
    ```
## Maven相关知识
  1. Maven核心程序中仅仅定义了抽象的生命周期，具体的工作需要特定的**插件**来完成。而插件并不在Maven核心程序中。  
  2. 当我们执行Maven命令时，Maven核心程序首先到**本地仓库**中查找插件,若没有则连接外网，到**中央仓库**下载。  
  3. 本地仓库位置：**系统当前用户的家目录\ .m2\repository**
  4. 修改仓库位置：
  - maven安装目录下 **\conf\settings.xml**
  - 找到**localRepository**标签修改路径即可

# 构建 
  - 清理：  
    将以前编译得到的旧的class字节码**删除**，为下一次编译做准备
  - 编译：  
    将java源程序文件**编译**成class字节码文件  
  - 测试：  
    **自动测试**，自动调用junit程序
  - 报告：  
    测试程序执行的**结果**
  - 打包：  
    动态Web工程打**war包**，java工程打**jar包**
  - 安装：  
    Maven特定的概念——将打包得到的文件**复制**到**仓库**中指定位置
  - 部署：  
    将动态Web工程生成的**war包**复制到s**ervlet容器**的指定**目录**下，使其可以运行

# Maven目录结构
    项目名  
    | --src源码  
    | --|--main(主程序)  
    | --|--|--java(源码)  
    | --|--|--resources(资源文件或框架配置文件)  
    | --|--test(测试程序)  
    | --|--|--java(源码)  
    | --|--|--resources(资源文件或框架配置文件)  
    | --pom.xml(maven配置文件)

# 常用Maven命令
  **注意**：执行与构建相关的Maven命令，必须进入pom.xml所在目录  
  `mvn clean` 清理  
  `mvn compile` 编译主程序  
  `mvn test-compile` 编译测试程序  
  `mvn test` 执行测试  
  `mvn package` 打包
  `mvn install` 对于maven工程安装到本地仓库
  `mvn site` 生成站点 

# Maven坐标
  使用三个向量唯一定位一个Maven工程  
  groupid：公司或组织**域名倒序**+**项目名**  
  &lt;groupid> com.phk.web &lt;/groupid>  
  artifactid：**模块名**  
  &lt;artifactid> socket &lt;/artifactid>  
  version：**版本号**  
  &lt;version> 1.0.0 &lt;/version>  
  Maven工程的坐标与仓库中路径是对应的
  

# Maven仓库
## 仓库的分类
  - **本地**仓库：当前电脑上所有Maven工程服务
  - **远程**仓库：
    - 私服：搭建在**局域网**环境中，为局域网范围内所有Maven工程服务
    - 中央仓库：架设在**Internet**上，为全世界所有Maven工程服务
    - 中央仓库**镜像**：架设在各个大洲，为中央仓库分担流量。减轻中央仓库的压力，同时更快响应用户请求

## 仓库保存的内容：Maven工程
  - Maven自身所需的插件
  - 第三方框架或工具的jar包
  - 我们自己开发的Maven工程

# Maven依赖
  - Maven依赖解析信息时会到**本地仓库**找被依赖的jar包。  
  - 对于我们自己开发的Maven工程，使用`mvn install`命令安装到本地仓库  

  ## 依赖范围\<Scope>
  默认为compile
  - compile
  - [x] 对主程序有效
  - [x] 对测试程序有效
  - [x]  参与打包
  - test
  - [ ] 对主程序有效
  - [x] 对测试程序有效
  - [ ]  参与打包
  - provide
  - [x] 对主程序有效
  - [x] 对测试程序有效
  - [ ]  参与打包  
  例子:servlet-api.jar  
  因为Tomcat容器提供了某些jar包，而开发时并没有这些jar包，部署时会被忽略。
  ## 依赖传递
  依赖具有传递性，Maven会自动将所有需要依赖的自动添加到当前工程在，拓扑序吧
  - 可以传递的依赖不必在每个模块工程都重复声明，只需要在**最底层**工程依赖一次即可
  - 注意：非**compile**范围的依赖不能传递
  ## 依赖排除
  ```
  <dependency>
    <exclusion>
      <groupId><\groudId> //设置排除的坐标即可
      <artifactId><\artifactId>
    <\exclusion>
  <\dependency>
  ```
  ## 依赖冲突
  - 当依赖传递过来的一个jar包有不同版本的时候，传递路径短的jar包版本优先
  - 当两者路径长度相同时，再dependency内先声明者优先
  ## 统一管理依赖版本号
  在pom.xml文件定义一个自定义标签  
  在其他依赖的version标签用${}来代替
  ```
  <properties>
    <phk.version>1.0.0.RELEASE<\phk.version>
  <\properties>

  <groupId>servlet<\groudId> 
  <artifactId>servlet<\artifactId>
  <version>${phk.verson}<\version>
  ```
  ## 依赖继承
  专程建立一个父工程来管理所有子工程的依赖，打包方式为pom  
  父工程：  
  配置依赖管理标签
  ```
  <dependencyManagement>
    <dependenies>
      <groupId>servlet<\groudId> 
      <artifactId>servlet<\artifactId>
      <version>4.9<\version>
      <scope>provided<\scope>
    <\dependenies>
  <\dependencyManagement>
  ```
  子工程声明父工程
  ```
  <parent>
     <groupId><\groudId>  //父工程坐标
      <artifactId><\artifactId>
      <version><\version>
      <!--以当前工程的pom为基准到父工程pom相对路径-->
      <relativePath><\relativePath>
  <\parent>
  ```
  子工程中删除重复部分：  
  例如删除\<groupID>,\<version>  
  配置继承后执行安装命令时要先安装父工程
# 聚合
  作用:一键安装各个模块工程  
  配置方式：在父工程加入\<modules>
  ```
  <modules>
  <!--指定各个子工程相对路径-->
    <module>../web<\module>
    <module>../UI<\**module**>
    <module><\module>
    <module><\module>
  <\modules>
  ```
  使用方式：在聚合工程上运行`mvn install`
# Maven生命周期
1. 各个构件环节执行的顺序：不能打乱顺序，必须按照既定的正确顺序来执行。
2. Maven的核心程序中定义了抽象的生命周期，生命周期中各个阶段的具体任务是由插件来完成的。
3. Maven核心程序为了更好的实现自动化构件，按照这一特点执行生命周期中的各个阶段：不论现在要执行生命周期中的哪一个阶段，都是从这个生命周期最初的位置开始执行
4. - 生命周期的各个阶段仅仅定义了要执行的任务是什么。
   - 各个阶段和插件的目标是对应的。
   - 相似的目标由特定的插件来完成。
 
# 依赖查找网站
www.mvnrepository.com

2020.1.23完结