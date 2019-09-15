---
title:CentOS基本命令 
tags: 
grammar_cjkRuby: true
---

## 网络相关

### [开放端口](https://blog.csdn.net/u012498149/article/details/78772058)
1. 命令

```shell
#添加
firewall-cmd --zone=public --add-port=80/tcp --permanent
#（--permanent永久生效，没有此参数重启后失效）

#重新载入
firewall-cmd --reload

#查看
firewall-cmd --zone=public --query-port=80/tcp

#删除

firewall-cmd --zone=public --remove-port=80/tcp --permanent
```
		

### 组操作

##### 将用户添加到某一个组

``` shell
chgrp <user> <group>
```