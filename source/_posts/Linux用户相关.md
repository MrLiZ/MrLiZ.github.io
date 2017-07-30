---
title: Linux用户相关
date: 2017-07-28 22:50:55
categories:
- Linux-Linux常用技巧
tags:
- Linux
---

### 新建用户，配置可通过sudo使用root权限

- 添加用户

```
adduser xxx
```

- 赋予root权限

    - 方法一：修改 `/etc/sudoers` 文件，找到下面一行，把前面的注释（#）去掉
    ```
    ## Allows people in group wheel to run all commands
    %wheel  ALL=(ALL)   ALL
    ```
    然后修改用户，使其属于root组（wheel），命令如下：
    ```
    # usermod -g root tommy
    ```
    修改完毕，现在可以用tommy帐号登录，然后用命令 su - ，即可获得root权限进行操作。
    - 方法二：修改 `/etc/sudoers` 文件，找到下面一行，在root下面添加一行，如下所示：
    
    ```
    ## Allow root to run any commands anywhere
    root    ALL=(ALL)   ALL
    tommy   ALL=(ALL)   ALL
    ```
    修改完毕，现在可以用tommy帐号登录，然后用命令 su - ，即可获得root权限进行操作。

### 将用户添加到另一个组

将一个用户添加到用户组中，千万不能直接用：`usermod -G groupA`

这样做会使你离开其他用户组，仅仅做为 这个用户组 groupA 的成员。 
应该用 加上 -a 选项： 

```
usermod -a -G groupA user
(FC4: usermod -G groupA,groupB,groupC user)
```

-a 代表 append， 也就是 将自己添加到 用户组groupA 中，而不必离开 其他用户组。 

> 注意：要退出再重新登陆才生效

查看用户所属的组使用命令：

```
$ groups user
```

或者查看文件：

```
$ cat /etc/group
```

