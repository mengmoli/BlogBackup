---
title: 学习SpringCloud:Nacos(注册中心+配置中心)
date: 2019-10-17
tags:
- springcloud
---
### 简介
Nacos 致力于帮助您发现、配置和管理微服务。Nacos 提供了一组简单易用的特性集，帮助您快速实现动态服务发现、服务配置、服务元数据及流量管理。  

Nacos 帮助您更敏捷和容易地构建、交付和管理微服务平台。 Nacos 是构建以“服务”为中心的现代应用架构 (例如微服务范式、云原生范式) 的服务基础设施。  

接下来使用Nacos作为微服务架构中的注册中心（替代：eureka、consul等传统方案）以及配置中心（spring cloud config）来使用。
### 第一步 安装Nacos

下载地址：https://github.com/alibaba/nacos/releases

本文版本：1.1.0

下载完成之后，解压。根据不同平台，执行不同命令，启动单机版Nacos服务：

在Linux / Unix / Mac平台上，运行以下命令以独立模式启动服务器：
```text
sh startup.sh -m standalone
```
在Windows平台上，运行以下命令以独立模式启动服务器。或者，您也可以双击startup.cmd以运行NacosServer。
```text
cmd startup.cmd -m standalone
```
>startup.sh脚本位于Nacos解压后的bin目录下

启动完成之后，访问：http://127.0.0.1:8848/nacos/，可以进入Nacos的服务管理页面

默认登录用户名密码为：nacos

![nacos控制台界面](/assets/blogImg/nacos-console.png)
### 第二步 编写应用接入到Nacos注册中心
创建一个spring-cloud-alibaba-learning项目，pom.xml加入全局依赖配置
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.mori</groupId>
    <artifactId>spring.cloud.learning</artifactId>
    <packaging>pom</packaging>
    <version>1.0</version>

    <!--定义springboot 版本 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <spring-cloud.version>Greenwich.RELEASE</spring-cloud.version>
        <spring-cloud-alibaba.version>0.9.0.RELEASE</spring-cloud-alibaba.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--Spring cloud alibaba-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>${spring-cloud-alibaba.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <!--放在依赖里，所有的子项目就直接依赖了-->
    <dependencies>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.8.1</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>

</project>

```
接下来创建一个模块 nacos-discovery-server, pom.xml 加入
```
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
    </dependencies>

```
在nacos-discovery-server中编写一个应用启动类
```java
@EnableDiscoveryClient
@SpringBootApplication
public class DiscoveryServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(DiscoveryServerApplication.class, args);
    }

    @Slf4j
    @RestController
    static class TestController {

        @GetMapping("/hello")
        public String hello(@RequestParam String name) {
            log.info("invoked name = " + name);
            return "hello " + name;
        }
    }
}
```
在resource中加入bootstrap.yml配置
```yml
server:
  port: 8001
spring:
  application:
    name: nocos-discovery-server
  cloud:
        nacos:
          discovery:
            server-addr: 127.0.0.1:8848
  profiles:
    active: dev
  main:
    allow-bean-definition-overriding: true
```
启动后，可以看到控制台打印,表示注册成功
```
INFO 44792 --- [    main] o.s.c.a.n.registry.NacosServiceRegistry  : nacos registry, nocos-discovery-server 10.0.75.1:8001 register finished
```
在nocos控制台查看服务列表，注册成功  
![nacos服务列表](/assets/blogImg/nacos-service-list.png)