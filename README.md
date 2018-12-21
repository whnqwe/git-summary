# git

> https://git-scm.com/

## git 安装



> https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git



##  git 命令



####  配置user信息

> 配置user.name 和 user.email



> git config --global user.name 'your_name'
>
> git config --global user.email 'your_email@domain.com'



#### config的三个作用域

###### 三个作用域

> 缺省等于local



> git config --local				local只对某个仓库有效
>
> git config --global				global对当前用户所有仓库有效 
>
> git config --system			system对系统所有登录的用户有效

###### 显示config的配置 加 --list

>  git config --list --local



>  git config --list --global



>  git config --list --system



#### 建git的仓库

> 把已有的项目代码纳入git管理

```nginx
$ cd 项目的目录
$ git init
```

> 新建的项目直接用Git管理

```nginx
$ cd 某个项目的目录下
$ git init your_project
$ cd your_project
```



#### 往仓库里添加文件

![](assert/add.jpg)



> 查看git的工作目录与暂存区状态
>
> Untracked files: 没有放入暂存区
>
> Changes to be committed:  已经放入暂存区

```nginx
$ git status
$ git add file1 file2 ....
$ git add -u //将工作空间被修改和被删除的文件添加到暂存区(不包含没有纳入Git管理的新增文件)
$ git commit -m "first commit"
$ git log 
```

> git log
>
> commit a24e5f0897dada1c6f8a01c5cf29436f63fed86d (HEAD -> master)
> Author: user.name <user.email>
> Date:   Sun Dec 16 13:11:49 2018 +0800
>
> first commit  //提交的信息



#### 给文件重命名的简便方式

> 将工作区index.html 重名成homepage.html

##### 方法一

- 在工作区将index.html 重命名为 homepage.html

```nginx
  mv index.html homepage.html
```

- 查看状态

  >  deleted:    index.html
  >
  >  Untracked files: homepage.html
  >
  >  Git以为进行了两步操作先删除index.html然后创建了一个新的homepage.html文件

```nginx
$ git status
```

- 加入暂存区

```nginx
$ git add homepage.html
$ git rm -r index.html
```

- 查看状态

  > Changes to be committed:  renamed:    index.html -> homepage.html

```nginx
$ git status
```

​	

##### 方法二

- git mv

```nginx
git mv index.html homepage.html
```

- git status

  > Changes to be committed:renamed:    index.html -> homepage.html



#### 查看版本演变历史

- 简洁的查看版本提交历史

```nginx
$ git log --oneline
```

![](assert/gitlogoneline.jpg)

- 查看历史提交的前几条数据

```nginx
$ git log -n2 --oneline
```

![](assert/gitlogn2.png)

- 查看所有分支的提交记录

```nginx
$ git log --all
```

- 查看有多少个分支

```nginx
$ git branch -v 
```

- 创建并切换到当前分支

```nginx
$ git checkout -b branch_na
```

- 查看图形化的 log 地址

```nginx
$ git log --all --graph
```

![](assert/gitlogall.png)

![](assert/gitlogallgraph.png)

- 查看所有分支最近 4 条单行的图形化历史。

```nginx
$ git log --oneline --all --graph -n4
```

#### 图像化界面

```nginx
$ gitk
```



#### .git文件的解读



##### HEAD

> 当前的分支引用

![](assert/head.png)

##### config

> 设置的用户名密码

![](assert/config.png)

#### commit ,tree,blob三个对象之间的关系

> git cat-file -t  查看对象类型
>
> 一个commit包含有一tree
>
> 一个tree里面可以包含tree (映射到系统中就是文件夹)或者 blob（映射到系统中就是文件）
>
> 文件内容相同，git眼里就是唯一的blob



![](assert/committreeblob.png)



```nginx
$ git log
```

![](assert/gitlog.png)

```nginx
$ git cat-file -p  836040341a3b
```

![](assert/gitcatfilecommit.png)

```nginx
git cat-file -p 136923d67
```

![](assert/gitcatfiletree.png)

```nginx
git cat-file -p 6ba2ecab847
```

![](assert/gitcatfileblob.png)



#### 分离头指针（detached HEAD）

> git checkout commit_id   切换到某个没有分支的commit下，进行修改



> 某个变更没有基于任何branch



#### HEAD 与 branch

> HEAD
>
> - 指向分支的最新一次的修改
> - 在分离头指针清空下，指向了某个commit上，没有和任何的分支挂钩

- 创建并切换到新分支

```nginx
$ git checkout -b newbranch
```

![](assert/addbranch.png)

![](assert/queryhead.png)



#### 怎么删除不需要的分支

```nginx
$ git branch -d branch_name
```

![](assert/gitbranchd.png)



#### 修改最新的commit的message的信息

```nginx
$ git commit --amend
```

![](assert/gitcommitamend.png)



#### 修改老旧的commit的message的信息

> git rebase -i  -----> reword

> 尚未提交到远端服务器的commit
>
> 变基时需要填父级 commit .那最老的父级是没有父级的，是不是就不可变基了？

```nginx
$ git rebase -i prant_commit_id
```

#### 连续的多个commit整理成一个

> git rebase -i  ----->  squash

```nginx
$ git rebase -i prant_commit_id
```

#### 怎样把间隔的几个commit整理成一个？

> 如果没有父类，在【-i】跳出的页面中，将pick添加上去
>
> git rebase -i  ----->  squash
>
> pick commit_id

```nginx
$ git rebase -i prant_commit_id
```

#### 比较暂存区和HEAD所含文件的差异

> 需要加 --cached

 ```nginx
$ git diff --cached
 ```

#### 比较工作区和暂存区所含文件的差异

```nginx
$ git diff [-- file1 file2 .....]
```

#### 暂存区恢复成和HEAD的一样

>  reset
>
>  - 不加 --hard，则暂存区的内容恢复成HEAD对应的内容，工作区的变更继续保留。
>
>  - 加了 --hard，则不管工作区还是暂存区，内容都变回HEAD对应的内容。  

>  git reset 有三个参数
>
> - --soft 这个只是把 HEAD 指向的 commit 恢复到你指定的 commit，暂存区 工作区不变
> - --hard 这个是 把 HEAD， 暂存区， 工作区 都修改为 你指定的 commit 的时候的文件状态
> - --mixed 这个是不加时候的默认参数，把 HEAD，暂存区 修改为 你指定的 commit 的时候的文件状态，工作区保持不变

```nginx
$ git reset HEAD
```

#### 工作区的文件恢复为和暂存区一样

```nginx
$ git checkout -- <file>
```



#### 取消暂存区部分文件的更改

```nginx
 git reset HEAD -- <file>
```

#### 消除最近的几次提交

```nginx
$ git reset --hard commit_id
```



#### 不同提交的指定文件的差异

```nginx
$ git diff brach_name1 branch_name2 -- file_name
```

#### 正确删除文件的方法

```nginx
$ git rm file_name
```

#### 开发中临时加塞了紧急任务的处理

> 开发A任务的过程中，临时接到了很紧急的任务

```nginx
$ git stash 
$ git stash apply
$ git stash pop
$ git stash list
```

#### 指定不需要Git管理的文件

> https://github.com/github/gitignore/

```nginx
vi .gitignore
```

