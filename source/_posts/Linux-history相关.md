---
title: Linux history 相关
date: 2017-07-29 21:30:00
categories:
- Linux-Linux常用技巧
tags:
- Linux
- history
---

### 让history命令保存时间

在`.bash_profile`中加入：

```
export HISTTIMEFORMAT="%d/%m/%y %T "
```

然后保存退出，运行：

```
$ source ~/.bash_profile
```

### 让history实时保存

在`.bash_profile`中加入：

```
shopt -s histappend
PROMPT_COMMAND="history -a;$PROMPT_COMMAND"
```

然后保存退出，运行：

```
source .bash_profile
```
