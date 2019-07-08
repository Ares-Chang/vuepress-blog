# nginx服务器配置+二级域名搭建项目
## [安装nginx](https://nginx.org/en/linux_packages.html#Ubuntu)
- 安装前提
```bash
sudo apt install curl gnupg2 ca-certificates lsb-release
```
- 为稳定版的nginx设置apt仓库，运行下面命令
```bash
echo "deb http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```
- 用主分支的nginx包，则运行一下命令
```bash
echo "deb http://nginx.org/packages/mainline/ubuntu `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```
- 下一步，导入官方nginx签名证书，为了让apt验证包的真实性
```bash
curl -fsSL https://nginx.org/keys/nginx_signing.key | sudo apt-key add -
```
- 确认你现在用的正确的密钥
```bash
sudo apt-key fingerprint ABF5BD827BD9BF62
```
- 输出应该包含完整的指纹
```bash
pub   rsa2048 2011-08-19 [SC] [expires: 2024-06-14]
      573B FD6B 3D8F BC64 1079  A6AB ABF5 BD82 7BD9 BF62
uid   [ unknown] nginx signing key <signing-key@nginx.com>
```
- 去安装nginx
```bash
sudo apt update
sudo apt install nginx
```

## 配置nginx
- Nginx安装结束后，yum默认安装位置在/etc/nginx中。配置文件位于：/etc/nginx/nginx.conf，可以修改处理器数量、日志路径、pid文件路径等，默认的日志。
- nginx.conf末尾的 include /etc/nginx/conf.d/.conf，意思是把用户自己的配置放到conf.d/*。修改默认的server配置，包括监听端口listen和文件目录root
```bash
$ vi /etc/nginx/conf.d/default.conf
```
将自己的项目配置放入，80端口监听blog项目的7777端口

多个服务并存
```bash
server
        {
                listen  80;
                server_name tack-out.qiufeihong.top;
                location / {
                proxy_set_header Host $http_host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_pass http://127.0.0.1:1234;
                }

        }
server
        {
                listen  80;
                server_name www.qiufeihong.top;
                location / {
                proxy_set_header Host $http_host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_pass http://127.0.0.1:7777;
                }

        }
server
        {
                listen  80;
                server_name jenkins.qiufeihong.top;
                location / {
                proxy_set_header Host $http_host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_pass http://127.0.0.1:1314;
                }

        }
server
        {
                listen  80;
                server_name resume.qiufeihong.top;
                location / {
                proxy_set_header Host $http_host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_pass http://127.0.0.1:2019;
                }

        }
server
        {
                listen  80;
                server_name express-mongodb-tack-out.qiufeihong.top;
                location / {
                proxy_set_header Host $http_host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_pass http://127.0.0.1:7979;
                }

        }
```
标签|用法
--|--
$http_host和$remote_addr|这里的$http_host和$remote_addr都是nginx的导出变量，可以再配置文件中直接使用。如果Host请求头部没有出现在请求头中，则$http_host值为空，但是$host值为主域名。因此，一般而言，会用$host代替$http_host变量，从而避免http请求中丢失Host头部的情况下Host不被重写的失误。
proxy_set_header X-Real-IP $remote_addr|将$remote_addr的值放进变量X-Real-IP中，此变量名可变，$remote_addr的值为客户端的ip
proxy_pass|只需要提供域名或ip地址和端口。可以理解为端口转发，可以是tcp端口，也可以是udp端口
- 修改nginx配置后，重新启动nginx
```bash
/etc/init.d/nginx reload
```
出现如下，成功

> [ ok ] Reloading nginx configuration (via systemctl): nginx.service.
- 每个服务匹配一个server,加服务就加server即可
## 添加二级域名
进入阿里云-域名-解析

- 添加主机记录（二级域名）

主机记录就是域名前缀，常见用法有：

标签|解析
--|--
www|解析后的域名为www.aliyun.com
@|直接解析主域名 aliyun.com
*|泛解析，匹配其他所有域名 *.aliyun.com
mail|将域名解析为mail.aliyun.com，通常用于解析邮箱服务器
二级域名|如：abc.aliyun.com，填写abc
手机网站|如：m.aliyun.com，填写m
显性URL|不支持泛解析（泛解析：将所有子域名解析到同一地址

记录值：写上ip地址

最后`确定`
- 添加记录

`blog`就是添加的记录
![avatar](../public/aliyun1.png)
![avatar](../public/aliyun2.png)
## nginx+https

### 将sites-enabled导入到nginx.conf

``` {19}
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

sites-available

```
##
# You should look at the following URL's in order to grasp a solid understanding
# of Nginx configuration files in order to fully unleash the power of Nginx.
# http://wiki.nginx.org/Pitfalls
# http://wiki.nginx.org/QuickStart
# http://wiki.nginx.org/Configuration
#
# Generally, you will want to move this file somewhere, and start with a clean
# file but keep this around for reference. Or just disable in sites-enabled.
#
# Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
##

# Default server configuration
#
server {
        listen 443 ssl;

        # SSL configuration
        #
        # listen 443 ssl default_server;
        # listen [::]:443 ssl default_server;
        #
        # Note: You should disable gzip for SSL traffic.
        # See: https://bugs.debian.org/773332
        #
        # Read up on ssl_ciphers to ensure a secure configuration.
        # See: https://bugs.debian.org/765782
        #
        # Self signed certs generated by the ssl-cert package
        # Don't use them in a production server!
        #
        # include snippets/snakeoil.conf;

        ssl_certificate /etc/nginx/ssl/server.crt;
        ssl_certificate_key /etc/nginx/ssl/server.key;
        server_name _;


        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
            proxy_pass http://127.0.0.1:5001;
            proxy_set_header X-Forwarded-For $remote_addr;
        }


        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #       include snippets/fastcgi-php.conf;
        #
        #       # With php7.0-cgi alone:
        #       fastcgi_pass 127.0.0.1:9000;
        #       # With php7.0-fpm:
        #       fastcgi_pass unix:/run/php/php7.0-fpm.sock;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #       deny all;
        #}
}


# Virtual Host configuration for example.com
#
# You can move that to a different file under sites-available/ and symlink that
# to sites-enabled/ to enable it.
#
#server {
#       listen 80;
#       listen [::]:80;
#
#       server_name example.com;
#
#       root /var/www/example.com;
#       index index.html;
#
#       location / {
#               try_files $uri $uri/ =404;
#       }
#}
~              

```

sites-enabled 

```
##
# You should look at the following URL's in order to grasp a solid understanding
# of Nginx configuration files in order to fully unleash the power of Nginx.
# http://wiki.nginx.org/Pitfalls
# http://wiki.nginx.org/QuickStart
# http://wiki.nginx.org/Configuration
#
# Generally, you will want to move this file somewhere, and start with a clean
# file but keep this around for reference. Or just disable in sites-enabled.
#
# Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
##

# Default server configuration
#
server {
        listen 443 ssl;

        # SSL configuration
        #
        # listen 443 ssl default_server;
        # listen [::]:443 ssl default_server;
        #
        # Note: You should disable gzip for SSL traffic.
        # See: https://bugs.debian.org/773332
        #
        # Read up on ssl_ciphers to ensure a secure configuration.
        # See: https://bugs.debian.org/765782
        #
        # Self signed certs generated by the ssl-cert package
        # Don't use them in a production server!
        #
        # include snippets/snakeoil.conf;

        ssl_certificate /etc/nginx/ssl/server.crt;
        ssl_certificate_key /etc/nginx/ssl/server.key;
        server_name _;


        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
            proxy_pass http://127.0.0.1:5001;
            proxy_set_header X-Forwarded-For $remote_addr;
        }


        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #       include snippets/fastcgi-php.conf;
        #
        #       # With php7.0-cgi alone:
        #       fastcgi_pass 127.0.0.1:9000;
        #       # With php7.0-fpm:
        #       fastcgi_pass unix:/run/php/php7.0-fpm.sock;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #       deny all;
        #}
}


# Virtual Host configuration for example.com
#
# You can move that to a different file under sites-available/ and symlink that
# to sites-enabled/ to enable it.
#
#server {
#       listen 80;
#       listen [::]:80;
#
#       server_name example.com;
#
#       root /var/www/example.com;
#       index index.html;
#
#       location / {
#               try_files $uri $uri/ =404;
#       }
#}
               
```
nginx项目结构
```
gushenxing@node6:/etc/nginx$ ls
conf.d          koi-win     nginx.conf       sites-enabled  win-utf
fastcgi_params  mime.types  scgi_params      ssl
koi-utf         modules     sites-available  uwsgi_params
```

## 参考文献

[nginx服务器简单配置文件路径](https://blog.csdn.net/haoaiqian/article/details/78961998)

[nginx: Linux packages](https://nginx.org/en/linux_packages.html#Ubuntu)