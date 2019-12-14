

# 一. docker学习 & 基本概念

## 1. docker 概述 

**1. docker是什么？**

​	**docker**是一个开源的应用容器引擎，基于golang，遵从 apache2.0 协议

**2. docker的优势**

1. Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。
2. 容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。



### 参考文献

- [[理解docker镜像与容器](https://baijiahao.baidu.com/s?id=1594187941922400728&amp;wfr=spider&amp;for=pc&amp;tdsourcetag=s_pctim_aiomsg&amp;qq-pf-to=pcqq.c2c)]
- [[关于docker容器和镜像的区别](https://www.cnblogs.com/baizhanshi/p/9655102.html)]
- [[这可能是最为详细的Docker入门吐血总结](<https://www.cnblogs.com/ECJTUACM-873284962/p/9789130.html>)]
- [docker 将正在运行的容器打包为镜像](https://www.cnblogs.com/jackadam/p/9528448.html)

```
FROM ubuntu:16.0.4
MAINTAINER netease
RUN apt-get update 
RUN mkdir node && cd node && wget https://nodejs.org/dist/v10.16.0/node-v10.16.0-linux-x64.tar.gz\
    && tar -zxvf node-v10.16.0-linux-x64.tar.gz

RUN echo "export  NODEJS_HOME=/node/node-v10.16.0-linux-x64" >> /etc/profile\
    && source /etc/profile\
    && echo "export PATH=$PATH:$NODEJS_HOME/bin" >> /etc/profile\
    && source /etc/profile

RUN sed -i s/"PermitRootLogin without-password"/"PermitRootLogin yes"/g /etc/ssh/sshd_config
RUN cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
EXPOSE 22
COPY sshd.conf /etc/supervisor/conf.d/sshd.conf
CMD ["/usr/bin/supervisord"]
```

