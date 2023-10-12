

# Maven

## 概述

Maven 的主要目的是为开发者提供

- 一个可复用、可维护、更易理解的工程综合模型
- 与这个模型交互的插件或者工具



Maven 工程结构和内容被定义在一个 xml 文件中 － pom.xml，是 Project Object Model (POM) 的简称



### 约定优于配置

**约定优于配置（convention over configuration）**，也称作**按约定编程**，是一种**软件设计范式**，旨在减少软件开发人员需做出决定的数量，活得简单的好处，而又不失灵活性。

假定 `${basedir}` 表示工程目录：

| 配置项             | 默认值                        |
| :----------------- | :---------------------------- |
| source code        | ${basedir}/src/main/java      |
| resources          | ${basedir}/src/main/resources |
| Tests              | ${basedir}/src/test           |
| Complied byte code | ${basedir}/target             |
| distributable JAR  | ${basedir}/target/classes     |



## POM

POM 代表工程对象模型。使用pom.xml文件描述

POM中设置的一些配置：

- project dependencies
- plugins
- goals
- build profiles
- project version
- developers
- mailing list

在创建 POM 之前，我们首先确定工程组（groupId），及其名称（artifactId）和版本，在仓库中这些属性是工程的唯一标识。



### POM举例

需要说明的是每个工程应该只有一个 POM 文件。

- 所有的 POM 文件需要 **project** 元素和三个必须的字段：**groupId, artifactId,version**。
- 在仓库中的工程标识为 **groupId:artifactId:version**
- POM.xml 的根元素是 **project**，它有三个主要的子节点：

| 节点       | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| groupId    | 这是工程组的标识。它在一个组织或者项目中通常是唯一的。例如，一个银行组织 com.company.bank 拥有所有的和银行相关的项目。 |
| artifactId | 这是工程的标识。它通常是工程的名称。例如，消费者银行。groupId 和 artifactId 一起定义了 artifact 在仓库中的位置。 |
| version    | 这是工程的版本号。在 artifact 的仓库中，它用来区分不同的版本。例如： com.company.bank:consumer-banking:1.0 com.company.bank:consumer-banking:1.1. |



### Super POM

所有POM都继承自一个父POM，项目由Super POM和POM共同配置



## 构建生命周期

一个典型的 Maven 构建生命周期是由以下几个阶段的序列组成的：

| 阶段              | 处理     | 描述                                                 |
| :---------------- | :------- | :--------------------------------------------------- |
| prepare-resources | 资源拷贝 | 本阶段可以自定义需要拷贝的资源                       |
| compile           | 编译     | 本阶段完成源代码编译                                 |
| package           | 打包     | 本阶段根据 pom.xml 中描述的打包配置创建 JAR / WAR 包 |
| install           | 安装     | 本阶段在本地 / 远程仓库中安装工程包                  |

当需要在某个特定阶段之前或之后执行目标时，可以使用 **pre** 和 **post** 来定义这个**目标**。

Maven 有以下三个标准的生命周期：

- clean
- default(or build)
- site



### Clean 生命周期

当我们执行 *mvn post-clean* 命令时，Maven 调用 clean 生命周期，它包含以下阶段。

- pre-clean
- clean
- post-clean

Maven 的 clean 目标（clean:clean）绑定到了 clean 生命周期的 clean 阶段



### Default (or Build) 生命周期

这是 Maven 的主要生命周期，被用于构建应用。包括 23 个阶段

一些重要概念：

当一个阶段通过 Maven 命令调用时，例如 mvn compile，只有该阶段之前以及包括该阶段在内的所有阶段会被执行。

不同的 maven 目标将根据打包的类型（JAR / WAR / EAR），被绑定到不同的 Maven 生命周期阶段。



### Site 生命周期

Maven Site 插件一般用来创建新的报告文档、部署站点等。

阶段：

- pre-site
- site
- post-site
- site-deploy



## 构建配置文件

构建配置文件是一组配置的集合，用来配置或者覆盖 Maven 构建的配置文件，可以为不同的环境构建不同的构建过程。

### Profile 类型

Profile 主要有三种类型。

| 类型        | 在哪里定义                                                   |
| :---------- | :----------------------------------------------------------- |
| Per Project | 定义在工程 POM 文件 pom.xml 中                               |
| Per User    | 定义在 Maven 设置 xml 文件中 （%USER_HOME%/.m2/settings.xml） |
| Global      | 定义在 Maven 全局配置 xml 文件中 （%M2_HOME%/conf/settings.xml） |

### Profile 激活

Maven 的 Profile 能够通过几种不同的方式激活。

- 显式使用命令控制台输入
- 通过 maven 设置
- 基于环境变量（用户 / 系统变量）
- 操作系统配置（例如，Windows family）
- 现存 / 缺失 文件
