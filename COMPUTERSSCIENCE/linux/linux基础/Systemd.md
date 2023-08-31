# Systemd



## 入门

Linux操作系统的开机过程是这样的，即从BIOS开始，然后进入Boot Loader，再加载系统内核，然后内核进行初始化，最后启动初始化进程。初始化进程作为Linux系统的第一个进程，它需要完成Linux系统中相关的初始化工作，为用户提供合适的工作环境。现在很多发行版系统已经替换掉了熟悉的初始化进程服务System V init，正式采用全新的systemd初始化进程服务。systemd初始化进程服务采用了并发启动机制，开机速度得到了不小的提升。



### 核心概念：`unit`

`unit`表示不同类型的`systemd`对象，通过配置文件进行标识和配置；文件中主要包含了系统服务、监听`socket`、保存的系统快照以及其它与`init`相关的信息



### 配置文件

* `/usr/lib/systemd/system`：每个服务最主要的启动脚本，类似于之前的`/etc/init.d`
* `/run/systemd/system`：系统执行过程中所产生的服务脚本，比上面目录优先运行
* `/etc/systemd/system`：管理员建立的执行脚本，类似于`/etc/rc.d/rcN.d/Sxx`类的功能，比上面目录优先运行