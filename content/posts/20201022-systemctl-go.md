---
title: "使用systemctl部署golang项目"
date: 2020-10-22T23:33:49+08:00

tags: ["go"]
categories: ["文档"]

lightgallery: true
---

#### 编写配置文件

```
cd /etc/systemd/system
vi pxtool.service

# pxtool.service
[Unit]
Description=PXApi

[Service]
Type=simple
Restart=always
RestartSec=5s
ExecStart=/root/go/src/gin-target/main
WorkingDirectory=/root/go/src/gin-target

[Install]
WantedBy=multi-user.target
```

每次修改 ExecStart 需要执行`systemctl daemon-reload`

```
# 启动
systemctl start pxtool.service
# 查看状态
systemctl status pxtool.service
# 自启动
systemctl enable pxtool.service
```

#### 常用 service 操作

```
# 自启动
systemctl enable nginx.service

# 禁止自启动
systemctl disable nginx.service

# 启动服务
systemctl start nginx.service

# 停止服务
systemctl stop nginx.service

# 重启服务
systemctl restart nginx.service

# 查看Unit定义文件
systemctl cat nginx.service

# 编辑Unit定义文件
systemctl edit nginx.service

# 重新加载Unit定义文件
systemctl reload nginx.service

# 列出已启动的所有unit，就是已经被加载到内存中
systemctl list-units

# 列出系统已经安装的所有unit，包括那些没有被加载到内存中的unit
systemctl list-unit-files

# 查看服务的日志
journalctl -u nginx.service    # 还可以配合`-b`一起使用，只查看自本次系统启动以来的日志

# 查看所有target下的unit
systemctl list-unit-files --type=target

# 查看默认target，即默认的运行级别。对应于旧的`runlevel`命令
systemctl get-default

# 设置默认的target
systemctl set-default multi-user.target

# 查看某一target下的unit
systemctl list-dependencies multi-user.target

# 切换target，不属于新target的unit都会被停止
systemctl isolate multi-user.target
systemctl poweroff    # 关机
systemctl reboot       # 重启
systemctl rescue    # 进入rescue模式
```

#### 相关文档

[systemctl部署go](https://learnku.com/articles/34025)

####  config
```
# frp
[Unit]
Description=PXApi

[Service]
Type=simple
ExecStart=/home/lib/frp/frps -c /home/lib/frp/frps.ini

[Install]
WantedBy=multi-user.target

```