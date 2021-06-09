# unraid使用手册


#### mysql8设置远程访问
```
# 登录
mysql -u root -p
# 查看用户信息
select host,user,plugin,authentication_string from mysql.user;

+-----------+------------------+-----------------------+------------------------------------------------------------------------+
| host      | user             | plugin                | authentication_string                                                  |
+-----------+------------------+-----------------------+------------------------------------------------------------------------+
| %         | root             | caching_sha2_password | $A$005$HF7;krfwhkKHp5fPenQm4J2dm/RJtbbyjtCUVdDCcboXQw3ALxsif/sS1 |
| localhost | mysql.infoschema | caching_sha2_password | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED |
| localhost | mysql.session    | caching_sha2_password | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED |
| localhost | mysql.sys        | caching_sha2_password | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED |
| localhost | root             | mysql_native_password | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9                              |
+-----------+------------------+-----------------------+------------------------------------------------------------------------+

# host为 % 表示不限制ip  plugin 需要修改为 mysql_native_password
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';  ### 123456 mysql的登录密码
flush privileges;
# 重新查看 plugin 已修改为 mysql_native_password
select host,user,plugin,authentication_string from mysql.user;
```

#### centOS7 获取ip地址
```
dhclient eth0
ip addr
```

#### centOS7开启ssh
先检查有没有安装ssh服务：rpm -qa | grep ssh
如果没有安装ssh服务就安装 ： yum install openssh-server
```
vi /etc/ssh/sshd_config
# 开启Port 22、PermitRootLogin YES
systemctl start sshd.service
systemctl enable sshd.service
```