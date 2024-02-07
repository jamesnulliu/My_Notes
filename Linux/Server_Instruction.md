# Server Instruction

## 密码相关事宜

Root 密码: shu2024ajhgf

其他用户的初始密码 shu202401

如果账户有问题, 请咨询 jamesnulliu@outlook.com .

super user 能够直接修改其他用户的密码, 而普通用户只能修改自己的密码;  
执行以下命令修改密码:

```bash
passwd your_user_name
```

## 如果想重装系统了....

### 物理重装

去 1 楼机房, 带好重装系统的 biaos 启动盘 (建议装带 GUI 的 Desktop 系统)

安装前进入 safe graphic 模式, 先把所有用户 "/home" 下的文件夹移动到 4T 的硬盘中 (硬盘目录自己找一下, 在 "/" 下面, 如果没找到就重启).

观察当前系统是如何挂载分区的, 重装的时候按照当前的挂载方式分配磁盘分区 (不会挂载分区的话, 先在自己电脑上装两遍 linux 练练手; 注意不要把分区挂载到硬盘上, 一定要在 ssd)

安装系统, 建议开 GUI, (因为不开 GUI 不好操作), 装完到时候把 GUI 关了即可...

### 系统装完后

**Step 1: 安装 nvidia 的驱动和 cuda toolkit**

命令行安装我没成功过. GUI 的话, 按一下 windows 键, 在弹出的界面里找 hardware drive 那个应用, 应该是芯片图标. 在硬件驱动安装选项中, 选择 525 (非 server); 2024 年 1 月 22 日, 535 我实测还是有点小 bug 的.

用以下命令查看驱动是否安装完成:

```bash
nvidia-smi
```

然后 google 一下 cuda toolkit archieve. 下载和 525 版本对应的 cuda toolkit.  
我个人建议下载 runfile, 最简单直观. 安装时记得把额

**Step 2: 迁移用户文件**

先创建用户, 登录一次后, 再把他们的文件从硬盘中移动到 "/home/target_user_name" 下. 切记不要初次登录前就移动文件, 可能会被覆盖.

如果没时间等着移动数据, 下文启动 ssh server 后会介绍挂后台移动的方式.



安装启动 ssh server:

```bash
sudo apt update
sudo apt install openssh-server
sudo ufw allow ssh
```

查看服务器是否在运行 (ctrl+z 退出):
```bash
service sshd status
```

查看服务器的 ipv4 地址:
```bash
ip addr
```

更改服务器端口:
```bash
sudo vi /etc/ssh/sshd_config
```

在 `#Port 22` 下面新开一行, 写端口, 例如:
```bash
#Port 22
Port 1145
```

搞完在你的电脑上测试连接, 假设服务器 ip 为 1.1.1.1, 端口 51419, 登录前面创建的某个用户:

```bash
ssh user_name@1.1.1.1:1145
```

关于如何后台把其他用户的文件从 4T 硬盘中移动回用户文件夹中, 首先进入 super user 模式:

```bash
sudo su
```

先要找到 4T 硬盘的路径, 应该在 "/media" 下. 然后找到对应用户文件夹.

然后执行类似下面的指令:

```bash
cd ~  # 挂载任务的 log 文件会放在你的用户文件夹内
nohup mv /media/disk_path/Tom/* /home/Tom &
```

把返回的进程号记下来, 同时关注生成的 nohup.out 文件; 然后你就可以下线了.

记得别把服务器关了或者 reboot 了.

过一段时间上来看看进程是否还在跑, 没跑说明移动完了.

然后关闭图形化界面:

```bash
systemctl set-default multi-user.target
sudo reboot
```

如果搞崩了想启用图形化界面...:

```bash
systemctl set-default graphical.target
sudo reboot
```