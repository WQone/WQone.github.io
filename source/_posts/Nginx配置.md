---
title: Nginx  配置
categories: Nginx
tags:
  - Nginx
---

## 常用的Nginx 配置

<!--more-->

```bash
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #开启或关闭gzip on off
    gzip on;
    gzip_static on;
    #不使用gzip IE6
    gzip_disable "MSIE [1-6]\.";
    #gzip压缩最小文件大小，超出进行压缩（自行调节）
    gzip_min_length 100k;
    #buffer 不用修改
    gzip_buffers 4 16k;
    #压缩级别:1-10，数字越大压缩的越好，时间也越长
    gzip_comp_level 8;
    #压缩文件类型
    gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
	
    server {
        listen       5000;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root C:\Users\Administrator\Documents\MyProgram\vue-knowledge-programs\dist;
            #判断手机端
            if ($http_user_agent ~* "(mobile|nokia|iphone|ipad|android|samsung|htc|blackberry)") {
                root C:\Users\Administrator\Documents\MyProgram\vue-h5\dist;
            }
            try_files $uri $uri/ /index.html;
        }

        #匹配 api 路由的反向代理到API服务 
        location ^~ /api/ { 
            proxy_pass http://192.168.10.217:8282/;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
    # server {
    #     listen       5002;
    #     server_name  localhost;

	  #     location / {
    #        root   D:\WebyindaTest\dist;
    #       try_files $uri $uri/ /index.html;
    #     }

 	  #     # 匹配 api 路由的反向代理到API服务 
    #     location ^~ /api/ { 
    #         proxy_pass http://localhost:8182/;
    #     }
    # }
}

```