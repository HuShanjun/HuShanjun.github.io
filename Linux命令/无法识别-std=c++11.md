# 无法识别 -std=c++11

**一 . 问题描述 ：**

 g++ 编译c++项目出现如下错误



```
cc1plus: error: unrecognized command line option "-std=c++11"
```

 **二. 原因 ：** 

当前gcc 版本为 4.7 太低不支持 c++11 标准 

**三. 解决方案：**

 当前g++版本 4.7 需要升级的 4.8 版本

