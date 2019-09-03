# centos 产看系统版本

 https://blog.csdn.net/Timeinsist/article/details/100042819

**显示系统版本**

```bash
cat  /etc/redhat-release

# 结果如下
CentOS release 6.10 (Final)
```

```bash
cat /proc/version

# 结果如下
Linux version 2.6.32-754.17.1.el6.x86_64 (mockbuild@x86-01.bsys.centos.org) (gcc version 4.4.7 20120313 (Red Hat 4.4.7-23) (GCC) ) #1 SMP Tue Jul 2 12:42:48 UTC 2019
```

```bash
 uname -a
 
 # 结果如下
Linux iZwz9fkhc6o4fi5ptnxshnZ 2.6.32-754.17.1.el6.x86_64 #1 SMP Tue Jul 2 12:42:48 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```

**查看位数**

```bash
getconf LONG_BIT

# 结果如下
64
```

```bash
file /bin/ls

# 结果如下
/bin/ls: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.18, strippe

```

