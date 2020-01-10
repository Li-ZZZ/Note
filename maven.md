<link rel="stylesheet" type="text/css" href="mkcss.css">

# Maven学习
- ## 背景
  **Maven**服务于**Java**平台的自动构件工具  
  构件工具演变：  
  > ***Make->Ant->Maven->Gradle***  
- ## 配置Maven相关环境变量  
  - MAVEN_HOME 到bin上一级目录 不带bin
    > E:\Maven\apache-maven-3.6.2
  - path 通常到bin目录 带bin
    > E:\Maven\apache-maven-3.6.2\bin  
  - 配置好后验证版本**cmd**里运行
    > `mvn -v`

---
- ## 构建 
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
---
- ## Maven目录结构
    >项目名  
    | --src源码  
    | --|--main(主程序)  
    | --|--|--java(源码)  
    | --|--|--resources(资源文件或框架配置文件)  
    | --|--test(测试程序)  
    | --|--|--java(源码)  
    | --|--|--resources(资源文件或框架配置文件)  
    | --pom.xml(maven配置文件)

- ## 常用Maven命令
  **注意**：执行与构建相关的Maven命令，必须进入pom.xml所在目录  
  命令:
    >- `mvn clean`:清理
    >- `mvn compile`:编译主程序
    >- `mvn test-compile`:编译测试程序
    >- `mvn test`:执行测试
    >- `mvn package`:打包
---
- ## Maven相关知识
  1. Maven核心程序中仅仅定义了抽象的生命周期，具体的工作需要特定的**插件**来完成。而插件并不在Maven核心程序中。  
  2. 当我们执行Maven命令时，Maven核心程序首先到**本地仓库**中查找插件,若没有则连接外网，到**中央仓库**下载。  
  3. 本地仓库位置：**系统当前用户的家目录\ .m2\repository**
  4. 修改仓库位置  
  > - maven安装目录下 **\conf\settings.xml**
  > - 找到**localRepository**标签修改路径即可

