# Nginx模块分为核心模块，基础模块和第三方模块
## 核心模块
  HTTP模块、EVENT模块(事件)、MAIL模快.
## 基础模块
  HTTP Access模块、HTTP FASTCGI模块、HTTP Proxy模块、HTTP Rewrite模快.
## 第三方模块
  HTTP Upstream Request Hash模块、Notice模块、HTTP Access Key模块.
  
## 配置文件
配置文件主要由四部分组成：main(全区设置),server(主机设置),upstream(负载均衡服务器设置),和location(URL匹配特定位置设置)。
### 全局变量
``` nginx
#nginx的worker进程运行用户以及用户组
#user nobody nobody;
#nginx开启的进程数
worker_processes 1;
#worker_processes auto;

```
### 事件配置
``` nginx
events{
 #use [ kqueue | rtsig | epoll | /dev/poll | select | poll]; epoll模型是Linux 2.6以上版本内核中的高性能网络I/O模型,如果泡在FressBSD上面，就用kqueue模型。
 use epoll;
}
```
### http参数
``` nginx
#文件扩展名与文件类型映射表
include mime.types;
#默认文件类型
default_type application/octet-stream;
```
### 虚拟主机基本设置
``` nginx
#虚拟主机定义
server {
  
}
```
### nginx状态监控
``` nginx
location / NginxStatus {
  #启用StubStatus的工作访问状态
  stub_status on;
}

```
### 反向代理
``` nginx
#nginx跟后端服务器连接超时时间(代理连接超时)
proxy_connect_timeout 5;

```
