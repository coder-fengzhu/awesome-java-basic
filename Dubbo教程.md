视频目录：

1. 微服务介绍；
2. dubbo介绍；
3. 基于spring boot开发dubbo服务提供方，服务调用方；
4. 部署docker部署注册中心nacos
5. 启动dubbo应用，测试dubbo调用

## 微服务介绍
#### 单体架构
早期的软件，所有功能都写在一起，这称为单体架构（monolithic software）。

整个软件就是单一的整体，彷佛一体化的机器。

可以想到，软件的功能越多，单体架构就会越复杂，很多缺点也随之暴露出来。

缺点：

+ 所有功能耦合在一起，互相影响，最终难以管理。
+ 哪怕只修改一行代码，整个软件就要重新构建和部署，成本非常高。
+ 因为软件做成了一个整体，不可能每个功能单独开发和测试，只能整体开发和测试，导致必须采用瀑布式开发模型。



#### 微服务
微服务（Microservices）是一种软件开发架构，它将一个应用程序构建为一系列小型服务的集合，每个服务运行在其独立的进程中，并通常围绕特定的业务能力进行构建。这些服务可以通过定义良好的API进行通信，通常是HTTP RESTful API或RPC调用。

微服务架构的主要特点包括：

1. **独立部署**：每个微服务可以独立部署，不需要重新部署整个应用程序。
2. **技术多样性**：不同的微服务可以使用不同的编程语言、数据库或其他技术栈。
3. **可扩展性**：可以根据需求对单个服务进行扩展，而不影响其他服务。
4. **容错性**：一个服务的失败不会立即导致整个应用程序的崩溃，提高了系统的稳定性。
5. **敏捷性**：微服务架构支持敏捷开发和持续集成/持续部署（CI/CD），使得新功能的迭代和部署更加快速和灵活。

微服务架构也有一些挑战，比如服务之间的协调、数据一致性问题、服务发现和负载均衡等。因此，采用微服务架构需要仔细规划和设计。



## Dubbo介绍
[dubbo介绍](https://cn.dubbo.apache.org/zh-cn/blog/2023/02/23/%E4%B8%80%E6%96%87%E5%B8%AE%E4%BD%A0%E5%BF%AB%E9%80%9F%E4%BA%86%E8%A7%A3-dubbo-%E6%A0%B8%E5%BF%83%E8%83%BD%E5%8A%9B/)



## 开发provider跟consumer应用
```shell
<dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo-spring-boot-starter</artifactId>
                <version>3.3.0</version>
            </dependency>
            <dependency>
                <groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo-nacos-spring-boot-starter</artifactId>
                <version>3.3.0</version>
            </dependency>

            <!-- spring starter -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter</artifactId>
                <version>3.2.3</version>
            </dependency>

            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>3.2.3</version>
            </dependency>

            <dependency>
                <groupId>com.fasterxml.jackson.datatype</groupId>
                <artifactId>jackson-datatype-jdk8</artifactId>
                <version>2.15.4</version>
            </dependency>

            <dependency>
                <groupId>com.fasterxml.jackson.datatype</groupId>
                <artifactId>jackson-datatype-jsr310</artifactId>
                <version>2.15.4</version>
            </dependency>

        </dependencies>
    </dependencyManagement>
```



## 部署注册中心
```shell
git clone https://github.com/nacos-group/nacos-docker.git
cd nacos-docker

docker-compose -f example/standalone-derby.yaml up
```



provider的配置

```shell
dubbo:
  application:
      name: dubbo-demo-provider
      logger: slf4j
  protocol:
    name: tri
    port: 50052
  registry:
    address: nacos://${nacos.address:127.0.0.1}:8848?username=nacos&password=nacos
```

consumer的配置

```shell
dubbo:
  application:
      name: dubbo-demo-consumer
      logger: slf4j
      qos-port: 33333
  registry:
    address: nacos://${nacos.address:127.0.0.1}:8848?username=nacos&password=nacos

```

