---
 toc: true
 title: 一步一步用hexo搭建自己的博客（一）
 date: 2018-01-20
 tags:
 - hexo
---
为什么要搭建自己的博客呢，一来可以记录自己生活学习的点点滴滴，又可以学习git，markdown的使用，真是一举多得。  
<!-- more --> 
### 第一步：环境配置
安装Node  
https://nodejs.org/en/  
安装Git  
https://git-scm.com/downloads  
申请GitHub  
https://www.github.com/  
### 第二步：git连接到github  
1.随便在桌面或者文件夹右键Git Bash Here打开git命令行
![右键Git Bash Here](/assets/blogImg/bash_here.png)
2.全局设置用户名和邮箱 
```bash
$ git config --global user.name "mori"  --设置用户名为mori
$ git config --global user.email "1363044717@qq.com"  --设置邮箱，建议设置成github注册的那个
```
查看当前用户名和邮箱的命令是  
```bash
$ git config user.name    --查看当前用户名
$ git config user.email    --查看当前邮箱
```
3.使用ssh-keygen生成私钥和公钥
```bash
$ ssh-keygen -t rsa -C "1363044717@qq.com" --生成一对密钥
```
然后会出现
```bash
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa):
```
意思是要生成到哪个目录，这里直接回车，默认就行,然后会出现
```bash
Created directory '/c/Users/Administrator/.ssh'.
Enter passphrase (empty for no passphrase): --此处直接回车
Enter same passphrase again:     --此处直接回车
```
这个是让你填密码，提交到github的时候会弹出填密码的窗口，    
即使是别人上你电脑也提交不了代码。直接回车表示不需要密码，  
我们直接回车就行，然后会出现
```
 Your identification has been saved in /c/Users/Administrator/.ssh/id_rsa.
 Your public key has been saved in /c/Users/Administrator/.ssh/id_rsa.pub.
 The key fingerprint is:
 SHA256:HngoqyBxtlcklrG5RqW1rTC04V/0oWWq5VuHDRruQno 1363044717@qq.com
 The key's randomart image is:
 +---[RSA 2048]----+
 |    + o . +      |
 |   o @ + * .     |
 |    & o B o      |
 |   o B X o +     |
 |. o + O S o o    |
 | + o * + + .     |
 |o . + E +        |
 |.. o . .         |
 |  .              |
 +----[SHA256]-----+
 ```
出现这个图案，就表示密钥生成成功了。
4.本地git连接到自己的github
查看我们刚刚生成的密钥  
```
 $ cat /.ssh/id_rsa.pub 然后就会出现
 ssh-rsa xxxxxxx
 xxxxx 1363044717@qq.com
```
把这一串全部复制下来，要包含ssh-rsa和你的邮箱，不能只复制中间的，
登录我们的github，右上角点击你的头像，点击settings，进入设置界面，  
然后点击左侧菜单SSH and GPG keys，进入SSH密钥列表，  
点击右上角绿色按钮New SSH key，新建一个ssh key，
title随便填一个描述，把刚刚复制的ssh_key填到key里面,
点击 Add SSH key保存，这样就添加好了。
接下来回到git命令行，输入  
```
 $ ssh -T git@github.com  
 --然后你会看到
 The authenticity of host 'github.com (192.30.255.112)' can't be established.
 RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
 Are you sure you want to continue connecting (yes/no)? yes --直接输入yes
 Warning: Permanently added 'github.com,192.30.255.112' (RSA) to the list of known hosts.
 Hi mengmoli! You've successfully authenticated, but GitHub does not provide shell access.
 
```
这样我们就成功连接到了自己的github。
   