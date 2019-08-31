



# Linux三剑客 - sed

## 简介

一. sed全名： stream editor：流编辑器

二.  作用 ：

1. 把文本逐行放入临时缓冲区，处理完毕后，输出到品目，继续处理下一行，知道文件末尾
2. 用程序的方式来处理文本
3. 可以程序化的实现对数据的增删改查等特定的工作
4. 通常被用作管道中的过滤器

三. 常用选项

1. -n 安静模式，只显示处理的行
2. -e 直接在指令模式上进行sed的动作编辑
3. -f 执行文件中的sed指令，sed -f filename XXX
4. -r sed的动作支持的是延伸正则表达式的语法
5. -i 只修改文件内容，不输出到终端

四. 常用的的动作

1. -a  新增，增加在后面
2. -i  插入，增加在前面
3. -d 删除， 
4. -c 修改，把选定的行改成某字符串
5. -s 替换，搭配正则表达式使用
6. -p 打印

五. 

![image-20190720215347828](/Users/hushanjun/Library/Application Support/typora-user-images/image-20190720215347828.png)



## sed 的使用

![image-20190720215506982](/Users/hushanjun/Library/Application Support/typora-user-images/image-20190720215506982.png)

```bash
# 查看要修改的文本内容
hsj@Birkhoff:~/testsed$ cat a.txt //操作的文本
this is first line
	this is second line
  this is second line
this is forth line

#在第一行后面加上hello world
hsj@Birkhoff:~/testsed$ sed '1a hello world' a.txt
this is first line
hello world
	this is second line
  this is second line
this is forth line

#在第一行和第三行每一行后面加上hello world
hsj@Birkhoff:~/testsed$ sed '1,3a hello world' a.txt
this is first line
hello world
	this is second line
hello world
  this is second line
hello world
this is forth line

#在第三行至为本结尾加入hello world
hsj@Birkhoff:~/testsed$ sed '3,$a hello world' a.txt
this is first line
	this is second line
  this is second line
hello world
this is forth line
hello world

#在含有 second 的行之后加入hello world
hsj@Birkhoff:~/testsed$ sed '/second/ a hello world' a.txt
this is first line
	this is second line
hello world
  this is second line
hello world
this is forth line

```



![image-20190720221309443](/Users/hushanjun/Library/Application Support/typora-user-images/image-20190720221309443.png)

```bash
# 要操作的文件内容
hsj@Birkhoff:~/testsed$ cat a.txt 
1 A
2 AA
3 AAA
4 AAAA
5 AAAAA

# 删除第一行
hsj@Birkhoff:~/testsed$ sed '1d' a.txt
2 AA
3 AAA
4 AAAA
5 AAAAA

# 删除第一行至第二行
hsj@Birkhoff:~/testsed$ sed '1,2d' a.txt
3 AAA
4 AAAA
5 AAAAA

# 删除最后一行
hsj@Birkhoff:~/testsed$ sed '$d' a.txt
1 A
2 AA
3 AAA
4 AAAA

# 删除匹配含有三个A的行
hsj@Birkhoff:~/testsed$ sed '/AAA/d' a.txt
1 A
2 AA
```



![image-20190720223243067](/Users/hushanjun/Library/Application Support/typora-user-images/image-20190720223243067.png)

![image-20190720223755058](/Users/hushanjun/Library/Application Support/typora-user-images/image-20190720223755058.png)

![image-20190720224737039](/Users/hushanjun/Library/Application Support/typora-user-images/image-20190720224737039.png)

![image-20190720230452754](/Users/hushanjun/Library/Application Support/typora-user-images/image-20190720230452754.png)

![image-20190720230959216](/Users/hushanjun/Library/Application Support/typora-user-images/image-20190720230959216.png)

![image-20190720231116791](/Users/hushanjun/Library/Application Support/typora-user-images/image-20190720231116791.png)

