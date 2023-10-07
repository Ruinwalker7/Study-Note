# Git

## Git分支

### 变基

#### 变基的基本操作

`merge`

它会把两个分支的最新快照（`C3` 和 `C4`）以及二者最近的共同祖先（`C2`）进行三方合并，合并的结果是生成一个新的快照（并提交）。

![通过合并操作来整合分叉了的历史。](assets/basic-rebase-2.png)

**`变基（rebase）`**

你可以使用 `rebase` 命令将提交到某一分支上的所有修改都移至另一分支上，就好像“重新播放”一样。

![将 `C4` 中的修改变基到 `C3` 上。](assets/basic-rebase-3.png)

变基的优点：使得提交变的简洁

```shell
$ git checkout experiment
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: added staged command
```

之后切换到master分支：

```shell
$ git checkout master
$ git merge experiment
```



#### 更有趣的变基例子

![从一个主题分支里再分出一个主题分支的提交历史。](assets/interesting-rebase-1.png)

```shell
$ git rebase --onto master server client
```

以上命令的意思是：“取出 `client` 分支，找出它从 `server` 分支分歧之后的补丁， 然后把这些补丁在 `master` 分支上重放一遍，让 `client` 看起来像直接基于 `master` 修改一样”。这理解起来有一点复杂，不过效果非常酷。





#### 变基的风险

唯一准则：

**如果提交存在于你的仓库之外，而别人可能基于这些提交进行开发，那么不要执行变基。**

变基操作的实质是**丢弃一些现有的提交**，然后相应地新建一些**内容一样但实际上不同的提交**。 如果你已经将提交推送至某个仓库，而其他人也已经从该仓库拉取提交并进行了后续工作，此时，如果你用 `git rebase` 命令重新整理了提交并再次推送，你的同伴因此将不得不再次将他们手头的工作与你的提交进行整合，如果接下来你还要拉取并整合他们修改过的提交，事情就会变得一团糟。





## Git工具 

### 贮藏

当你做了一些工作，但是还没有完成，此时有需要去到别的分支做一些别的事情，就需要使用`git stash`

#### 贮藏

`git stash list`：查看存储的东西

```shell
$ git stash list
stash@{0}: WIP on master: 049d078 added the index file
stash@{1}: WIP on master: c264051 Revert "added file_size"
stash@{2}: WIP on master: 21d80a5 added number to log
```



`git stash apply`：默认应用最新的存储

`git stash apply stash@{2}`：指定某一个存储

应用`stash`可以在不干净的目录或者其他分支上面，如果不能干净的应用，都会产生`冲突`



git会自动将`stash`放入工作区而不是暂存区，如果需要将本来在暂存区的修改直接放入暂存区，请使用`--index`参数



`git stash`并不会删除文件，可以使用`git stash drop`删除文件，或者使用`git stash pop`，这条命令的参数与上面相同，但是会在弹出的时候删除弹出的贮藏。



#### 特殊使用

`git stash --keep-index`将已暂存的内容存储的同时，会将已暂存的内容保持

`git stash --include-untracked`or`git stash -u`贮藏未跟踪的文件

`git stash --patch`交互性的提示哪些需要贮藏



#### 从贮藏创建分支

有时候不想处理某些冲突的时候`git stash branch <new branchname>`可以创建一个新的分支



