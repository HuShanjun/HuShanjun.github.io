---
title: 01 libuv 简介
tags: libuv
grammar_cjkRuby: true
---

---
##  概述
libuv 提供了一种基于事件的异步Reactor模型，其核心原理是运行一个永不停止的EventLoop来通知每个用户所关心的事件。libuv提供timer，network，file等核心功能。

## EventLoop
libuv 中每个线程运行一个EventLoop,用户只需为他关心的事件注册回调，当事件被触发时，用户注册的回调函数则会被调用，EventLoop在其生命周期内，通常会一直运行，其伪代码如下：
```
	while there are still events to process:
    e = get the next event
    if there is a callback associated with e:
        call the callback
```
常见的事件如下
	1. 文件可写
	2. socket可读或者可写
	3. 定时器超时
