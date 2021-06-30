# frp设置


#### 服务器配置
```
[common]
bind_port = 7000
dashboard_port = 7001
token=xxxxx
dashboard_user = xxx
dashboard_pwd = xxx
vhost_http_port = 8080
subdomain_host = frp.linpx.cn
```
#### nginx 转发
```
# frp
server{
    listen 80;
    server_name *.frp.linpx.cn;
    location / {
            proxy_pass http://127.0.0.1:8080;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_hide_header X-Powered-By;
    }
    access_log off;
}
```
#### 客户端配置
```
[common]
server_addr = xxx
server_port = 7000
token=xxx

[unraid]
type = http
local_ip = 10.10.10.254
local_port = 80
subdomain = unraid
# unraid.frp.linpx.cn 访问web服务
```

