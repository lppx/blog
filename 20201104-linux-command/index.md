# linux常用命令


#####  查看端口占用

```bash
netstat -lnp|grep 8000
# 进程详细信息
tcp6       0      0 :::9000                 :::*                    LISTEN      8808/main 
# 查看进程详细信息 kill进程
ps 8808
kill -9 11100
```






