---
title: ssh相关
date: 2017-07-30 22:57:35
categories:
- Linux-Linux常用技巧
tags:
- Linux
- ssh
---

### 修改ssh默认登录的端口号

编辑配置文件`/etc/ssh/sshd_config`

```
$ vim /etc/ssh/sshd_config
```

找到`Port 22`，然后将端口22改为你想要的端口号，重启SSH服务：

```
/etc/init.d/sshd restart
```

即可。测试：

```
ssh localhost -p 你的端口号
```

如果你希望保险起见，不至于因为一个端口连接不了（比如受到攻击）而不能使用ssh连接，那么你可以使用多个ssh连接端口，还是在配置文件`/etc/ssh/sshd_config`中修改，找到Port 端口号地方，然后在下面添加一行：

```
Port 23
```

这样就又增加了一个新的连接端口，重启SSH服务，测试：

```
ssh localhost -p 23
```

另外需要注意的是，如果本机测试没有问题，但还是不能使用第三房工具从外部SSH链接的话，可能是防火墙的问题

### ssh 免密码登录

- 服务器端：

home 目录下新建.ssh文件夹：

```
mkdir .ssh
```

- 客户端：

生成sshkey：

```
ssh-keygen -t rsa
```

拷贝sshkey的公钥到服务器上：

```
scp ~/.ssh/id_rsa.pub root@192.168.1.113:/root/.ssh/authorized_keys
```

