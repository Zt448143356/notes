# git study

## 基本概念

+ 工作区：就是你在电脑里能看到的目录。
+ 暂存区：英文叫stage, 或index。一般存放在 ".git目录下" 下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
+ 版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

![git1](images/git1.jpg "")

## 创建仓库(git init,git clone)

### `git init`

+ 在当前目录初始化git仓库，在当前目录生成一个隐藏的(.git文件)

### `git clone`

+ 使用 git clone 从现有 Git 仓库中拷贝项目

## git基本操作

### `git status`

+ 展示当前的本地git仓库的状态

### `git add .`

+ 把工作区改动的文件添加到本地的git暂存区

### `git commit -m"message(你想要的注释的信息)"`

+ 把现在git暂存区的提交到git的版本库

### `git push`

+ 把本地的版本库提交到git远端的仓库

### `git pull`

+ 把远端的git仓库download到本地

### `git reset`

+ 6162b421
+ 6162b42e
+ 6162b422

`git reset 6162b42e --hard`是回到6162b42e这个版本提交的情况下。回退到6162b42e这个版本


`git reset 6162b42e(这个为这个版本的唯一表示字符串)`
`git reset 6162b42e --hard`指定那种模式
`git reset HEAD~2`回到2个版本前

+ 默认是`mixed`模式

#### 三种模式

##### `hard`

+ 重置位置的同时，直接将工作区、暂存区及版本库都重置成目标reset节点的內容（工作区回到提交的reset节点的情况下）

##### `mixed`

+ 重置位置的同时，只保留工作区的內容，但会将暂存区和版本库中的內容更改和reset目标节点一致（版本库信息回到你reset节点的情况下，保留你后面的修改内容在工作区中）

##### `soft`

+ 重置位置的同时，保留工作区和暂存区的内容，只让版本库中的内容和reset目标节点保持一致（相当于mixed模式下它帮你`git add .`了一下）

### git revert

+ 6162b423
+ 6162b421
+ 6162b42e
+ 6162b422

`git reset 6162b42e`是撤销6162b42e这次及以后的提交，相当于现在的工作空间`git push`了`6162b422`这个版本（当时是能push以前的版本的。只是相当于），发生冲突需要解决。以这个命令拿到历史版本内容，然后可以`git add .`、`git commit -m""`来提交。

+ 这样可以push到远端。

+ 前面的`git reset`会使本地版本比远端的旧，不能直接push。需要强性push。`git push -f`

### 