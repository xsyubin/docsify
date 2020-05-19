------


# 中间件WEB服务

# Tomcat
# Jetty
# WAS
# Weblogic
# Nginx

## 安装
/configure --with-http_ssl_module --prefix=/usr/local/nginx

make

make install

groupadd nginx

useradd -M -g nginx -s /sbin/nologin nginx


## 配置 
cd /usr/local/nginx/conf

vim nginx.conf，设置user参数如下：

user nginx nginx


### 开启gzip压缩
```
    gzip on;
    gzip_buffers 32 4K;
    gzip_comp_level 3;
    gzip_min_length 1k;
    gzip_types text/plain application/javascript application/x-javascript text/javascript text/xml text/css;
    gzip_disable "MSIE [1-6]\.";
    gzip_vary on;
```

## 注册服务
vim /lib/systemd/system/nginx.service

chmod 754 /lib/systemd/system/nginx.service

systemctl enable nginx.service

systemctl stop nginx.service
systemctl start nginx.service


systemctl reload nginx.service

## 配置详细

### 错误日志
		access_log  /home/logs/access.log;
	    error_log   /home/logs/error.log;



### php-sock
	    location ~ \.php$ {
            include        fastcgi_params;
            fastcgi_pass   unix:/tmp/php-fcgi.sock;
            fastcgi_index  index.php;
        }

php-fpm -c /usr/local/php7/etc/php.ini


#### 开启gzip

gzip  on;  #开启gzip
gzip_min_length 1k;  #低于1kb的资源不压缩
gzip_comp_level 3; #压缩级别【1-9】，越大压缩率越高，同时消耗cpu资源也越多，建议设置在4左右。
gzip_types text/plain application/javascript application/x-javascript text/javascript text/xml text/css;  #需要压缩哪些响应类型的资源，多个空格隔开。不建议压缩图片，下面会讲为什么。
gzip_disable "MSIE [1-6]\.";  #配置禁用gzip条件，支持正则。此处表示ie6及以下不启用gzip（因为ie低版本不支持）
gzip_vary on;  #是否添加“Vary: Accept-Encoding”响应头


#### 301跳转

		server {
		        listen 80;
		        server_name *.hqytgz.com;
		        return 301 https://$server_name$request_uri;
		}

#### 开启SSL

        ssl on;
        ssl_certificate ssl/1_www.hqytgz.com_bundle.crt;
        ssl_certificate_key ssl/2_www.hqytgz.com.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers  ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers   on;

# Apache
