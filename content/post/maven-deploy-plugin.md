---
title: Maven Deploy Plugin 使用方法
author: Scott
tags:
  - Maven
  - Java
categories: []
date: 2020-01-02 11:23:00
---

Maven 依赖配置：
```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-deploy-plugin</artifactId>
      <configuration>
        <skip>true</skip>
      </configuration>
    </plugin>
  </plugins>
</build>
```
可以通过 `<skip>true</skip>` 配置来跳过某些模块执行 `mvn deploy` 动作。


> 最新版本：https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-deploy-plugin

