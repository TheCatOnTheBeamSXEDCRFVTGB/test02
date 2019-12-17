# git课程笔记

[TOC]

---



<img src="C:\Users\GYQC\AppData\Roaming\Typora\typora-user-images\1576566753248.png" alt="1576566753248" style="zoom:67%;" />



<img src="C:\Users\GYQC\AppData\Roaming\Typora\typora-user-images\1576566788428.png" alt="1576566788428" style="zoom:67%;" />



## git 基本介绍

仓库又名版本库，英文名repository，我们可以简单理解成是一一个目录，用于存放代码的，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除等操作Git都能跟踪到。

①在安装好后首次使用需要先进行全局配置

```cmd
$ git config--global user.name"用户名"
$ git config--global user.emaill"邮箱地址”
```



②创建仓库
当我们需要让Git 去管理某个新项目/已存在项目的时候，就需要创建仓库了。注意，创建仓库时使用的目录不一-定要求是空目录，选择-一个非空目录也是可以的，但是不建议在现有项目上来学习Git，否则造成的一切后果概不负责！
注意：为了避免在学习或使用过程中出现各种奇葩问题，请不要使用包含中文的目录名（父目录亦是如此）。



③Git常用指令操作
查看当前状态：git status
添加到缓存区：gitadd 文件名

> 说明：git add指令，可以添加一个文件，也可以同时添加多个文件。
> 语法1：git add文件名
> 语法2：git add文件名1文件名2文件名3...
> 语法3：git add .   [添加当前目录到缓存区中]

提交至版本库：git commit-m“注释内容”



## 时光穿梭机

版本回退分为两步骤进行操作：步骤：
①查看版本，确定需要回到的时刻点指令：

```cmd
git log 
git log --pretty=oneline
```

②回退操作
指令：

```cmd
git reset--hard 版本号
```

> 注意：回到过去之后，要想再回到之前最新的版本的时候，则需要使用指令去查看历史操作，以得到最新的commit id。指令：git reflog，

> 小结：
> a.要想回到过去，必须先得到commit id，然后通过git reset-hard进行回退；
> b.要想回到未来，需要使用git reflog进行历史操作查看，得到最新的commit id；
> c.在写回退指令的时候commit id可以不用写全，git 自动识别，但是也不能写太少，至少需要写前4位字符；



## 远程仓库使用

### 基于https协议

a.使用clone指令克隆线上仓库到本地
语法：

```cmd
git clone 线上仓库地址
```

b.在仓库上做对应的操作（提交暂存区、提交本地仓库、提交线上仓库、拉取线上仓库）
①.提交到线上仓库的指令：

```cmd
git push
```

> git push 错误提示：
>
> remote: Permission to bjitcast/shop. git denied to chn20190910
> fatal: unable to access ' https://github.com/bjitcast/shop.git/': The requested URL returned error: 403
>
> 403 错误代表没有权限
>
> 原因：
> 在首次往线上仓库提交内容的时候出现了403 的致命错误，原因是不是任何人都可以往线上仓库提交内容，必须需鉴权。
>
> 处理：
> 需要修改“.git/config"文件内容：
>
> #将
> [remote"origin"]
> url =https://github.com/用户名/仓库名.git
> 修改为：
> [remote"origin"]
> url = https://用户名：密码@github.com/用户名/仓库名.git
>
> ```
> [core]
> 	repositoryformatversion = 0
> 	filemode = false
> 	bare = false
> 	logallrefupdates = true
> 	symlinks = false
> 	ignorecase = true
> [remote "origin"]
> 	url = https://github.com/TheCatOnTheBeamSXEDCRFVTGB/test.git
> 	fetch = +refs/heads/*:refs/remotes/origin/*
> [branch "master"]
> 	remote = origin
> 	merge = refs/heads/master
> ```

②.拉取线上仓库：

```cmd
git pull
```

> 提醒：
> 在每天工作的第一-件事就是先git pull 拉取线上最新的版本：每天下班前要做的是git push，将本地代码提交到线上仓库。



### 基于ssh协议

该方式与前面https方式相比，只是影响github 对于用户的身份鉴权方式，对于git 的具体操作（如提交本地、添加注释、提交远程等操作）没有任何影响。

生成公私玥对指令（需先自行安装OpenSSH）：

```
ssh-keygen-t rsa_C“注册邮箱"
```

步骤：
①生成客户端公私玥文件
②将公钥上传到Github
③其他操作相同



## git分支操作

​		在版本回退的章节里，每次提交后都会有记录，Git 把它们串成时间线，形成类似于时间轴的东西，这个时间轴就是一一个分支，我们称之为==master分支==。
在开发的时候往往是团队协作，多人进行开发，因此光有-一个分支是无法满足多人同时开发的需求的，并且在分支上工作并不影响其他分支的正常使用，会更加安全，Git 鼓励开发者使用分支去完成一些开发任务。

```
分支相关指令：
查看分支：git branch
创建分支：git branch 分支名
切换分支：git checkout 分支名
删除分支：git branch -d 分支名
合并分支：git merge 被合并的分支名
```

> 对于新分支，可以使用“git checkout-b分支名”指令来切换分支，-b选项表示创建并切换，相当于是两个操作指令。
>
> 注意：在删除分支的时候，一定要先退出要删除的分支，然后才能删除。



## 冲突的产生与解决

### 产生

​		原因：次日在代码进行修改时前未进行 `git pull`

​				产生冲突后：代码修改后不能顺利 `git push`

### 解决

​		①. 进行`git pull`，代码会进行合并

​		②. 处理合并

​		③. 再次`git push`

