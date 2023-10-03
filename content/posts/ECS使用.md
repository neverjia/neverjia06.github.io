+++
title = '云服务器操作手册'
date = 2023-10-02T21:15:18+08:00
draft = false
+++
# 一、购买CentOS7.6操作系统，登录

![](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928120406785.png)



## 二、开放安全组2222，修改端口2222在虚拟机里面配置



vi  ssd_config

cat   ssd_config   |  grep   port

#vi  /etc/ssh/sshd_config

#service  sshd   restart 

![image-20220928154759087](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928154759087.png)



# 三、 为该云主机添加一块10G的硬盘，名为disk-姓名全拼，将其挂载到云主机的/mnt目录下，并将etc目录下以.conf结尾的文件拷贝到该目录下，为该磁盘创建一个名为disk-snap的快照，创建快照成功后，将/mnt目录下的内容删除，然后使用快照还原云硬盘的存储数据



挂载到/mnt

挂载-》分区-》格式化-》挂载-》卸载-》

## ①分区，挂载

![image-20220928182007312](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928182007312.png)



## ②格式化，挂载到/mnt目录下

![image-20220928181913447](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928181913447.png)



## ③复制/etc/.conf下的文件到/mnt/目录下

![image-20220928182225085](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928182225085.png)



## ④拍快照

![image-20220928182506015](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928182506015.png)

## ⑤然后删除/mnt/下面的数据，

![image-20220928182545720](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928182545720.png)

## ⑥卸载云磁盘

![image-20220928182659661](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928182659661.png)

## ⑦回滚数据

![image-20220928182927985](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928182927985.png)



## ⑧再将磁盘挂载

![image-20220928183051197](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928183051197.png)

## ⑨在命令行操作

```
#回滚数据以后，发现多了一个磁盘/dev/vdc1
[root@ecs-wuruan-mijia ~]# lsblk
NAME   MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
vda    253:0    0  40G  0 disk 
└─vda1 253:1    0  40G  0 part /
vdc    253:32   0  10G  0 disk 
└─vdc1 253:33   0  10G  0 part 

#查看磁盘挂载情况
[root@ecs-wuruan-mijia ~]# df  -hT
/dev/vdb1      xfs        10G   33M   10G   1% /mnt

#卸载原来的磁盘/dev/vdb1
[root@ecs-wuruan-mijia ~]# umount  /dev/vdb1/

[root@ecs-wuruan-mijia ~]# df  -hT
Filesystem     Type      Size  Used Avail Use% Mounted on
devtmpfs       devtmpfs  486M     0  486M   0% /dev
tmpfs          tmpfs     496M     0  496M   0% /dev/shm
tmpfs          tmpfs     496M  6.8M  489M   2% /run
tmpfs          tmpfs     496M     0  496M   0% /sys/fs/cgroup
/dev/vda1      ext4       40G  2.1G   36G   6% /
tmpfs          tmpfs     100M     0  100M   0% /run/user/0


#挂载回滚以后的磁盘
[root@ecs-wuruan-mijia ~]# mount  /dev/vdc1  /mnt
[root@ecs-wuruan-mijia ~]# df  -hT
Filesystem     Type      Size  Used Avail Use% Mounted on
devtmpfs       devtmpfs  486M     0  486M   0% /dev
tmpfs          tmpfs     496M     0  496M   0% /dev/shm
tmpfs          tmpfs     496M  6.8M  489M   2% /run
tmpfs          tmpfs     496M     0  496M   0% /sys/fs/cgroup
/dev/vda1      ext4       40G  2.1G   36G   6% /
tmpfs          tmpfs     100M     0  100M   0% /run/user/0
/dev/vdc1      xfs        10G   33M   10G   1% /mnt

[root@ecs-wuruan-mijia ~]# ls  /mnt/
asound.conf  libaudit.conf   sestatus.conf
chrony.conf  libuser.conf    sos.conf
dracut.conf  locale.conf     sudo.conf
e2fsck.conf  logrotate.conf  sudo-ldap.conf
GeoIP.conf   man_db.conf     sysctl.conf
host.conf    mke2fs.conf     vconsole.conf
kdump.conf   nsswitch.conf   yum.conf
krb5.conf    resolv.conf
ld.so.conf   rsyslog.conf
[root@ecs-wuruan-mijia ~]# 
```









四、将云服务器的规格变更为2H2G后将系统重装为windows，Server2019；在磁盘管理中找到数据盘，并且将该盘格式化为ntfs

![image-20220928184738448](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928184738448.png)

变更操作系统完成

![image-20220928185516950](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928185516950.png)



登录到Windows

找到磁盘管理-》格式化为ntfs类型

![image-20220928191011421](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928191011421.png)



![image-20220928191045516](C:/Users/never/AppData/Roaming/Typora/typora-user-images/image-20220928191045516.png)

五、 重置云服务器的系统密码为WuRuan12#$

六、 删除以上购买的云服务器、云硬盘、快照、弹性公网IP

diskmgmt.msc