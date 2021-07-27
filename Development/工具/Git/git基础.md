# Git

[TOC]



## Git的配置

Git提供了一个叫做git config的工具，专门用来配置或读取相应的工作环境变量。而正式由这些环境变量，决定了Git在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方

- `/etc/gitconfig` 文件: 系统中对所有用于都普遍适用的配置。若使用git config 时用`--system`选项，读写的就是这个文件
- `~/.gitconfig 文件`: 用户目录下的配置文件只适用于该用户。若使用git config时用`--global`选项，读写的就是这个文件
- 当前项目的git目录中的配置文件`.git/config`文件:这里的配置仅仅针对当前项目有效。若使用git config时用`--local`选项，读写的就是这个文件

每个级别的配置都会覆盖上层的相同配置，所以`.git/config`里的配置会覆盖`/etc/gitconfig`中的同名变量

​    

#### 配置用户信息

> 这两条信息很重要，每次Git提交时都会引用这两条信息，说明是谁提交了更新，会随着更新内容以前纳入历史版本。
>
> 使用`--global`选项 更改的配置就是位于用户主目录下的那个

```bash
git config --global user.name "luozongle"
git config --global user.email "luozongle@xxx.com"
```

  

#### 查看配置信息

```bash
# 查询所有的配置信息，可能会有重复的变量名，因为它们可能来自不同的配置文件，不过git实际采用的是最后一个
git config --list   
# 可以指定查询~/.gitconfig中的内容
git congig --list --global
# 查询某个环境变量 只要把特定的名字跟在后面就可以
git config user.name
```



Git能够识别的配置项分为两大类:客户端和服务器端。其中大部分属于客户端配置

> 查看当前版本的git支持的选项列表

```bash
man git-config
```



**core.excludesfile**

> 可以配置全局生效的.gitignore文件

```bash
git config --global core.excludesfile ~/.gitignore_global
```



**help.autocorrect**

如果我们打错了一条命令，会显示

```bash
$ git chekcout master
git：'chekcout' 不是一个 git 命令。参见 'git --help'。

您指的是这个么？
  checkout
```

git会尝试猜错我们的意图，但它不会执行。如果把help.autocorrect设置成1，那么只要有一个命令被模糊匹配到了，git会自动执行该命令

```bash
git config --global help.autocorrect=1
```



**core.ui**

> git会自动着色大部分输出内容，但如果不喜欢花花绿绿，也可以关掉

```bash
git config --global color.ui false
```



**core.autocrlf**

格式化与多余的空白字符是许多开发人员在协作时，特别在跨平台情况下，不时遇到的令人头疼的问题。由于编辑器的不同或者文件行尾的换行符被windows下被替换掉了。

windows使用回车(CR)和换行(LF)两个字符来结束一行，而macOS和Linux只使用换行(LF)一个字符。

Git可以在提交时自动地把回车和换行符换成换行，而在检出代码时把换行转换成回车和换行。

```bash
# windwos上可以设置为true，检出时换行会被转换成回车和换行
git config --global core.autocrlf true

# 如果使用以换行为结束符的Linux或macOS，不需要Git在检出文件时自动转换，input可以告诉git在提交时把回车和换行转换成换行，检出时不转换
git config --global core.autocrlf input

# 如果是windows，且开发仅运行在windows上的项目，可以设置false取消此功能
git config --global core.autocrlf false

```





**获取帮助**

> 如果想了解git的工具该怎么使用，可以阅读它们的使用帮助

```bash
git help <verb>
git <verb> --help
man git-<verb>
```

例: 学习config命令可以怎么使用

```bash
git help config
```



**常用命令**

```bash
# 从远程仓库克隆
git clone git://github.com/xxx

# 从远程仓库克隆并指定目录名 
git clone git://github.com/xxx learnGit
```







**忽略某些文件**

> 如果有一些文件无需纳入Git的管理。我们可以创建一个`.gitignore`的文件，列出要忽略的文件模式

文件`.gitignore`的格式规范如下

- 所有空行或以注释符号#开头的行都会被Git忽略
- 可以使用标准的glob模式匹配
- 匹配模式最后跟反斜杠`/`说明要忽略的是目录
- 要忽略指定模式以外的文件或目录，可以在模式前加上`!` 取反

> glob模式是指shell所使用的简化的正则表达式。
>
>  `*` 匹配零个或多个任意字符
>
> `[abc]` 匹配任意一个方括号中的字符(要么匹配一个a,要么匹配一个b,要么匹配一个c)
>
> `?`只匹配一个任意字符
>
> `[0-9]`匹配所有0到9的数字

```txt
# 忽略所有以.o或.a结尾的文件
*.[oa]

# 忽略所有以~结尾的文件
*~

```





## 常用操作



### 初始化仓库

使用git init初始化仓库

```bash
# 初始化仓库
git init

# 在指定目录中初始化仓库
git init learngit
```





### 提交文件

```bash
# 将文件或目录纳入版本控制或更新到暂存区(stage)
git add 文件名或目录

# 将已经跟踪的文件更新到暂存区
git add -u 


# 提交
git commit -m "xxx"

# 跳过使用暂存区，把所有已经跟踪过的文件暂存起来一并提交，跳过使用add的步骤
git commit -a -m "xxx"
```





### 删除文件

```bash
# 从Git中移除文件，并从工作目录中删除(如果自己手动删除文件，没有使用git rm删除，无法将文件从版本控制中移除，还需要git rm一下)
git rm 文件

# 如果删除之前已经修改文件并已经存放到暂存区的话，需要使用-f命令强制删除
git rm -f 文件

# 如果我们想把文件从git仓库中删除(或者从暂存区移除)，但仍然希望保留在当前工作目录中。就使用--cached参数
git rm --cache 文件名  

# 也可以使用glob模式, *之前的反斜杠\，因为Git有它自己的文件模式扩展匹配方式，所以我们不需要shell帮忙展开,如果不加\ 仅仅会删除log目录下的*.log，不会递归匹配
git rm log/\*.log

# 加了\后由git自己匹配，会递归匹配，删除当前目录及其子目录中所有~结尾的文件
git rm \*~
```





### 文件重命名

```bash
# 将one.txt改名为two.txt  
# 运行git mv命令相当于执行了下面三条命令，如此分开操作，git也会意识到这是一次改名
# 1. mv one.txt two.txt
# 2. git rm one.txt
# 3. git add two.txt
git mv one.txt two.txt
```



### 比较差异

```bash
# 比较工作目录当前文件和暂存区快照之间的差异
git diff

# 比较暂存区和已提交快照之间的差异(--cached和--staged效果是一样的)
git diff --cached 
git diff --staged
```



### git reset

> 如果commit 已经被推到git远程仓库中了，需要使用git push -f 强制推到远程仓库中，需要有管理员权限

```bash
# 会仅仅将HEAD恢复上一个版本，index与工作目录不会被恢复(仅仅撤销了上一个commit)
git reset --soft HEAD~

# 会用HEAD指向的当前快照内容更新索引(HEAD和index都会被恢复至上个版本) --minxed也是reset的默认行为
git reset --minxed HEAD~

# 会把工作目录 索引 HEAD都恢复到上个版本
git reset --hard HEAD~
```



### git revert

git revert可以删除某些历史

```bash
# 删除364d705到61ab6e0 范围内的(不包含364d705 包含61ab6e0) 364d705提交时间比61ab6e0早
git revert 364d705..61ab6e0

# 不自动提交commit，这样只会生成一个commit 如果不加-n 会把删除的每一个commit都对应生成一个新的commit
git revert 364d705..61ab6e0 -n
```







## Git历史

> 查看git历史使用git log命令

```bash
# 查询git当前分支历史记录
git log

# 查询master分支的历史记录
git log master

# 显示每次提交的内容差异
git log -p

# 查询git所有分支的历史记录 如果--all和指定分支名同时出现，--all优先
git log --all

# 带图形化的git历史
git log --all --graph

# 查询git历史记录 只显示单行
git log --oneline

# 查询git最近4次的历史记录
git log -n4 或 git log -4

# 查询git最近4次的历史记录 并显示单行
git log -n4 --oneline

# 显示简要的增改行数统计，每个提交都列出了修改过的文件，以及增加移除的函数，并在最后列出所有增减行数小计
git log --stat

# 只显示 --stat中最后行数修改添加移除统计
git log --shortstat

# 仅在提交信息后显示已修改的文件清单
git log --name-only

# 仅显示新增、修改、删除的文件清单
git log --name-status

# 仅显示SHA-1的前几个字符，而非所有的40个字符
git log --abbrev-commit 

# 使用较短的相对时间显示(比如,"2 weeks age")
git log --relative-date

# 显示ASCII图形表示的分支合并历史
git log --graph 

# 使用--pretty选项可以指定使用完全不同于默认格式的方式展示提交历史。可选项有oneline、short、full、fuller
git log --pretty=oneline

# 我们还可以使用--graph参数，可以看到开头以ASCII字符串表示的简单图形
git log --pretty=format:"%h %s" --graph

```



format选项可以定制要显示的记录格式，这样的输出便于后期编程提取分析

例:

```bash
git log --pretty=format:"%h - %an, %ar : %s"

746318c - r_zhang, 6 hours ago : 11:49
55cecf4 - _luo, 6 hours ago : 11:48
6040bc4 - v_luo, 6 hours ago : 11:47
cb4ec46 - luozongle, 6 hours ago : 11:46
a784584 - lzl, 6 hours ago : 11:44
b4bcb09 - v_luozongle, 8 hours ago : lzl20210725
0d39398 - v_luozongle, 3 days ago : 3
a83813e - v_luozongle, 3 days ago : rm_one.txt
9fde8be - v_luozongle, 3 days ago : 2
65e3d7e - v_luozongle, 3 days ago : 1
4316d86 - v_luozongle, 3 days ago : new
```



常用的格式占位符写法

| 选项 | 说明                                     |
| ---- | ---------------------------------------- |
| %H   | 提交对象(commit)的完整哈希串             |
| %h   | 提交对象的简短哈希串                     |
| %T   | 树对象(tree)的完整哈希串                 |
| %t   | 树对象的简短哈希串                       |
| %P   | 父对象(parent)的完整哈希串               |
| %p   | 父对象的简短哈希串                       |
| %an  | 作者(author)的名字                       |
| %ae  | 作者的电子邮箱地址                       |
| %ad  | 作者修订日期(可以用 -date= 选项定制格式) |
| %ar  | 作者修订日期，按多久以前的方式显示       |
| %cn  | 提交者(committer)的名字                  |
| %ce  | 提交者的电子邮件地址                     |
| %cd  | 提交日期                                 |
| %cr  | 提交日期，按多久以前的方式显示           |
| %s   | 提交说明                                 |



限制输出长度

除了定制输出格式的选项之外，git log 还有许多实用的限制输出长度的选项，也就是只输出部分提交信息。

按照条数筛选

```bash
# 显示最近4次提交，n可以是任何自然数，表示仅显示最近的若干条提交
git log -n 4 或 git log -4
```



按照时间筛选

> 使用--since(指定时间之后的提交;不包含)和--until(指定时间之前的提交包含)，对时间做筛选，可以给出各种时间格式，比如具体的某一天"2008-01-15",或者多久以前("2 years 1 day 3 minutes age")

```
# 显示最近两周内的提交
git log --since=2.weeks

# 筛选出2021.07.20(不包含)-2021.07.22(包含)的提交
git log --since="2021.07.20" --until="2021.07.22"
```



按作者或提交者筛选

```bash
# 筛选出作者是luozongle的提交
git log --author=luozongle

# 筛选出提交者是luozongle的提交
git log --committer=luozongle
```

按commit信息筛选
```bash
# 筛选出commit信息中包含删除的commit
git log --grep "删除"
```







## 分支
几乎每一种版本控制系统都以某种形式支持分支。使用分支意味着我们可以从开发主线上分离开来，然后在不影响主线的同时继续工作。在很多版本控制系统中，这是个昂贵的过程，常常需要创建一个源代码目录的完整副本，对大型项目来说会花费很长时间。但是Git的分支非常的轻量级，它的新建操作几乎可以在瞬间完成，Git鼓励在工作流程中频繁使用分支与合并。


### 查询分支

```bash
# 列出当前本地所有的分支清单
git branch

# 查询各个分支最后一个提交对象的信息
git branch -v

# 查询远程分支
git branch -r 

# 查询所有远程与本地分支
git branch -a

# 查询已经与当前分支合并的分支或者说哪些分支是当前分支的直接上游
git branch --merged

# 查询哪些分支尚未与当前分支合并
git branch --no-merged


```



### 新建分支

```bash
# 在当前commit对象上新建一个testing分支
git branch testing

# 新建并切换到testing分支 相当于执行了 git branch testing && git checkout testing 两条命令
git checkout -b testing

git checkout -b serverfix origin/serverfix
```




### 切换分支

```bash
# 切换到testing分支
git checkout testing
```

  

### 合并分支

```bash
# 将testing分支的内容合并到当前分支
git merge testing
```

  

### 删除本地分支

```bash
# 删除testing分支
git branch -d testing
```

### 删除远程分支
```bash
git push origin :serverfix
```


### 推送本地分支

```bash
# 将本地分支testing推送到远程origin仓库的testing分支上
git push origin testing

# 其实这才应该是上面完整的命令
git push refs/heads/testing:refs/heads/testing
```

  

### 跟踪远程分支
从远程分支checkout出来的本地分支，称为跟踪分支。跟踪分支是一种和某个远程分支有直接联系的本地分支。在跟踪分支里输入git push，Git会自行判断应该向哪个服务器的哪个分支推送数据。同样，在这些分支里运行git pull会获取所有远程索引，并把它们都合并到本地分支中来。
在克隆仓库的时，git通常会自动创建一个master的分支来跟踪origin/master。这正是git push和git pull一开始就能正常工作的原因。当然我们也可以随心所欲的设定为其它跟踪分支。
```bash
git checkout -b [分支名] [远程名]/[分支名]

# 如果git是1.6.2以上的版本也可以使用--track简化
git checkout --track origin/serverfix
```



### 合并

整合分支最容易的方法是merge命令。它会把两个分支的最新快照以及二者最近的共同祖先进行三方合并，合并的结果是生成一个新的快照。

还有一种方法是使用rebase。可以使用rebase命令将提交到某一分支上的所有修改都移至另一个分支上，就好像重新播放一样。

例:

```bash
git checkout one
git rebase two
```

上面rebase的原理就是先找到这两个分支(当前分支one、变基操作的目标基底分支master)的最近共同祖先，然后对比当前分支相对于该祖先分支的历次提交，提取相应的修改并存为临时文件，然后将当前分支指向目标基底，最后将此前另存为临时文件的修改依次应用。

也就是说如果我在one分支上面执行git rebase two，two分支所有的commit会先放到他们祖先commit，然后才会把one分支的commit放到two分支最后一个commit后，不管one分支是否和two分支的commit时间有交错，one分支的commit id也会发生变化。千万不要让已经push到远程仓库中的commit id发生变化，要不然别人在更新下载后在其基础上开展的工作，会因为我们把commit id搞的不一样了，而出现问题。

总的原则就是，只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变基操作。














## 修改commit信息



> 修改最近的一次提交

```bash
git commit --amend
```



## 取消已经暂存的文件

> 如果我们有两个修改过的文件，想分开提交，但是不小心用`git add .`全加到了暂存区域，可以使用`git reset HEAD`命令
>
> 比较新的版本可以使用git restore --staged命令

```bash
# 将xxx.java恢复到已修改未暂存的状态
git reset HEAD xxx.java

# 如果不小心添加了.idea目录，可以使用这种方式恢复到未暂存
git reset HEAD .idea

# 如果不小心添加了target目录，也可以恢复到未暂存
git reset HEAD */target

```



## 取消对文件的修改

> 如果觉得刚才对文件的修改完全没有必要，可以取消修改，回到之前的状态
>
> 比较新的版本可以使用git restore命令

```bash
# 将xxx.java恢复到之前的状态
git checkout -- xxx.java

# 将所有后缀以java结尾的文件恢复到之前状态
git checkout -- *.java
```



## 修订版本



### 检视提交

```bash
git show [commit id]
```





### 引用日志

git在后台保存一个引用日志(reflog),引用日志记录了最近几个月HEAD和分支所指向的历史。可以使用`git reflog`查询引用日志，每当HEAD锁指向的位置发生了变化，git就会将这个信息存储到引用日志的记录里。

引用日志只存在于本地仓库，只记录在自己的仓库中做过什么的日志。

```bash
git reflog

# 查看仓库中HEAD在五次前的所指向的提交，可以使用@{n}来引用reflog中输出的提交记录
git show HEAD@{5}
```





## Git工具



### 储藏与清理

如果我们在项目的一部分上已经工作一段时间了，这时候想切换到别的分支做一点别的事情，但又不想创建一次提交。就可以使用`git stash`命令。

```
# 储藏修改 git stash save命令已经被废弃了，但是暂时不会从git中删除
git stash 或 git stash push

# 不仅储藏所有已暂存的内容，同时还要将它们保留在索引中
git stash --keep-index

# 默认git只会储藏已修改或已跟踪的文件，要储藏未跟踪的文件使用-u
git stash -u 

# 要储藏忽略文件中明确忽略的文件
git stash -a

# 查询储藏的信息
git stash list 

# 将刚刚储藏的工作重新应用
git stash apply

# 将指定储藏的工作重新应用
git stash apply stash@{2}

# 应用储藏并移除它
git stash pop

# 移除储藏
git stash drop stash${0}
```



### 清理工作目录

`git clean`命令可以去除冗余或清理工作目录。

```bash
# 移除工作目录中所有未追踪的文件以及空的子目录
git clean -f -d

# 只是看看它会做什么，不会真的移除
git clean -d -n

# 默认情况下，git clean只能移除没有忽略的未跟踪文件。任务与.gitignore或其它忽略文件中的模式匹配的文件都不会被移除。如果也想移除，加-x参数
git clean -d -x -n
```





### 文件标注

如果在追踪代码中的一个bug，并且想知道是什么时候以及为何会引入，文件标注通常是最好用的工具。它能显示任务文件中每行最后一次提交的提交记录。

```bash
# 查询文件标注
git blame 文件名

# 查询10-20行的文件标注
git blame -L 10,20 文件名

# 可以尝试找出文件中从别的地方复制过来的代码片段
git blame -C 文件名
```









## 远程仓库



### 查看当前远程仓库

要查看和配置有哪些远程仓库可以使用`git remote`命令,它会列出每个远程仓库的简短名字。在克隆完某个项目后，至少可以看到一个名为origin的远程库，Git默认使用这个名字来标识识别我们所克隆的原始仓库。

```bash
# 显示简短的远程仓库名字
git remote

# 显示对应的克隆地址
git remote -v 

# 查看origin仓库的详细信息
git remote show origin
```



### 添加远程仓库

```bash
git remote add origin git@gitee.com:luozongle/note.git
```



### 远程仓库的删除和重命名

> 使用git remote rename 修改远程仓库在本地的简称，使用git remote rm 删除远程仓库
>
> 例:将pb修改为paul，删除paul

```
git remote rename pb paul

git remote rm paul
```



### 从远程仓库抓取数据

> 此命令会抓取上次colone或者fetch以来别人提交的更新。fetch命令只是将远端的数据拉到本地仓库，并不会自动合并到当前工作分支。git fetch会更新remote索引

```bash
git fetch [remote-name]
```



### 推送数据到远程仓库

> 可以将本地仓库的数据推送到远程仓库
>
> 例:将本地仓库推送到origin仓库的master分支

```bash
git push origin master
```





## 标签



### 列出现有标签

> 列出现有标签，显示的表情按字母顺序排序

```bash
git tag

# 仅显示v1.0.开头的标签
git tag -l 'v1.0.*'

# 显示相应标签的版本信息，并且连同显示打标签时的提交对象
git show v1.2
```



### 分享标签

> 默认情况下，git push 并不会把标签传到远端服务器上，只有通过显示的命令才能分享标签到远端服务器

```bash
git push origin v1.5
```









































