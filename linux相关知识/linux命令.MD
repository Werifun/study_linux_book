# linux常用命令：

## 基础命令：

cpu:

1. lscpu: display information about the CPU architecture (结构)

## 进程:

1. nohup: run a command immune(免于) to hangups, with output to a non-tty(tty??)

注：tty(终端设备的统称):tty一词源于Teletypes，或teletypewriters，原来指的是电传打字机，是通过串行线用打印机键盘通过阅读和发送信息的东西，后来这东西被键盘和显示器取代，所以现在叫终端比较合适。

2. ps: report a snapshot(快照) of the current processes 

3. kill 
4. top 
5. killall 
6. read
7. wait
8. sleep 
9. source 

## 存储:

1. df 

2. du 

3. lsblk 

4. fdisk 

5. mount 

6. unmount 

7. mkfs 

8. iostat

## 文件：

1. touch 
2.  cat 
3. cut 
4. sort 
5. uniq 
6. more 
7. less 
8. head 
9.  tail 
10.  rm 
11. ls
12. chown 
13. chmod 
14. find 
15. locate 
16. scp 
17. tar 
18. zip 
19. tr 
20. wc 
21. chgrp



## 内存：

1. free

## 网络：

1. wget 

2. curl 

3. ping 

4. ip 
5. ipconfig 
6. dig 
7. hostname 
8. netstat 
9. trace 
10.  traceroute 
11. ifup/ifdown 
12. nslookup 
13. iptables 
14. tshark 
15. ssh 
16. telnet 

## 用户管理：

1. useradd 

2. userdel 

3. passwd 

4. usermod 

5. groupadd 

6. groupdel 

7. groupmod

## 其他：

1. whereis 

2. which 

3. who/whoami 

4. whatis 

5. yun 

6. rpm 

7. crontab 

8. awk 

9. sed 

10. grep 

11. alias 

12. echo 

13. exec 

14. xargs 

15. su/sudo 

16. export 

17. set

 

## [linux服务管理](https://www.cnblogs.com/shijingjing07/p/9301590.html)

18.service: service命令其实是去/etc/init.d目录下，去执行相关程序

```shell
eg.
# service命令启动redis脚本
service redis start
# 直接启动redis脚本
/etc/init.d/redis start
# 开机自启动
update-rc.d redis defaults

```

19. systemctl: systemd是Linux系统最新的初始化系统(init),作用是提高系统的启动速度，尽可能启动较少的进程，尽可能更多进程并发启动。systemd对应的进程管理命令是systemctl

```shell
systemctl redis start  # 启动某个进程
systemctl redis stop
systemctl enable redis  # 开机自启动
```

