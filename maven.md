<link rel="stylesheet" type="text/css" href="mkcss.css">

# Maven学习
* ## 背景
    **Maven**服务于**Java**平台的自动构件工具  
    构件工具演变：  
    > ***Make->Ant->Maven->Gradle***  
* ## 配置Maven相关环境变量  
  - MAVEN_HOME 到bin上一级目录 不带bin
    > E:\Maven\apache-maven-3.6.2
  - path 通常到bin目录 带bin
    > E:\Maven\apache-maven-3.6.2\bin  
  - 配置好后验证版本**cmd**里运行
    > `mvn -v`


# 构建 
  * ## 构件的各个环节  
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
