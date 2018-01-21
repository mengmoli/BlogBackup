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
### 第三步：Github Pages设置
[Github Pages](https://pages.github.com/)是Github免费提供给开发者的一款托管个人网站的产品  
每个帐号只能有一个仓库来存放个人主页，而且仓库的名字必须是username/username.github.io，  
这是特殊的命名约定。你可以通过http://username.github.io 来访问你的个人主页。  
在github点击右上角你的头像选择your profile，然后选择Repositories，点击New新建一个repo  
![github_repo](/assets/blogImg/github_repo.png)
在github在这里我创建了一个github repo叫做mengmoli.github.io，用来托管我们的网站  
![创建github_page_repo](/assets/blogImg/创建githubpage_repo.png)
### 第四步：安装hexo
在本地新建一个目录，用来存放我们的hexo，然后在cd到这个文件夹输入以下命令
```text
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo g  -- 或者hexo generate,生成静态文件
$ hexo s  -- 或者hexo server，启动本地web服务,预览我们的博客，可以在http://localhost:4000/ 查看
```
然后打开http://localhost:4000/ 就可以看到一篇内置的blog。
### 第五步：部署Hexo到Github Pages
hexo可以部署到很多平台，这里我们部署到刚刚我们创建的的github pages  
打开bolg目录下的_config.yml,修改deploy配置  
![deploy配置](/assets/blogImg/hexo_deploy_config.png)
这里的repo替换成我们刚刚创建的repo的地址
![查看github repo 地址](/assets/blogImg/github_page_git_address.png)
然后保存，接下来回到命令行，输入命令
```text
$ hexo d
```
这样我们就完成了部署。
##### 可能遇到的问题：
1.部署注意需要提前安装一个扩展
```text
$ npm install hexo-deployer-git --save
```




