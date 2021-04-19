# centos7安装nginx


####  安装nginx

```
sudo yum -y install nginx   # 安装 nginx
sudo yum remove nginx  # 卸载 nginx
```

####  配置nginx

yum安装方式 ，配置文件目录在`/etc/nginx/nginx.conf `

也可以到conf.d文件夹下，新建web.conf写入配置

常用配置

```
server
{
    listen 9001;
    index index.html index.htm index.php;
    root  /var/www/lppx-on-hugo/public;

    #error_page   404   /404.html;
    #include enable-php.conf;

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
    {
        expires      30d;
    }

    location ~ .*\.(js|css)?$
    {
        expires      12h;
    }

    location ~ /\.
    {
        deny all;
    }

    access_log  /www/wwwlogs/access.log;
}

```

####  制作反向代理配置

```
# 
server {
    server_name h.linpx.cn;
    listen 80;

    location / {
        proxy_pass_header Server;
        proxy_redirect off;
        proxy_pass http://127.0.0.1:7017;
    }
}
```



```
sudo systemctl start nginx # 启动 
sudo systemctl enable nginx # 设置开机启动 
sudo service nginx start # 启动 nginx 服务
sudo service nginx stop # 停止 nginx 服务
sudo service nginx restart # 重启 nginx 服务
sudo service nginx reload # 重新加载配置，一般是在修改过 nginx 配置文件时使用。

```


