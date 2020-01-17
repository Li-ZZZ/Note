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
  - [依赖范围<spoce>](#%e4%be%9d%e8%b5%96%e8%8c%83%e5%9b%b4spoce)

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
   Maven依赖解析信息时会到**本地仓库**找被依赖的jar包。  
  对于我们自己开发的Maven工程，使用`mvn install`命令安装到本地仓库
## 依赖范围<spoce>
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
  因为Tomcat容器提供了某些jar包，而开发时并没有这些jar包，部署时会被忽略。