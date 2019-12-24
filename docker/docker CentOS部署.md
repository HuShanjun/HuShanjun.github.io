# CentOS docker部署Nodejs 服务 

1. 安装docker

   ```
   yum install -y docker-ce
   或者
   yum -y install docker
   ```

   

2. 启动docker服务

   ```
   systemctl start docker
   查看版本
   docker version
   docker info
   ```

   

3. 安装并运行unbuntu镜像

   ```
   docker pull ubuntu
   docker images
   docker run -d -it 775349758637 /bin/bash
   ```

   注：**775349758637** 为**imageid** 通过 **docker images** 可以看到当前挂载的镜像

4. 进入unbuntu镜像终端

   ```
   docker ps -a
   docker attach bfd332e86500
   ```

   注：**bfd332e86500** 为 **CONTAINER ID** 通过 **docker ps -a** 可以获取

## 参考文献

- docker基础操作 https://blog.csdn.net/YJ108283/article/details/95166286
- docker删除容器  https://blog.csdn.net/qq_26709459/article/details/80785761
- docker nodejs服务部署 https://www.cnblogs.com/pass245939319/p/8473861.html
  - https://www.jianshu.com/p/3f2f57690b3e
- docker删除镜像 https://blog.csdn.net/flydreamzhll/article/details/80900509
- docker 进入容器 https://www.cnblogs.com/xhyan/p/6593075.html
- [[Docker删除镜像和容器](https://blog.csdn.net/qq_26709459/article/details/80785761)]



# centos 常见服务器重启及配置文件

## centos7重启apache、nginx、mysql、php-fpm命令

apache
启动
systemctl start httpd
停止
systemctl stop httpd
重启
systemctl restart httpd


mysql
启动
systemctl start mysqld
停止
systemctl stop mysqld
重启
systemctl restart mysqld


php-fpm
启动
systemctl start php-fpm
停止
systemctl stop php-fpm
重启
systemctl restart php-fpm

nginx
启动
systemctl start nginx
停止
systemctl stop nginx
重启
systemctl restart nginx



```
FROM hub.c.163.com/library/ubuntu:16.04

RUN apt-get update && apt-get -y install curl && (curl -sL https://deb.nodesource.com/setup_8.x | bash) && apt-get -y install nodejs
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install yarn


export NVM_DIR="/root/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvmRemoving intermediate container 292fe3ad46f7
 ---> d6130dfda37f

```

