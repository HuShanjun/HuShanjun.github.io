# vscode win10远程开发

## 一. 生成ssh密钥对

打开cmd 输入如下命令，一路回车

```
$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\Administrator/.ssh/id_rsa):
Created directory 'C:\Users\Administrator/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\Administrator/.ssh/id_rsa.
Your public key has been saved in C:\Users\Administrator/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:QjOfwkbBqxAaypwZyhQRg6PVdf9xi+eAPLs2vOnDa1k administrator@HR-20190216CFZZ
The key's randomart image is:
+---[RSA 4096]----+
|.=+. oo .        |
|+o+ . .o .       |
|O=+.  =.  . . .  |
|=*.  +.+ o o + . |
|   . .= S + + o  |
|    .. o   oE+   |
|         o.o  .  |
|          Oo     |
|         +*=     |
+----[SHA256]-----+
```

## 二. 将生成的公钥写入服务器 ~/.ssh 目录下 authorized_keys中

1. 上传公钥文件  rz 选择需要上传的文件
2. 将 id_rsa.pub 以追加的方式写入 authorized_keys 中

## 三. vscode 安装remote-SSH插件

![1575038581609](E:\open-doc\HuShanjun.github.io\vscode\1575038581609.png)

## 四. 配置ssh下connfig文件

![1575038666634](E:\open-doc\HuShanjun.github.io\vscode\1575038666634.png)

