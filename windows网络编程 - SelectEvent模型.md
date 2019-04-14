---
title:  linux程序设计 - 文件操作 stat系类系统调用
tags: linux,文件
grammar_cjkRuby: true
---
# 函数原型及作用
fstat，stat及lstat系统调用返回打开的文件或者文件描述符相关的状态信息，该信信息写到一个stat的buf结构中。

```c++
#include <unistd>
#include <sys/stat.h>
#include <sys/types.h>


       int stat(const char *pathname, struct stat *buf);
       int fstat(int fd, struct stat *buf);
       int lstat(const char *pathname, struct stat *buf);

```






	