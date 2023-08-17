# ssh登录



#### 远程登录服务器

```bash
ssh user@hostname
```



#### 默认登录端口号为22。如果想登录某一特定端口

```bash
ssh user@hostname -p 22
```



#### 配置文件

创建文件`~/.ssh/config`

```bash
Host myserver1
	HostName IP地址或域名
	User 用户名
	
Host myserver2
	HostName IP地址或域名
	User 用户名
```

之后再使用服务器时，可以直接使用别名`myserver1`、`myserver2`。



# 密钥登录

#### 创建密钥

```bash
ssh-keygen
```

执行结束后，`~/.ssh/`目录下会多两个文件：

* `id_rsa`：私钥
* `id_rsa.pub`：公钥

想免密登录哪个服务器，就将公匙传给哪个服务器即可。

* 可以手动将公钥复制到服务器中`~/.ssh/authorized_keys`文件里即可。
* 也可使用如下命令`ssh-copy-id servername`



#### 执行命令

```bash
ssh user@hostname command
```







# scp传文件



命令格式

```bash
scp source destination
```

将`source`路径下的文件复制到`destination`中

可以一次复制多个文件

```bash
scp source1 source2 destination
```

复制文件夹

```bash
scp -r source_dir destination
```

指定服务器端口号

```bash
scp -P number source1 source2 destination
```

