## 1. 什么是 Nginx?
Nginx 是一个高性能的 HTTP 服务器和反向代理服务器。它最初由 Igor Sysoev 开发，旨在解决 C10K 问题，即在单台服务器上处理 10,000 个并发连接的问题。如今，NGINX 还可以作为负载均衡器、邮件代理服务器和通用 TCP/UDP 代理服务器。

![](https://cdn.nlark.com/yuque/0/2024/png/26411187/1725116572685-5d6963b3-0934-484a-9178-7d04d43ebbda.png)

## 2. Nginx 的基本概念
+ **Web 服务器**: Nginx 可用作静态内容（如 HTML 页面、图片和视频）的 Web 服务器。它处理客户端请求并返回相应的内容。
+ **反向代理**: <font style="color:rgb(31, 35, 40);">是指以代理服务器来接受 internet 上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给 internet 上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。</font>
+ **负载均衡**: Nginx 可以将流量分发到多个后端服务器，以提高系统的性能和可靠性。

## 3. 安装 Nginx
### 3.1 在windows上安装
1. **下载Nginx**:
    - 访问Nginx的官方网站：[https://nginx.org/en/download.html](https://nginx.org/en/download.html)
    - 选择适合Windows的版本下载。通常是一个`.zip`文件。
2. **解压Nginx**:
    - 将下载的`.zip`文件解压到你选择的目录。例如，你可以将其解压到`C:\nginx`。
3. **启动Nginx**:
    - 打开命令提示符（CMD）或PowerShell。
    - 切换到Nginx的目录，例如，如果你将其解压到`C:\nginx`，你可以输入：

```plain
cd C:\nginx
```

    - 运行Nginx：

```plain
start nginx
```

或者直接双击`nginx.exe`文件。

4. **验证Nginx是否运行**:
    - 在浏览器中输入`http://localhost:8080`或`http://127.0.0.1:8080`，如果Nginx正在运行，你应该能看到Nginx的欢迎页面。
5. **配置Nginx**:
    - Nginx的配置文件通常位于Nginx目录下的`conf`文件夹中，名为`nginx.conf`。
    - 你可以编辑这个文件来配置服务器，例如设置端口、添加虚拟主机等。
6. **重启Nginx以应用更改**:
    - 如果你修改了配置文件，需要重启Nginx以使更改生效。在命令行中运行：

```plain
nginx -s reload
```

7. **关闭Nginx**:
    - 如果需要停止Nginx，可以在命令行中运行：

```plain
nginx -s stop
```

请注意，如果你的系统防火墙开启，可能需要在防火墙设置中允许Nginx使用的端口（默认是80和443）。

如果你需要更详细的指导或遇到问题，可以查看Nginx的官方文档或搜索相关的教程。

### 3.2 在Mac上安装
```shell
// 安装
brew install nginx

// 启动nginx
brew services start nginx

// 重启nginx
brew services restart nginx
```



## 4.Nginx基本命令
```shell
nginx -s stop       快速关闭Nginx，可能不保存相关信息，并迅速终止web服务。
nginx -s quit       平稳关闭Nginx，保存相关信息，有安排的结束web服务。
nginx -s reload     因改变了Nginx相关配置，需要重新加载配置而重载。
nginx -s reopen     重新打开日志文件。
nginx -c filename   为 Nginx 指定一个配置文件，来代替缺省的。
nginx -t            不运行，仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。
nginx -v            显示 nginx 的版本。
nginx -V            显示 nginx 的版本，编译器版本和配置参数。
```



## 5. Nginx配置文件介绍
Nginx 的配置文件通常由一系列的块组成，这些块按照层级顺序组织，每个块定义了不同的配置上下文。主要的配置块包括 `http`、`server` 和 `location`，每个块都有其特定的作用和配置指令。以下是对这些配置块的详细介绍：

### 1. `http` 块
`http` 块是配置文件中最高级别的块之一（另一个是 `events`），它定义了 HTTP 服务器的全局设置。在 `http` 块中，你可以设置各种参数，如日志路径、缓存路径、默认的 `server` 和 `location` 行为等。`http` 块可以包含多个 `server` 块。

```nginx
http {
    include       mime.types;
    default_type  application/octet-stream;

    # 日志文件定义
    access_log  /var/log/nginx/access.log;
    error_log   /var/log/nginx/error.log;

    # 其他全局设置...
}
```

### 2. `server` 块
`server` 块定义了虚拟主机的配置，每个 `server` 块代表一个虚拟主机，可以监听一个或多个 IP 地址和端口。在 `server` 块中，你可以设置域名、端口、SSL 证书、日志文件、代理设置等。

```nginx
server {
    listen       80;
    server_name  example.com www.example.com;

    # 代理设置、静态文件处理、重定向等...
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    # 其他 location 块...
}
```

### 3. `location` 块
`location` 块定义了请求的处理方式，它可以根据请求的 URI、命名管道或正则表达式来匹配和处理请求。`location` 块可以存在于 `server` 块中，也可以嵌套在其他 `location` 块中。在 `location` 块中，你可以设置代理、重写规则、静态文件服务、权限控制等。

```nginx
location / {
    # 处理对 / 的请求
}

location = / {
    # 精确匹配 /
}

location ~* \.(jpg|jpeg|png)$ {
    # 处理所有 jpg、jpeg 和 png 图片请求
}

location @fallback {
    # 命名 location，用作错误页面处理
}
```

### 其他重要指令
+ `listen`：指定 `server` 块监听的端口和 IP 地址。
+ `server_name`：定义 `server` 块的域名。
+ `root`：定义服务器的根目录，用于静态文件服务。
+ `index`：定义默认的首页文件。
+ `proxy_pass`：将请求代理到另一个服务器。
+ `try_files`：尝试按顺序查找并返回文件，直到找到为止。
+ `error_page`：定义错误页面的重定向。
+ `rewrite`：重写 URL 规则。

### 示例配置
下面是一个更完整的示例配置，展示了 `http`、`server` 和 `location` 块的组合使用：

```nginx
http {
    include       mime.types;
    default_type  application/octet-stream;

    # 定义日志文件
    access_log  /var/log/nginx/access.log  main;
    error_log   /var/log/nginx/error.log;

    # 定义负载均衡服务器组
    upstream myapp {
        server app1.example.com;
        server app2.example.com;
    }

    server {
        listen       80;
        server_name  example.com www.example.com;

        # 处理根目录请求
        location / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }

        # 处理图片文件
        location ~* \.(jpg|jpeg|png)$ {
            root   /usr/share/nginx/images;
        }

        # 代理到后端应用服务器
        location /api {
            proxy_pass http://myapp;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        # 静态文件服务
        location /static {
            alias /var/www/static/;
        }

        # 处理 404 错误
        error_page 404 /404.html;
        location = /404.html {
            root /usr/share/nginx/html;
        }
    }
}
```

在这个示例中，`http` 块定义了全局设置，`server` 块定义了虚拟主机的配置，而 `location` 块则根据不同的请求路径定义了具体的处理规则。这种结构使得 Nginx 的配置非常灵活和强大。

## 6.HTTP反向代理
```shell
    server {
        listen       80;
        server_name  fengzhu.com;

        location / {
            proxy_pass http://my_server1;
            proxy_set_header Host $host;
        }

        error_log /tmp/error.log warn;
    }

    upstream my_server1 {
        server localhost:8080;
    }

```

这样，访问fengzhu.com的请求都会被转发到后端8080端口上

## 7.负载均衡
![](https://cdn.nlark.com/yuque/0/2024/png/26411187/1725116525658-b4cb41ed-807c-4164-913e-3118ba146ea9.png)

```shell
upstream my_server1 {
        server localhost:8080;
        server localhost:8081;
        server localhost:8082;
    }
```

 

### 负载均衡策略
Nginx 支持多种负载均衡策略，可以在 `upstream` 块中配置以实现不同的请求分发方式。以下是 Nginx 支持的一些常见负载均衡策略：

1. **轮询（round-robin）**：默认的负载均衡方法，将请求依次分配给每个服务器，不考虑服务器的当前负载情况。
2. **权重（weight）**：在轮询的基础上，可以根据服务器的性能或角色分配不同的权重，权重高的服务器将处理更多的请求。
3. **最少连接（least_conn）**：将请求分配给当前活跃连接数最少的服务器，适合处理时间长短不一的请求。
4. **IP哈希（ip_hash）**：通过客户端IP地址进行哈希，确保来自同一IP的请求总是被分配到同一个服务器上，适用于需要保持会话持久性的应用。
5. **公平（fair）**：（需要第三方模块支持）根据服务器的响应时间来分配请求，响应时间越短的服务器优先分配请求。
6. **URL哈希（url_hash）**：（需要第三方模块支持）根据请求的URL进行哈希，使得相同的URL总是被分配到同一个服务器上，适合缓存服务器的场景。

除了上述策略，还可以结合使用 `max_fails`、`fail_timeout` 和 `backup` 等参数来增强负载均衡的健壮性。例如，`max_fails` 可以设置尝试连接后端服务器失败的次数，`fail_timeout` 用于设置在一定时间内失败达到 `max_fails` 次数后，服务器被认为是不可用的，而 `backup` 用于标记某个服务器作为备份服务器，在所有主服务器都不可用时才会被使用。



