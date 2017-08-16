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

### scp 远程拷贝

Linux scp命令用于Linux之间复制文件和目录。

scp是 secure copy的缩写, scp是linux系统下基于ssh登陆进行安全的远程文件拷贝命令。

#### 简易写法

```
scp [可选参数] file_source file_target 
```

例子：

拷贝远程的test.txt文件到当前目录
```
$ scp pi@192.168.1.198:test.txt .
```

#### 语法

```
scp [-1246BCpqrv] [-c cipher] [-F ssh_config] [-i identity_file] [-l limit] [-o ssh_option] [-P port] [-S program] [[user@]host1:]file1 [...] [[user@]host2:]file2
```

#### 参数说明

- `-1`： 强制scp命令使用协议ssh1
- `-2`： 强制scp命令使用协议ssh2
- `-4`： 强制scp命令只使用IPv4寻址
- `-6`： 强制scp命令只使用IPv6寻址
- `-B`： 使用批处理模式（传输过程中不询问传输口令或短语）
- `-C`： 允许压缩。（将-C标志传递给ssh，从而打开压缩功能）
- `-p`：保留原文件的修改时间，访问时间和访问权限。
- `-q`： 不显示传输进度条。
- `-r`： 递归复制整个目录。
- `-v`：详细方式显示输出。scp和ssh(1)会显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题。
- `-c` cipher： 以cipher将数据传输进行加密，这个选项将直接传递给ssh。
- `-F` ssh_config： 指定一个替代的ssh配置文件，此参数直接传递给ssh。
- `-i` identity_file： 从指定文件中读取传输时使用的密钥文件，此参数直接传递给ssh。
- `-l` limit： 限定用户所能使用的带宽，以Kbit/s为单位。
- `-o` ssh_option： 如果习惯于使用ssh_config(5)中的参数传递方式，
- `-P` port：注意是大写的P, port是指定数据传输用到的端口号
- `-S` program： 指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项。

