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



# centos 常见服务器重启及配置文件

## [centos7重启apache、nginx、mysql、php-fpm命令](https://www.cnblogs.com/zhang-ding-1314/p/8403819.html)

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



DocumentRoot "/var/www/html"：