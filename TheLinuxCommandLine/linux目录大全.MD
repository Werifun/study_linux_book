## 目录与应放置文件内容

- /bin : /usr/bin: __可执行二进制文件__的目录，如常用的命令ls、tar、mv、cat等。

  

- /boot : 放置linux__系统启动__时用到的一些文件。/boot/vmlinuz 为 linux 的内核文件，以及 /boot/gurb。建议单独分区，分区大小100M即可

   

- /dev：存放linux系统下的设备文件，访问该目录下某个文件，相当于访问某个设备，常用的是挂载光驱 mount /dev/cdrom /mnt。

-  在Linux系统上，任何装置与接口__设备__都是以文件的型态存在于这个目录当中的。你只要透过存取这个目录底下的某个文件，就等于存取某个装置啰～比要重要的文件有/dev/null, /dev/zero, /dev/tty, /dev/lp, /dev/hd, /dev/sd*等等 

- /etc: __系统配置文件__存放的目录，不建议在此目录下存放可执行文件，重要的配置文件有 /etc/inittab、 /etc/fstab、 /etc/init.d、 /etc/X11、 /etc/sysconfig、 /etc/xinetd.d修改配置文件之前记得备份。

  - /etc/apt: Ubuntu(debian)使用`apt`来管理软件包，`apt`可以将软件库存储在如下文件中：

    - `/etc/apt/sources.list`和`/etc/apt/sources.list.d/`目录中带.list后缀的文件中。可以使用命令`man sources.list`来查看`apt`的完整存储机制。通过编辑这些文件，我们可以添加、删除、或者临时关闭某些软件库。

      > 注意：在更改上述文件之前，最好先备份一下。例如， sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup

    - 在安装完debian操作系统之后大家做的第一件事大概就是修改source.list文件了吧，否则你是无法在线更新软件的，那么source.list文件中的各个配置项的具体含义你搞懂了么？下面就以我的source.list文件为例为大家讲解一下。

      ```shell
      deb http://mirrors.163.com/debian/ wheezy main non-free contrib
      deb http://mirrors.163.com/debian/ wheezy-proposed-updates main non-free contrib
      deb-src http://mirrors.163.com/debian/ wheezy main non-free contrib
      deb-src http://mirrors.163.com/debian/ wheezy-proposed-updates main non-free contrib
      ```

      其中可以把每一行分为四个部分,说白了，当你在线更新某个软件时，debian就是遵照这四个选项（准确的说是后三项,具体释义见参考网页）的指示找到软件来给你安装的：

      ```shell
      deb    ###   ftp地址                         ###  版本代号   ###  限定词
      deb    ###   http://mirrors.163.com/debian/ ###  wheezy    ###   main non-free contrib
      ```

  - /etc/X11 存放与 x windows 有关的设置。

- /home：系统默认的用户家目录，新增用户账号时，用户的家目录都存放在此目录下，~表示当前用户的家目录，~edu 表示用户 edu 的家目录。建议单独分区，并设置较大的磁盘空间，方便用户存放数据

  

- /lib: /usr/lib: /usr/local/lib：系统使用的__函数库__的目录，程序在执行过程中，需要调用一些额外的参数时需要函数库的协助，比较重要的目录为 /lib/modules。

  - 系统的函式库非常的多，而/lib放置的则是在开机时会用到的函式库，以及在/bin或/sbin底下的指令会呼叫的函式库而已。__什么是函式库呢？__妳可以将他想成是『外挂』，某些指令必须要有这些『外挂』才能够顺利完成程序的执行之意。尤其重要的是/lib/modules/这个目录，因为该目录会放置核心相关的模块(驱动程序)喔！

- /lost+fount：系统异常产生错误时，会将一些遗失的片段放置于此目录下，通常这个目录会自动出现在装置目录下。如加载硬盘于 /disk 中，此目录下就会自动产生目录 /disk/lost+found

- /mnt: /media：光盘默认挂载点，通常光盘挂载于 /mnt/cdrom 下，也不一定，可以选择任意位置进行挂载。

- /opt：给主机__额外安装软件(第三方软件)__所摆放的目录。如：FC4使用的Fedora 社群开发软件，如果想要自行安装新的 KDE 桌面软件，可以将该软件安装在该目录下。以前的 Linux 系统中，习惯放置在 /usr/local 目录下。

- /root：系统管理员root的家目录，系统第一个启动的分区为 /，所以最好将 /root和 /放置在一个分区下。

- /sbin: /usr/sbin: /usr/local/sbin：放置__系统管理员__(??)使用的可执行命令，如fdisk、shutdown、mount 等。与 /bin 不同的是，这几个目录是给系统管理员 root使用的命令，一般用户只能"查看"而不能设置和使用。

- /srv：服务启动之后需要访问的数据目录，如 www 服务需要访问的网页数据存放在 /srv/www 内。

- /tmp：一般用户或正在执行的程序临时存放文件的目录,任何人都可以访问,重要数据不可放置在此目录下

- /proc：__此目录的数据都在内存中__，如系统核心，外部设备，网络状态，由于数据都存放于内存中，所以不占用磁盘空间，比较重要的目录有 /proc/cpuinfo、/proc/interrupts、/proc/dma、/proc/ioports、/proc/net/* 等。

- /usr：应用程序存放目录，/usr/bin 存放应用程序，/usr/share 存放共享数据，/usr/lib 存放不能直接运行的，却是许多程序运行所必需的一些函数库文件。/usr/local: 存放软件升级包。/usr/share/doc: 系统说明文件存放目录。/usr/share/man: 程序说明文件存放目录，使用 man ls 时会查询 /usr/share/man/man1/ls.1.gz 的内容建议单独分区，设置较大的磁盘空间

- /var：放置系统执行过程中经常变化的文件，如随时更改的日志文件 /var/log，/var/log/message：所有的登录文件存放目录，/var/spool/mail：邮件存放的目录，/var/run:程序或服务启动后，其PID存放在该目录下。建议单独分区，设置较大的磁盘空间







grub: 开机管理程序  GNU *GRUB*（GRand Unified Bootloader简称“*GRUB*”）





## 参考

[linux中各个目录的含义和作用](<https://blog.csdn.net/weixin_42595331/article/details/90311261>)

[Linux 下各个目录的作用及内容](<https://www.cnblogs.com/sytfyf/p/6364691.html>)

[详解Ubuntu的sources.list文件](<https://blog.csdn.net/u012843189/article/details/80964287>)

[debian软件源source.list文件格式说明](https://www.cnblogs.com/beanmoon/p/3387652.html)