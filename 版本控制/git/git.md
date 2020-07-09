### 一.配置命令

`git config`

### 二.常用操作命令

初始化空仓库：`git init`

添加文件：`git add filename`

克隆远程仓库：`git clone address`

检查文件状态：`git status`

提交：`git commit`

跳过暂存直接提交：`git commit -a`

查看暂存和提交的区别：`git diff --staged` 

查看暂存和本地的区别：`git diff`

移除：`git rm`

```
直接删除本地文件需要先add再commit才能实现，git rm直接从本地库删除，直接commit就可以，本地文件也会被删除
```

暂存库移除，本地保留：`git rm --cache`

文件重命名：`git mv src des`

查看提交历史：`git log`

指定查看提交历史：`git log -p `  `git log --patch `

查看修改状态：`git log --stat`

撤销修改：`git restore filename`

### 三.忽略某些类型文件

1.创建.gitignore文件

2.文件中添加类型

3.使用规则

```java
所有空行或者以 # 开头的行都会被 Git 忽略。

可以使用标准的 glob 模式匹配，它会递归地应用在整个工作区中。

匹配模式可以以（/）开头防止递归。

匹配模式可以以（/）结尾指定目录。

要忽略指定模式以外的文件或目录，可以在模式前加上叹号（!）取反。
```

### 四.远程仓库

查看远程仓库：`git remote -v`

添加远程仓库：`git remote add <shortname> <url>`

从远程仓库获得数据（不合并分支）：`git fetch <remote>`

从远程仓库拉去数据（合并分支）：`git pull`   拉去远程分支，将本地分支合并到拉去的分支上去

推送到远程服务器：`git push <remote> <branch>`

查看远程仓库信息：`git remote show <remote>`

修改远程仓库简写名：`git remote rename pb paul`

移除远程仓库：`git remote remove filename`



### 五.标签tag

有需要参照文档

### 六.分支

创建分支：`git branch name`

切换分支：`git checkout name`

创建并切换分支：`git checkout -b name`

合并分支：`git merge name`

删除分支：`git branch -d name`

**分支操作**

1. 切换到主分支，使用合并分支将一支分支合并到主分支，实际是将主分支的对象指针挪到被合并的分支处
2. 删除被合并的分支
3. 合并另一个分支
4. 如果有冲突，手动解决冲突，然后通过add添加到暂存区
5. 删除被合并的分支

**分支管理**

> git branch 
>
> git branch -v
>
> git branch --merged
>
> git branch --no-merged



**远程分支**

> git ls-remote remotename
>
> git remote show remotename
>
> git checkout --track origin/serverfix     设置跟踪其他分支，而不是默认的master
>
> git checkout -b sf origin/serverfix   给远程分支设置本地分支的名称
>
> git branch -u origin/serverfix   设置当前所在本地分支跟踪远程分支origin/serverfix
>
> git push origin --delete serverfix 删除远程origin的serverfix分支
>
> -----
>
> git checkout experiment   
>
> git rebase master     
>
> 两步操作将expereiment分支的修改变基到master分支，在master对应的对象基础上新加了一个提交对象
>
> git checkout master
>
> git merge experiment
>
> 切回master分支，将expereiment分支合并入master分支，master对象挪到上一步新加的提交对象上

