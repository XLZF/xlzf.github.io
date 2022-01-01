---
title: Nginx+Net6 Api 负载均衡示例
date: 2022-01-01 21:23:15
tags: [Nginx]
excerpt: Nginx+Net6 负载均衡示例
categories: Nginx
index_img: https://gitee.com/xlzf/blog-image/raw/master/Home/20220101213220.jpeg
---

# Nginx 负载均衡配置

![](https://gitee.com/xlzf/blog-image/raw/master/Home/20220101213220.jpeg)

## 下载 [nginx: download](http://nginx.org/en/download.html)

![image-20220101205520096](https://gitee.com/xlzf/blog-image/raw/master/Home/20220101205536.png)

## 配置Conf

``` sh

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

    #gzip  on;
	
	# 配置多个地址
    upstream ServiceInstance{
        server localhost:5726;
        server localhost:5727;
        server localhost:5728;
    }

    server {
    	# 监听端口 8080
        listen       8080;
        
        # 监听本地 可替换成IP
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            # root   html;
            # index  index.html index.htm;
            
            # 指向配置
            proxy_pass http://ServiceInstance;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```



## 创建应用



![image-20220101205640901](https://gitee.com/xlzf/blog-image/raw/master/Home/20220101205643.png)

![image-20220101205703680](https://gitee.com/xlzf/blog-image/raw/master/Home/20220101205705.png)

![image-20220101205718852](https://gitee.com/xlzf/blog-image/raw/master/Home/20220101205720.png)

![image-20220101205813853](https://gitee.com/xlzf/blog-image/raw/master/Home/20220101205815.png)

## 执行多个应用

![image-20220101210214117](https://gitee.com/xlzf/blog-image/raw/master/Home/20220101210216.png)

执行:

``` shell
dotnet run urls="http://*:5726"
dotnet run urls="http://*:5727"
dotnet run urls="http://*:5728"
```



## 启动Nginx

``` shell
D:\nginx-1.18.0\nginx-1.18.0>start nginx
```



## 停止Nginx

``` shell
D:\nginx-1.18.0\nginx-1.18.0>nginx.exe -s stop
```



## 访问流程

![访问流程](https://gitee.com/xlzf/blog-image/raw/master/Home/20220101211445.png)

## 访问结果

![image-20220101211704858](https://gitee.com/xlzf/blog-image/raw/master/Home/20220101211706.png)
