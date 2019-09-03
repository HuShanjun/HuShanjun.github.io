#  ssh 免密登录

##  ssh免密登录简述

实现ssh免密登录核心工作为在本机生成 ssh密钥对后将本机公钥内容写至服务器 ~/.ssh 目录下authorized_keys 文件中，并在服务器主机中设置认证文件为 authorized_keys 配置文件为 sshd_config

## 具体步骤

1. 在本机生成ssh密钥（输入如下命令，enter 到底）

   ```bash
   mkdir ~/.ssh
   ssh-keygen -t rsa  # 本机生成ssh密钥对
   ```

2. 登录远程主机并设置认证文件：ssh

   ```bash
   ssh root@server_ip 		#服务器ip 输入密码
   vi /etc/ssh/sshd_config 	#注意不是ssh_config
   #设置如下内容
   RSAAuthentication yes
   PubkeyAuthentication yes
   AuthorizedKeysFile    .ssh/authorized_keys  这是默认的路径
   ```

3.  检查远程主机是否具有ssh相关目录，如果没有，创建.ssh目录，并推出 
    ```bash
    # 若服务器主机没有如下文件则执行如下命令
    mkdir ~/.ssh 
    touch authorized_keys 
    chmod 700 .ssh
    exit
    ```
4. 拷贝本机公钥至服务器主机并将其内容追加至authorized_keys

   ```bash
   scp ~/.ssh/id_rsa.pub root@you_server_ip:~/.ssh/temp.pub #要求输入密码
   ssh root@you_server_ip # 登录远程主机
   cat ~/.ssh/temp.pub >> ~/.ssh/authorized_keys  # 追加本地公钥文件至 authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   rm ~/.ssh/temp.pub
   exit
   ```
5. 此时本地则可免密登录服务器

   ```bash
   ssh root@you_server_ip 
   ```

##  备注

实现ssh免密后 svn更新亦可免密