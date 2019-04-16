---
title: 02 libuv 文件系统
tags: libuv
grammar_cjkRuby: true
---

## 1. 概述
libuv可通过uv_fs_\*系列函数和 uv_fs_t 结构体进行操作。所有文件均提供同步和异步两种操作方式，在API函数使用过程中主要区别是函数回调是否为NULL,如果为NULL，则使用同步模式，其返回值则是文件描述符，否则则以异步方式调用，其返回值为libev错误码。

*特别注意*
* libev 文件异步操作与socket不同，socket异步操作是依靠操作系统提供的非阻塞接口，文件操作则通过线程池实现 *

## 2. Reading/Writing Files
2.1 打开及关闭文件文件
```c++
	int uv_fs_open(uv_loop_t* loop, uv_fs_t* req, const char* path, int flags, int mode, uv_fs_cb cb)
	int uv_fs_close(uv_loop_t* loop, uv_fs_t* req, uv_file file, uv_fs_cb cb)
	void callback(uv_fs_t* req); //文件回调签名
```
flags 和 mode与unix中文件操作的flag和mode一致，libuv内部负责将其转化成与windows兼容的windows flags

