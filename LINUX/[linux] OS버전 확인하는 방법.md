![](https://velog.velcdn.com/images/sunblock99/post/58ac8a85-ac4f-4ff4-b215-6ecd8be54e0b/image.png)

# 일반적인 커널에 대한 정보

```
[root@localhost ~]# uname -a
Linux localhost.localdomain 2.6.32-71.el6.x86_64 #1 SMP Fri May 20 03:51:51 BST 2011 x86_64 x86_64 x86_64 GNU/Linux
```

# OS버전에 대한 정보 1

```
[root@localhost ~]# cat /etc/issue
CentOS Linux release 6.0 (Final)
Kernel \r on an \m
```

# OS버전에 대한 정보 2

```
[root@localhost ~]# cat /etc/redhat-release
CentOS Linux release 6.0 (Final)
```

# OS버전에 대한 정보 3

```
[root@localhost ~]# cat /etc/*release*
CentOS Linux release 6.0 (Final)
CentOS Linux release 6.0 (Final)
CentOS Linux release 6.0 (Final)
cpe:/o:centos:linux:6:GA
```
