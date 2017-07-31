---
title: Linux服务相关
date: 2017-07-27 22:30:00
categories:
- Linux-Linux常用技巧
tags:
- Linux
---

### 搜索对应关键词正在运行的程序

```
ps -ef | grep 关键词
```

### 强制杀掉进程

```
sudo kill -9 进程号
```

### 查看端口号程序

```
sudo netstat -tulpn | grep 端口号
```

