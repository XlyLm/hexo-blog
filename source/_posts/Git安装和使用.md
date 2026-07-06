---
title: Git安装和使用
abbrlink: 2373421626
date: 2026-07-06 21:28:36
tags: 版本管理
index_img: /img/版本管理/git.png
cover: /img/版本管理/git.png
---
# Git安装和使用

## 一、安装Git

- Windows：[Git下载地址](https://git-scm.com/install/)

- Mac：[Git下载地址](https://sourceforge.net/projects/git-osx-installer/)
- Linux：Ubuntu：`sudo apt-get install git-core`，Red Hat：`sudo yum install git-core`

​	下载安装文件，双击执行安装。

## 二、配置SSH

- 打开 git bash 终端
- 设置 user.name 和 user.email

```bash
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub注册邮箱"
```

- 生成 ssh 密钥

```bash
ssh-keygen -t rsa -C "你的GitHub注册邮箱"
```

​	此时，你的用户文件夹 (C:/user/***/.ssh) 下会有一个新的文件夹.ssh，里面有刚刚创建的 `ssh` 密钥文件 `id_rsa` 和 `id_rsa.pub`。

- 将 ssh 公钥添加到 GitHub 上

​	进入 GitHub-> 用户头像→Settings→SSH and GPG keys→New SSH key→将 id_rsa.pub 中的内容复制到 Key 文本框中，然后点击 Add SSH key (添加 SSH) 按钮。

## 三、git指令详解

### 1、初始化

创建一个git仓库，创建之后就会在当前目录生成一个.git的文件

```bash
git init	#在项目目录进入git bush面板
```

### 2、系统配置

 该命令将分别设置提交代码的用户名和电子邮件地址 。

```bash
git config –global user.name “[name]”
```

```bash
git config –global user.email “[email address]”
```

### 3、添加文件到缓冲区

单个文件

```bash
git add [filename]
```

 添加所有文件到缓冲区

```bash
git add *
or
git add --all
```

加all可以添加被手动删除的文件，而加“*”不行。

### 4、 删除文件

```bash
git rm [filename]
```

 该命令将删除工作目录中的文件，并将删除动作添加到stage。 

### 5、提交缓冲区的所有修改到仓库

```bash
git commit -m "提交的说明"

commit可以一次提交缓冲区的所有文件
```

### 6、查看git库的状态

```bash
git status
```

未提交的文件，分为两种，add过已经在缓冲区的(绿色)，未add过的(红色)

### 7、比较文件

```bash
git diff [filename]
```

 如果文件修改了，还没有提交，就可以比较文件修改前后的差异

```bash
git diff --staged
```

该命令可以显示尚未添加到stage的文件的变更。 

```bash
git diff [first branch] [second branch]
```

 该命令可以显示两个分支之间的差异。 

### 8、查看日志

```bash
git log
```

### 9、版本回退

```bash
git reset
```

可以将当前仓库回退到历史的某个版本

- 第一种用法，回到上一个版本：

  ```bash
  git reset --hard HEAD^
  ```

- 第二中用法，回到指定版本：

  ```bash
  git reset --hard [版本号]
  ```

### 10、 查看仓库的操作历史 

```bash
git reflog
```

### 11、分支管理

#### (1)查看分支情况

 前面带*号的就是当前分支 

```bash
git branch //查看本地分支(绿色)
git branch -r //查看远程所有分支(红色)
git branch -a //查看本地和远程所有分支
```

#### (2)创建分支

```bash
git branch [分支名]
```

#### (3)切换当前分支到指定分支

```bash
git checkout [分支名]
```

#### (4)创建分支并切换到创建的分支

```bash
git checkout  -b [分支名]
```

#### (5)合并某分支的内容到当前分支

```bash
git merge [分支名]
```

#### (6)拉取远程分支到本地

> **意：该方法创建的本地分支 xxx 会和远程仓库 yyy 的 zzz 分支建立映射关系**

- **方法一：**

```shell
//自动切换到本地分支 xxx。
git checkout -b xxx(本地分支名) yyy(远程仓库名)/zzz(远程仓库分支名)
```

- **方法二：**

```shell
//不会自动切换
git fetch yyy(远程仓库名) zzz(远程仓库分支名):xxx(本地分支名)
```

如果两个分支同时进行了同一个文件的修改和提交，在merge时就会产生冲突，首先要手动打开文件解决冲突，再提交，就相当于进行了merge 

#### (7)删除分支

```bash
git branch -d [分支名]
git branch -d -r [本地分支名]	//删除远程分支，删除后要推送到服务器才行
```

#### (8)查看分支合并图

```bash
git log --graph
```

#### (9)本地分支和远程分支的映射关系

- [教程](https://blog.csdn.net/tterminator/article/details/78108550)

### 12、 版本标签 

#### (1)为指定版本增加标签

```bash
git tag [标签名] [版本号]
```

#### (2)查看所有标签

```bash
git tag
```

#### (3)查看标签的详细信息

```bash
git show [标签名]
```

#### (4)将tag 提交到远端仓库 

 推送所有tag: 

```bash
git push [别名](origin) --tags
```

 推送某个tag： 

```bash
git push [别名](origin) v1.0 
```

### 13、git远端库相关

#### (1)记录远程仓库地址并为其取别名

```bash
git remote add [别名](origin) [远程仓库地址]
```

#### (2)移出远程仓库

```bash
git remote remove [别名](origin)
```

#### (3)将本地仓库推送到远程仓库

```bash
git push [-u] [别名](origin) [分支名](master)	#-u表示第一次推送，后面推送可以不写

推送所有分支
git push --all [别名](origin)
```

#### (4)从远程库更新内容到本地

```bash
git pull [别名](origin) [分支名](master)
```

 **如果push的时候，本地和文件和远端文件有冲突，就要先pull、然后手动解决冲突，才能继续push** 

#### (5)git记住用户密码

 push的时候默认每次都需输入GitHub的用户名和密码，在git仓库根目录下.git文件夹的config文件末尾增加如下内容，即可记住用户名密码，无需每次推送都输入了 

```bash
[credential]
     helper = store
```

### 14、克隆远程仓库

 该命令可用于通过指定的URL获取一个代码库。 

```bash
git clone [url]
```

### 15、查看某个版本历史记录

```bash
git log --follow [file]
```

### 16、临时保存所有修改文件

```bash
git stash save
```

### 17、恢复最近stash的文件

```bash
git stash pop
```

### 18、显示stash所有变更

```bash
git stash list
```

### 19、丢弃最近一次变更

```bash
git stash drop
```

### 20、git 中一些选项解释

```javascript
-d  --delete：删除
-D  --delete --force的快捷键
-f  --force：强制
-m  --move：移动或重命名
-M  --move --force的快捷键
-r  --remote：远程
-a  --all：所有
```

## 四、密钥配置

可以配置多个密钥，将要配置的密钥所在文件取名

在.ssh文件创建config文件：

```javascript
# 第一个配置
Host self
    HostName gitee.com 
    User xxx@qq.com 
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_self
    
# 第二个配置
Host company
    HostName gitee.com 
    User xxx@xxx.com 
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_xx
    
Host：区分不同的配置，可以任意取，但会影响远程仓库地址，比如由原来的git@gitee.com:testUser/xxx.git 变成 git@self:testUser/xxx.git（建议常用的跟原来的保持一致）
HostName：真实的服务器地址（域名）
User：之前配置的用户名（xxx@xxx.com）
PreferredAuthentications：权限认证（publickey,password publickey,keyboard-interactive）一般直接设为publickey
IdentityFile：rsa文件地址
```

## 五、添加密钥

```javascript
ssh-add ~/.ssh/id_rsa_github
```

如果执行ssh-add时提示"Could not open a connection to your authentication agent"，则需先执行以下命令：

```javascript
ssh-agent bash
```

添加成功会出现提示：Identity added 等字样。

ssh命令验证：

```javascript
ssh -T git@github.com
或
ssh -T git@<IP-address>	//企业git域
```

