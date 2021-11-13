# 获取电信宽带超级密码 兆能ZN600、ZN601


#### 
```
准备工具：
一台电脑
一个U盘（已提前插入天翼网关）
首先打开天翼网关后台
192.168.1.1
使用天翼网关后面的普通管理员账号进入系统


然后选择已提前插入天翼网关的U盘

登录光猫点存储然后跳到存储设置
浏览器F12然后 comsole ，二级菜单选 storage iframe （settings）
在下面打命令输入 get_path_files("/mnt/usb1_1/../..") 转到光猫系统文件夹里面
打开var文件夹，将文件romfile.cfg复制到U盘

username="telecomadmin" web_passwd="telecomadminxxxx"

```



