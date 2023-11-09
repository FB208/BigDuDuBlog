---
{"dg-publish":true,"permalink":"/01-编程技术/【tomcat】与【nginx】配置-持续更新/","dgPassFrontmatter":true}
---

#nginx  #tomcat

> 以下所有测试均基于nginx-1.21.5-windows版



### 发布前端(VUE)项目，启用gzip压缩

1. tomcat<br />修改server.xml

```xml
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

改成
```xml
<Connector port=”8080” protocol=”HTTP/1.1”
           
           connectionTimeout="20000"
           redirectPort="8443" compression="on" compressionMinSize="2048"   
           noCompressionUserAgents="gozilla, traviata" compressableMimeType="text/html,text/xml,application/javascript,text/css,text/plain,text/json" useSendfile = "false"
/>
```

2. nginx<br />修改nginx.conf，在http节点下添加如下配置

```nginx
   # 开启gzip
   gzip on;
   # 启用gzip压缩的最小文件；小于设置值的文件将不会被压缩
   gzip_min_length 1k;
   # gzip 压缩级别 1-10 
   gzip_comp_level 2;
   # 进行压缩的文件类型。
   gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;

   # 是否在http header中添加Vary: Accept-Encoding，建议开启
   gzip_vary on;
   include       mime.types;
   default_type  application/octet-stream;


   sendfile        on;

   keepalive_timeout  65;
```


### 代理非80端口，在url中隐藏端口号

利用反向代理的原理，首先80端口和443端口必须让出来给nginx

配置两个域名解析
![image.png](/img/user/assets/1650877254629-bfe523a2-654e-4d25-9c44-02ac44a22606.png)
修改nginx配置，在http节点下添加多个server节点
```nginx
    server {
        listen 8081;
        location /{
            root   html/test2;
            index  index.html index.htm;
                
            add_header Cache-Control no-cache;
            add_header Access-Control-Allow-Origin *;
        }
     }    
    server {
        listen 80 default_server;
        server_name test1.ha-iot.com;     
        location /{
            root   html/test1;
            index  index.html index.htm;
                
            add_header Cache-Control no-cache;
            add_header Access-Control-Allow-Origin *;
        }
     }
    server {
        listen 80;
        server_name test2.ha-iot.com; 
        location /{

            proxy_pass http://ip:8081;
        }
     }

```

#### 代理443端口（https）
原理与代理80相同，需要注意以下几点：

1. 443 default_server配置的域名A要有ssl证书，且在default_server处配置。
2. 映射的域名B也要有ssl证书，在原代理处和映射配置处都要配置

```json
#代理6012是真正的服务地址    
server {
        listen       6012 ssl;
        server_name file.ha-iot.com; #修改为申请证书绑定的域名
		 
		ssl_certificate   E://path//B.com.pem;
        ssl_certificate_key  ://path//B.com.key;
		ssl_session_timeout 5m;
		ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
		ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
		ssl_prefer_server_ciphers on;

        location / {
			      root   E:\\demo;
            index  index.html index.htm;
            add_header Cache-Control no-cache;
            add_header Access-Control-Allow-Origin *;
        }
		
        error_page   500 502 503 504  /50x.html;

       
    }

#默认443入口
    server {
        listen 443 default_server;
        server_name ssl39.ha-iot.com; 
        
        ssl_certificate   E://A.com.pem;
        ssl_certificate_key  E://A.ha-iot.com.key;
		ssl_session_timeout 5m;
		ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
		ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
		ssl_prefer_server_ciphers on;
       
        location /{

            root   html/test1;
            index  index.html index.htm;
                
            add_header Cache-Control no-cache;
            add_header Access-Control-Allow-Origin *;
        }
    }
#配置映射
    server {
        listen 443 ssl;
        server_name file.ha-iot.com; 
        ssl_certificate   E://B.com.pem;
        ssl_certificate_key  E:/B.com.key;
		ssl_session_timeout 5m;
		ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
		ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
		ssl_prefer_server_ciphers on;
        location /{
            proxy_pass https://localhost:6012;
        }
    }
```

### 用Nginx实现socket高可用
添加stream节点，与http节点平级
```nginx
stream {
    # 负载均衡地址
    upstream use_transfer {
        server 192.168.18.170:2011;
        server 192.168.18.170:2012;
    }
    # 监听端口2004并将socket链接转至use_transfer
    server {
        listen 2004;
        proxy_pass use_transfer;
    }
}
```
收到socket链接默认平均分配，一边一个，当其中某一个地址链接不上是会自动将所有链接转发到另一个可用地址


### 配置ssl（支持https）
推荐将证书放到nginx文件夹cert下
```json
server {
        listen       6012 ssl;
        server_name file.ha-iot.com; #修改为申请证书绑定的域名

        ssl_certificate   E://path//B.com.pem;
        ssl_certificate_key  ://path//B.com.key;
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;

        location / {
			      root   E:\\demo;
            index  index.html index.htm;
            add_header Cache-Control no-cache;
            add_header Access-Control-Allow-Origin *;
        }
		
        error_page   500 502 503 504  /50x.html;

       
    }
```

# 启用ipv6
``` shell
# 查看是否安装了ipv6模块
nginx -V
# 从数据内容中找【–with-ipv6】，没有的话说明没装过

# 安装ipv6模块

```