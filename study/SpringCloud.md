# SpringCloud

SpringCloud + SpringCloud alibaba

技术要求：

* java8
* maven
* git
* Nginx
* RabbitMQ
* SpringBoot



**约定 > 配置 > 编码**

搭建一个微服务模块，主要包含一下几步：

1. **建立module**
2. **改pom**
3. **写yml**
4. **主启动**
5. **业务类**



## 一、微服务

### 1.1 什么是微服务

​		微服务是一种架构模式，它将单一应用程序划分为一组小的服务，各个服务之间互相协调配合，为用户体现价值。每个服务运行在其独立的进程中，服务与服务之间采用轻量级的通信机制互相协作（通常是基于HTTP协议的RESTful API）。每个服务都围绕其具体业务进行构建，并且能够被独立的部署到生产环境、类生产环境等。通俗而言，微服务就是每个SpringBoot开发的一个一个功能单元。

### 1.2 分布式微服务

SpringCloud是分布式微服务的其中一种，微服务全家桶。

SpringCloud至少包含以下功能：

![image-20200824233131721](C:\Users\10538\AppData\Roaming\Typora\typora-user-images\image-20200824233131721.png)



1. 一个通用的微服务框架如下：

![image-20200824233507256](C:\Users\10538\AppData\Roaming\Typora\typora-user-images\image-20200824233507256.png)



2. 所用到的主要技术有：

![image-20200824233731460](C:\Users\10538\AppData\Roaming\Typora\typora-user-images\image-20200824233731460.png)



3. 最终，一个SpringCloud微服务的整个流程如下：

![image-20200824233914534](C:\Users\10538\AppData\Roaming\Typora\typora-user-images\image-20200824233914534.png)



4. Cloud升级

   ![image-20200901104753097](C:\Users\10538\AppData\Roaming\Typora\typora-user-images\image-20200901104753097.png)



## 二、项目搭建

项目搭建分为两步，分别是：

1. **父工程project**
2. **project下面创建多个module**,  比如**订单模块**、**支付模块**。





## 三、结构分析

SpringCloud主要分为：

* **服务注册中心**
* **服务调用**
* **服务降级**
* **服务网管**
* **服务配置**
* **服务总线**



### 3.1 服务注册中心

**服务注册**：将服务信息注册到注册中心

**服务发现**：从注册中心上获取服务信息

实质：是一个Map结构。

​			key：服务名

​			value：调用地址

**问题：微服务RPC远程服务调用最核心的是声明？**

答：高可用。如果注册中心只有一个，并且其出故障，将会导致整个服务环境不可用。

解决办法：搭建Eureka注册中心集群，实现负载均衡+故障容错