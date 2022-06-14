# 常用git命令

## 本地仓库

查看文件内容

```
$git <filename>
```

添加所有文件

```
$git add .
```

添加文件到git(暂存区)

```
$git add <filename>
```

查看仓库状态

```
$git status
```

提交文件到仓库

```
$git commit -m "更新内容"
```

查看文件不同的地方

```
$git diff
```

查看提交日志

```
$git log
```

查看提交日志(简略)

```
$git log --pretty=oneline
```

查看历史命令

```
$ git reflog
```

回退到版本

```
//上个版本
$git reset --hard HEAD^

//上上个版本
$git reset --hard HEAD^^

//100个之前的版本
$git reset --hard HEAD~100

//回退到对应commit id的版本
$git reset --hard 1094a
```

修改内容已经使用add命令添加到暂存区，使内容回到工作区

```
$git reset HEAD <filename>    //HEAD表示当前版本
```

撤销最近一次修改(回到最近一次`git commit`或`git add`时的状态)

```
$git checkout -- <filename>
```

在版本库中删除文件

```
$git rm <filename>
$git commit -m "..."
```

## 远程仓库

### 1、创建SSH KEY

创建完成后会在用户主目录下生成.ssh目录（id_rsa是私钥  id_rsa.pub是公钥）

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

### 2、绑定ssh

在github主页中，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

### 3、添加远程仓库

在本地仓库目录下执行，

origin后添远程仓库的ssh

添加后远程仓库的名字就是origin(默认名)

```
$ git remote add origin git@github.com:ericStefan/gitTest.git
```

### 4、推送本地内容到远程仓库

实际上是把当前分支`master`推送到远程。

```
$git push -u origin master
```

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

### 5、删除远程库

查看远程信息

```
$git remote -v
```

根据名字删除

```
$git remote rm origin
```

这里的删除时接触本地和远程的绑定关系。

### 6、克隆远程仓库

```
$git clone git@github.com:ericStefan/gitTest.git
```

Git支持多种协议，包括`https`，但`ssh`协议速度最快。

## 分支管理

### 1、创建合并分支

Git鼓励大量使用分支

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支（快速合并）：`git merge <name>`

删除分支：`git branch -d <name>`

### 2、解决冲突

当两个分区的内容在合并的时候产生冲突(例如在相同的地方有不同的修改)，需要解决冲突后再合并。

合并两个冲突的分支时，Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，方便我们更改。

用`git log --graph`命令可以看到分支合并图。

### 3、分支管理策略

master是十分稳定的，仅仅用来发布新版本，平时不在上面干活

一般都在dev分支上干活，到一个阶段时再把dev分支合并到master上

`Fast forward`模式：这种模式下，删除分支后，会丢掉分支信息。

`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

```
$ git merge --no-ff -m "merge with no-ff" dev
```

