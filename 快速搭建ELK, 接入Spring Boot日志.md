### 架构
![](https://cdn.nlark.com/yuque/0/2023/png/26411187/1703257418498-57bce12e-dbeb-4e75-9e34-4cfb93700024.png)



### **<font style="color:rgb(0, 0, 0);">docker-compose.yml</font>**
```xml
version: '3'
services:
  elasticsearch:
    image: elasticsearch:7.5.1
    container_name: elasticsearch
    environment:
      - "cluster.name=elasticsearch" #设置集群名称为elasticsearch
      - "discovery.type=single-node" #以单一节点模式启动
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m" #设置使用jvm内存大小
    volumes:
      - /usr/local/docker/elk/elasticsearch/plugins:/usr/share/elasticsearch/plugins #插件文件挂载
      - /usr/local/docker/elk/elasticsearch/data:/usr/share/elasticsearch/data #数据文件挂载
    ports:
      - 9200:9200
  kibana:
    image: kibana:7.5.1
    container_name: kibana
    depends_on:
      - elasticsearch #kibana在elasticsearch启动之后再启动
    environment:
      - ELASTICSEARCH_URL=http://elasticsearch:9200 #设置访问elasticsearch的地址
    ports:
      - 5601:5601
  logstash:
    image: logstash:7.5.1
    container_name: logstash
    volumes:
      - /usr/local/docker/elk/logstash/logstash-springboot.conf:/usr/share/logstash/pipeline/logstash.conf #挂载logstash的配置文件
    depends_on:
      - elasticsearch #kibana在elasticsearch启动之后再启动
    links:
      - elasticsearch:es #可以用es这个域名访问elasticsearch服务
    ports:
      - 4560:4560
```



### logstash配置文件
```xml
input {
  tcp {
    mode => "server"
    host => "0.0.0.0"
    port => 4560
    codec => json_lines
  }
}
output {
  elasticsearch {
    hosts => "es:9200"
    index => "springboot-logstash-%{+YYYY.MM.dd}"
  }
}
```



### 启动运行ELK
docker-compose up -d



### SpringBoot 整合
```xml
<dependency>
	<groupId>net.logstash.logback</groupId>
  <artifactId>logstash-logback-encoder</artifactId>
  <version>5.2</version>
</dependency>
```



配置logback的appender

```xml
<!--输出到logstash的appender-->
    <appender name="LOGSTASH" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
        <!--可以访问的logstash日志收集端口-->
        <destination>localhost:4560</destination>
        <encoder charset="UTF-8" class="net.logstash.logback.encoder.LogstashEncoder"/>
    </appender>
```

