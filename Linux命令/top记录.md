# top 字段含义

列名 含义
PID 进程 ID
USER 进程所有者的用户名
PR 任务优先级
NI nice 值。数值越小表示优先级越高，数值越大表示优先级越低
VIRT 进程使用的虚拟内存总量，单位：kb。VIRT=SWAP+RES
RES 进程使用的、未被换出的物理内存大小，单位：kb。RES=CODE+DATA
SHR 共享内存大小，单位：kb
S 进程状态。
D= 不可中断的睡眠状态
R= 运行
S= 睡眠
T= 跟踪 / 停止
Z= 僵尸进程
%CPU 上次更新到现在的 CPU 时间占用百分比
TIME+ 进程使用的 CPU 时间总计，精确到 1/100 秒
COMMAND 命令名 / 命令行



# npm 源设置

**查看npm源：** npm config get registry

**设置npm源：** npm config set registry https://registry.npm.taobao.org

 编辑 ~/.npmrc 加入下面内容

```
registry = https://registry.npm.taobao.org
```

