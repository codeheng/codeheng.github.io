---
comments: true
---

> [Maven](https://maven.apache.org/) 是一款用于 **^^管理和构建Java项目的工具^^**

!!! Note "作用"

    1. **依赖管理**：方便快捷的管理项目依赖的资源(jar包)，避免版本冲突

        - 直接通过`pom.xml`中添加依赖即可

    2. **项目构建**：标准化的跨平台的自动化构建方式
    3. **统一项目结构**

    ```code
    project
    ├── main
    │   ├── java
    │   └── resources
    ├── test
    │   ├── java
    │   └── resources
    └── pom.xml    
    ```

## 概述

> 基于 ^^项目对象模型（POM，*Project Object Model*）^^，通过一小段描述信息来管理项目的构建、报告和文档

![](./assets/maven模型.jpg)

即将自己的项目抽象成一个对象模型，有自己的专属坐标

- 坐标：资源(jar包)的唯一标识，通过坐标可以定位到所需资源(jar包)位置
    * 组成：groupId(组织名), arfitactId(模块名), Version(版本)