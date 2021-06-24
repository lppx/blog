# 树莓派使用手册


#### 安装mysql8
```
sudo apt install mariadb-server
sudo mysql
UPDATE user SET password = password('xxxx') WHERE user = 'root';
UPDATE user SET plugin = 'mysql_native_password' WHERE user = 'root';
flush privileges;
```
设置root远程访问
```
vi /etc/mysql/mariadb.conf.d/50-server.cnf
# 注释 port bind-address
sudo systemctl restart mariadb
```
设置账号权限
```
# 重新进入
mysql -u root -p
use mysql;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'xxxx' WITH GRANT OPTION;
```


