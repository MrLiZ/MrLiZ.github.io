---
title: Linux磁盘相关
date: 2017-07-28 21:30:30
categories:
- Linux-Linux常用技巧
tags:
- Linux
---

### 查看是否有固态硬盘

```
$ cat /sys/block/sda/queue/rotational
```

返回0为固态硬盘，返回1为普通硬盘
