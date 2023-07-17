# Linux用户及权限

### 用户

Unix 和 Linux 的设计初衷是多⽤户操作系统

⼀台计算机在同⼀时间可以由 多个⽤户 使⽤，多个⽤户共同享⽤系统的全部硬件和软件资源

#### 创建

```shell
adduser xxx

passwd xxx

useradd -m -d name /home/name

// 超级用户创建

useradd -o -u 0 -g 0 -M -d /root -s /bin/bash admin

useradd -o -u 0 -g 0   lifanping
```

#### 删除

sudo userdel -r newuser

#### 密码

passwd  usename

#### 其他常用命令

##### 1、创建的用户添加到sudo的配置文件中

切换到root用户 运行vi sudo：

```shell
su root

vi sudo
```

在`root    ALL=(ALL:ALL) ALL`，下面添加一行` lifanping ALL=(ALL:ALL) ALL lifanping`为用户名

保存并退出配置文件

##### 2、用户查看

vi /etc/group

vi /etc/passwd

##### 3、usermod命令

功能：修改用户帐号，包含添加用户的所属组、重命名用户名、锁定用户、修改帐号的有效期限等。

将 newuser添加到组root中 # usermod -G root newuser

修改 newuser 的用户名为 newuser1 # usermod -l newuser1 newuser

锁定账号 newuser1 # usermod -L newuser1

解除对 newuser1 的锁定 # usermod -U newuser1

增加sudo权限 #usermod -a -G sudo newuser1

### 文件权限

修改目录的用户
chown -R deployer:deployer my-dir

修改目录的用户组
sudo chgrp -R deployer my-dir

修改目录的权限
chmod 775 my-dir



