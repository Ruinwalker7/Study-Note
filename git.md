# git

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