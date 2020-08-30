# Git

> 作者：子墨同学
>
> 时间：2020-04-05

## 目录

[TOC]



## 最常⽤的 Git 命令总结

### 1. 本地库初始化

*命令*：`git init`

*效果*：

![image-20200829225817887](https://gitee.com/szimo/picture_repository/raw/master/images/20200829225817.png)

*注意*：`.git` 目录中存放的是本地库相关的子目录和文件，**不要删除，也不要胡**
**乱修改**。

### 2. 设置签名

- *形式*

  - 用户名：tom

  - Email 地址：tom@outlook.com
    - 作用：区分不同开发人员的身份
    - 辨析：**这里设置的签名和登录远程库(代码托管中心)的账号、密码没有任何关系**。

- *命令*

  - *项目级别/仓库级别*：**仅在当前本地库范围内有效**
    - `git config user.name tom`
    - `git config user.email tom@outlook.com`
    - 信息保存位置：`./.git/config` 文件
    - ![image-20200829230446209](https://gitee.com/szimo/picture_repository/raw/master/images/20200829230446.png)
  - *系统用户级别*：登录当前操作系统的用户范围
    -  `git config --global user.name tom`
    -  `git config --global tom@outlook.com`
    -  信息保存位置：`~/.gitconfig` 文件 windows就是`C:\Users\用户名\.gitconfig` 文件
    - ![image-20200829231143069](https://gitee.com/szimo/picture_repository/raw/master/images/20200829231143.png)
  - *级别优先级*
    - 就近原则：**项目级别优先于系统用户级别，二者都有时采用项目级别的签名**
    - 如果只有系统用户级别的签名，就以系统用户级别的签名为准
    - 二者都没有不允许

### 3. 基本操作

#### （1）状态查看

*命令*：`git status`

*描述*：查看工作区、暂存区状态

#### （2）添加

*命令*：`git add [file name]`
*描述*：将工作区的“新建/修改”的文件添加到暂存区

#### （3）提交

*命令*：`git commit -m "commit message" [file name]`
*描述*：将暂存区的内容提交到本地库

#### （4）查看历史记录

*命令*：git log

*多屏显示控制方式*：

- 空格向下翻页
- b 向上翻页
- q 退出

![image-20200830120151501](https://gitee.com/szimo/picture_repository/raw/master/images/20200830120151.png)

*命令*：`git log --pretty=oneline`

*描述*：显示完整的commit id

![image-20200830120406355](https://gitee.com/szimo/picture_repository/raw/master/images/20200830120406.png)

*命令*：`git log --oneline`

*描述*：显示部分的commit id

![image-20200830120528042](https://gitee.com/szimo/picture_repository/raw/master/images/20200830120528.png)

*命令*：`git reflog`

*描述*：可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）

![image-20200830120732252](https://gitee.com/szimo/picture_repository/raw/master/images/20200830120732.png)

*注意*：`HEAD@{移动到当前版本需要多少步}`

#### （5）前进后退

*本质*

![image-20200830121208042](https://gitee.com/szimo/picture_repository/raw/master/images/20200830121208.png)

*基于索引值操作[推荐]*

- `git reset --hard [局部索引值]`
- `git reset --hard a6ace91`

*使用^符号：只能后退*

-  `git reset --hard HEAD^`  后退一步
- `git reset --hard HEAD^^` 后退两步
- 注：一个^表示后退一步，n 个表示后退 n 步

*使用~符号：只能后退*

- git reset --hard HEAD~n
-  注：表示后退 n 步

####  （6）reset 命令的三个参数对比

*--soft 参数*

- 仅仅在本地库移动 HEAD 指针

*--mixed 参数*

- 在本地库移动 HEAD 指针
- 重置暂存区

*--hard 参数*

- 在本地库移动 HEAD 指针
- 重置暂存区
- 重置工作区

####  （7）删除文件并找回

*前提*：删除前，文件存在时的状态提交到了本地库。

*操作*：`git reset --hard [指针位置]`

- 删除操作已经提交到本地库：指针位置指向历史记录
- 删除操作尚未提交到本地库：指针位置使用 HEAD

#### （8）比较文件差异

- `git diff [文件名]`
  - 将工作区中的文件和暂存区进行比较
- `git diff [本地库中历史版本] [文件名]`
  - 将工作区中的文件和本地库历史记录比较
- 不带文件名比较多个文件

### 4. 分支管理

#### （1）作用

在版本控制过程中，使用多条线同时推进多个任务。

#### （2）好处

- 同时并行推进多个功能开发，提高开发效率
- 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。

#### （3）分支操作

*创建分支*

`git branch [分支名]`

*查看分支*

`git branch -v`

*切换分支*

git checkout [分支名]

*合并分支*

- 第一步：**切换到接受修改的分支（被合并，增加新内容）上**
  - `git checkout [被合并分支名]`

- 第二步：执行 merge 命令

  - `git merge [有新内容分支名]`

- 例如想把iss53分支合并到master

  - ```console
    git checkout master
    git merge iss53
    ```

*新建一个分支并同时切换到该分支上*

`git checkout -b [分支名]`

它是下面两条命令的简写

```console
git branch [分支名]
git checkout [分支名]
```

*解决冲突*

- 冲突的表现

![image-20200830124033713](https://gitee.com/szimo/picture_repository/raw/master/images/20200830124033.png)

- 冲突的解决
  - 第一步：编辑文件，删除特殊符号
  - 第二步：把文件修改到满意的程度，保存退出
  - 第三步：git add [文件名]
  - 第四步：git commit -m "日志信息"
    - 注意：**此时 commit 一定不能带具体文件名**

### 5. 远程库

#### （1）创建远程库地址别名

`git remote -v` 查看当前所有远程地址别名

![image-20200830124527422](https://gitee.com/szimo/picture_repository/raw/master/images/20200830124527.png)

`git remote add [别名] [远程地址]`  添加远程库地址和别名

#### （2）推送 

 `git push [别名] [分支名]` 推送到远程库

![image-20200830125500833](https://gitee.com/szimo/picture_repository/raw/master/images/20200830125500.png)

#### （3）克隆

*命令*：`git origin [远程地址]`

![image-20200830125741371](https://gitee.com/szimo/picture_repository/raw/master/images/20200830125741.png)

*效果*

- 完整的把远程库下载到本地
- 创建 origin 远程地址别名
- 初始化本地库

#### （4）拉取

- `pull=fetch+merge`
- `git fetch [远程库地址别名] [远程分支名]`   
  - `git fetch`是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中
- `git merge` [远程库地址别名/远程分支名]
- `git pull` [远程库地址别名] [远程分支名]
  - `git pull` 则是将远程主机的最新内容拉下来后直接合并





--------------------

----------------

---------------

### 6.  其他命令

#### 分⽀操作

1. `git branch [分支名]` 创建分⽀
2. `git branch -b [分支名]` 创建并切换到新建的分⽀上
3. `git checkout [分支名]` 切换分⽀
4. `git branch` 查看分⽀列表
5. `git branch -v` 查看所有分⽀的最后⼀次操作
6. `git branch -vv` 查看当前分⽀
7. `git brabch -b [分⽀名] origin/[分⽀名]` 创建远程分⽀到本地
8. `git branch --merged` 查看别的分⽀和当前分⽀合并过的分⽀
9. `git branch --no-merged` 查看未与当前分⽀合并的分⽀
10. `git branch -d [分⽀名]` 删除本地分⽀
11. `git branch -D [分⽀名]` 强⾏删除分⽀
12. `git branch origin :[分⽀名]` 删除远处仓库分⽀
13. `git merge [分⽀1]` 合并分⽀1到当前分⽀上

#### 暂存操作

1. `git stash` 暂存当前修改
2. `git stash apply` 恢复最近的⼀次暂存
3. `git stash pop` 恢复暂存并删除暂存记录
4. `git stash list` 查看暂存列表
5. `git stash drop` 暂存名(例：stash@{0}) 移除某次暂存
6. `git stash clear` 清除暂存

#### 回退操作

1. `git reset --hard HEAD^` 回退到上⼀个版本
2. `git reset --hard ahdhs1(commit_id)` 回退到某个版本
3. git checkout -- file 撤销修改的⽂件(如果⽂件加⼊到了暂存区，则回退到暂存区的，如果⽂件加⼊到了版本库，则还原⾄加⼊版本库之后的状态)
4. git reset HEAD file 撤回暂存区的⽂件修改到⼯作区

#### 标签操作

1. git tag 标签名 添加标签(默认对当前版本)
2. git tag 标签名 commit_id 对某⼀提交记录打标签
3. git tag -a 标签名 -m '描述' 创建新标签并增加备注
4. git tag 列出所有标签列表
5. git show 标签名 查看标签信息
6. git tag -d 标签名 删除本地标签
7. git push origin 标签名 推送标签到远程仓库
8. git push origin --tags 推送所有标签到远程仓库
9. git push origin :refs/tags/标签名 从远程仓库中删除标签

#### 常规操作

1. git push origin test 推送本地分⽀到远程仓库
2. git rm -r --cached ⽂件/⽂件夹名字 取消⽂件被版本控制
3. git reflog 获取执⾏过的命令
4. git log --graph 查看分⽀合并图
5. git merge --no-ff -m '合并描述' 分⽀名 不使⽤Fast forward⽅式合并，采⽤这种⽅式合并可以看到合并记录
6. git check-ignore -v ⽂件名 查看忽略规则
7. git add -f ⽂件名 强制将⽂件提交
    git创建项⽬仓库
8. git init 初始化
9. git remote add origin url 关联远程仓库
10. git pull
11. git fetch 获取远程仓库中所有的分⽀到本地，忽略已加⼊到版本库中的⽂件
12. git update-index --assume-unchanged file 忽略单个⽂件
13. git rm -r --cached ⽂件/⽂件夹名字 (. 忽略全部⽂件) 取消忽略⽂件
14. git update-index --no-assume-unchanged file 拉取、上传免密码
15. git config --global credential.helper store

### 7. ⾼频使⽤的 Git 命令
`git log`

> 查看⽇志，常规操作，必备

```shell
#输出概要⽇志,这条命令等同于
#git log --pretty=oneline --abbrev-commit
git log --oneline
#指定最近⼏个提交可以带上 - + 数字
git log --oneline -5
#提供类似 GUI ⼯具的 log 展⽰
git log --graph --date=relative --pretty=tformat:'%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%an %ad)%Creset'
```

`git status`

> 查看⼯作区状态的命令

```shell
git status
git status -s
git status --show-stash
git checkout
```

⽤来切换到对应记录的,可以基于分⽀,提交,标签。

切提交和标签⼀般⽤来热修复或者⽼版本需要加新特性。

```shell
git checkout dev
git checkout origin/test
git checkout --track origin/feature-test
git checkout -b testbranch
git checkout -- file
git checkout .
git checkout -
```

`git commit`

> 天天打交道的命令，这⾥说⼀些很常⻅的姿势

```shell
git commit --amend --no-edit
git commit --no-verify -m "xxx"
git commit -m "xxx"
git commit -t templateFile
git commit -F
```

`git reset`

> 不得不说，代码回滚中这个命令也是⽤的很多，⽽且是 --hard

```shell
git reset --hard commit_sha1
git reset --soft commit_sha1
git reset --soft HEAD~1
git reset --mixed commit_sha1
git reset --merge commit_sha1
git reset --keep commit_sha1
```



`git revert`

> ⼀般⽤于master 的代码回滚，因为多⼈在上⾯协作，
>
> revert 可以平稳的回滚代码,但却保留提交记录,不会让协作的⼈各种冲突

```shell
git revert commit- sha1
```

`git rebase`


变基在项⽬中算是很频繁的，为什么这么说。
⽐如你开发⼀个新的 feature, 遵循最⼩化代码提交的理念。
在整个功能开发完毕的时侯，会有⾮常多的 commit，⽤ rebase 可以让我们的commit记录很⼲净
git rebase -i git-sha1|branch(HEAD)
git rebase --continue
git rebase --skip
git rebase --abort
pick: 是保留该 commit(采⽤)
edit: ⼀般你提交的东西多了,可以⽤这个把东东拿回⼯作区拆分更细的 commit
reword: 这个可以重新修改你的 commit msg
squash: 内容保留，把提交信息往上⼀个 commit 合并进去
fixup: 保留变动内容，但是抛弃 commit msg
drop: ⽤的⽐较少，⽆⽤的改动你会提交么！！！
突然发现截图还有⼏个新的⾏为，估计是新版本带来的，
从字⾯上就可以看出来⼤体的意思, 就是把回滚和打标签这些放到变基中简化操作。
温馨提⽰：
本地提交之前，最好把基准点变为需要合并的分⽀，这样提交 PR/MR 的时侯就不会冲突(本地来解决冲突)
不要在公共分⽀上变基！！！⼀变其他协作者基本都⼀堆冲突！除⾮你们有很清晰的分⽀管理机制
git merge
git merge --no-ff branchName
git pull
git pull中⽤的最多是带--rebase(-r)的⽅式(变基形式拉取合并代码),保持分⽀⼀条线。
默认的pull会⾛ff模式,多数情况会产⽣新的commit,部分参数与 merge提供⼀致。
git push
当本地分⽀存在，远程分⽀不存在的时侯，可以这样推送关联的远程分⽀
git push origin localbranch
git push -d origin branchName
git push --tags
git push --follow-tags
git push -f origin branchName
git push --force-with-lease
git remote
这个东西⽤在你需要考虑维护多个地⽅仓库的时侯会考虑，或者修改仓库源的时侯
git remote add origin url
git remote add github url
git remote set- url origin(或者其他上游域) url
git branch
该命令⽤的最多的就是删除本地分⽀，重命名分⽀，删除远程分⽀了
git branch -d branchName
git branch -M oldBranch newNameBranch
git branch -- set-upstream- to=origin/xxx
git branch -- set-upstream- to origin xxx
git stash
暂存⽤的最多时侯就是你撸代码撸到⼀半，突然说有个紧急 BUG 要修正。
或者别⼈在你这⾥需要帮忙排查代码，你这时候也会⽤到。
强烈建议给每个 stash 添加描述信息！！！
git stash save stashName
git stash -u save stashName
git stash push -m "更改了 xx"
git stash apply stash@{0}
git stash pop stash@{0}
git stash list
git stash clear
git stash drop stash@{0}
git stash show stash@{0 }
git reflog
这个命令的强⼤之处，是记录了所有⾏为，包括你 rebase,merge, reset 这些
当我们不⼩⼼硬回滚的时侯,或变基错了都可以在这⾥找到⾏为之前的commit，然后回滚。
当然这个时间回溯也只在本地有⽤，你推送到远程分⽀的破坏性改动,该凉还是得凉。
git reflog -5
git cherry-pick
这个东西你可以理解为你去买橘⼦，你会专⻔挑⼀些符合⼼意的橘⼦放到购物篮中。
你可以从多个分⽀同时挑取部分需要的 commit 合并到同⼀个地⽅去，是不是贼骚。
这货和变基有点类似，但是仅仅类似，挑过来的 commit 若是没有冲突则追加。
有冲突会中断，解决后 --continue
git cherry-pick commit- sha1
git cherry-pick master~4 master~2
git cherry-pick startGitSha1..endGitSha1
git rm
这个命令在旧的版本⽤的⽐较最多的姿势是为了重新索引.gitignore 的范围
git rm --cache -- file
git rm -r --cached .
git add .
git commit -m "xxx"
git rev-parse
这个估计⼀般⼈⽤的不是很多，可以通过这个快速获取部分git 仓库的信息
我在弄脚本的时侯就会从这⾥拿东西
git rev-parse --short HEAD --verify
git rev-parse --show-toplevel
git rev-parse --git-dir
git rev-parse --all
git diff
对于这个命令，在终端⽐对⽤的不是很频繁，除了少量改动的时侯可能会⽤这个看看。
其他情况下我更倾向于⽤ GUI ⼯具来看，因为⽐对更加直观。
总结
git 的常⽤命令其实很好掌握，很多命令都有 Linux 的影⼦。
列出来的命令都是⾼频使⽤的，或许有⼀些更骚的姿势没有摸索到，

### 8. Git 基本操作

`git init`

> 用 `git init` 在目录中创建新的 Git 仓库。 你可以在任何时候、任何目录中这么做，完全是本地化的。创建成功后会生成一个.git的子目录，所有有关你的此项目的快照数据都存放在这里。

```shell
mkdir zimo
cd zimo
git init
```

`git clone`

> 使用 `git clone` 拷贝一个 Git 仓库到本地，让自己能够查看该项目，或者进行修改。
>
> 默认情况下，Git 会按照你提供的 URL 所指示的项目的名称创建你的本地项目目录。 通常就是该 URL 最后一个 / 之后的项目名称。如果你想要一个不一样的名字， 你可以在该命令后加上你想要的名称。

```shell
git clone  https://gitee.com/learning-notes.git
```

代码下载完成后在当前⽂件夹中会有⼀个 `learning-notes` 的⽬录，通过 `cd learning-notes` 命令进⼊⽬录。

`git add`

> git add 命令可将该文件添加到缓存。

```shell
git add [file1,file2,...] # [file1,file2,...]为文件列表
git add README.md index.html #将文件README.md index.html添加在缓存
```

> 新项目中，添加所有文件很普遍，我们可以使用 `git add .` 命令来添加当前项目的所有文件。

`git status`

> `git status` 命令用于查看项目的当前状态。查看在你上次提交之后是否有修改。
>
> 参数： `-s` ，获得简短的结果输出。如果没加该参数会详细输出内容

`git diff`

> 执行 `git diff` 来查看执行 `git status` 的结果的详细信息。
>
> `git diff` 命令显示已写入缓存与已修改但尚未写入缓存的改动的区别。`git diff` 有两个主要的应用场景。

- 尚未缓存的改动：**git diff**
- 查看已缓存的改动： **git diff --cached**
- 查看已缓存的与未缓存的所有改动：**git diff HEAD**
- 显示摘要而非整个 diff：**git diff --stat**

> `git status` 显示你上次提交更新后的更改或者写入缓存的改动， 而 git diff 一行一行地显示这些改动具体是啥。

`git commit`

> 使用 `git add` 命令将想要快照的内容写入缓存区， 而执行 `git commit` 将缓存区内容添加到仓库中。

```shell
git commit -m '第一次版本提交'	
```

Git 为你的每一个提交都记录你的名字与电子邮箱地址，所以第一步需要配置用户名和邮箱地址。

```shell
git config --global user.name 'zimo'
git config --global user.email zimo@outlook.com
```

如果你觉得 git add 提交缓存的流程太过繁琐，Git 也允许你用 -a 选项跳过这一步。

```shell
git commit -am 'update README.md'
```

`git config`

> 配置开发者⽤⼾名和邮箱

```shell
git config user.name zimo
git config user.email zimo@outlook.com
```

每次代码提交的时候都会⽣成⼀条提交记录，其中会包含当前配置的⽤⼾名和邮箱。

`git reset HEAD`

> `git reset HEAD` 命令用于取消已缓存的内容。
>
> 简而言之，执行 `git reset HEAD` 以取消之前 `git add` 添加，但不希望包含在下一提交快照中的缓存。

```shell
git reset HEAD index.html #取消缓存区中的index.html 
```

执行`git reset HEAD`命令后，使用`git commit -m 'update XXX'`不会提交index.html文件

要想跳过缓存区直接提交`indxe.html`需要添加`-a`选项

```shell
git commit -am 'commit index.html'
```

`git push`

> 将本地的代码改动推送到服务器

```shell
git push origin daily/0.0.1
```

origin 指代的是当前的git服务器地址，这⾏命令的意思是把 daily/0.0.1 分⽀推送到服务器，当看到命
令⾏返回如下字符表⽰推送成功了。

```shell
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 267 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local objects.
To https://gitee.com/szimo/learning-notes.git
[new branch] daily/0.0.1 -> daily/0.0.1
```



`git pull`

> 将服务器上的最新代码拉取到本地

```shell
git pull origin daily/0.0.1
```

如果线上代码做了变动，⽽你本地的代码也有变动，拉取的代码就有可能会跟你本地的改动冲突，⼀般情况下 Git 会⾃动处理这种冲突合并，但如果改动的是同⼀⾏，那就需要⼿动来合并代码，编辑⽂件，保存最新的改动，再通过 `git add .` 和 `git commit -m 'xxx'` 来提交合并。

`git log`

> 查看版本提交记录

```shell
git log
```

通过以上命令，我们可以查看整个项⽬的版本提交记录，它⾥⾯包含了提交⼈、⽇期、提交原因等信息。

`git tag`

> 为项⽬标记⾥程碑

```shell
git tag publish/0.0.1
git push origin publish/0.0.1
```

当我们完成某个功能需求准备发布上线时，应该将此次完整的项⽬代码做个标记，并将这个标记好的版本发布到线上，这⾥我们以 publish/0.0.1 为标记名并发布，当看到命令⾏返回如下内容则表⽰发布成功了

```shell
Total 0 (delta 0), reused 0 (delta 0
)
To https://github.com/gafish/gafish.github.com.git
* [new tag] publish/0.0.1 -> publish/0.0.1
```

.`gitignore`

> 设置哪些内容不需要推送到服务器，这是⼀个配置⽂件

```shell
touch .gitignore
```

`.gitignore` 不是 Git 命令，⽽在项⽬中的⼀个⽂件，通过设置 `.gitignore` 的内容告诉 Git 哪些⽂件应该被忽略不需要推送到服务器，通过以上命令可以创建⼀个 `.gitignore` ⽂件，并在编辑器中打开⽂件，每⼀⾏代表⼀个要忽略的⽂件或⽬录，如：

```text
HELP.md
target/	
```

以上内容的意思是 Git 将忽略 HELP.md⽂件 和 target/ ⽬录，这些内容不会被推送到服务器上

`git rm`

> 如果只是简单地从工作目录中手工删除文件，运行 `git status` 时就会在 **Changes not staged for commit** 的提示。
>
> 要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除，然后提交。可以用以下命令完成此项工作

```shell
git rm <file>
```

如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 **-f**

```shell
git rm -f <file>
```

如果把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，使用 **--cached** 选项即可

```
git rm --cached <file>
```

可以递归删除，即如果后面跟的是一个目录做为参数，则会递归删除整个目录中的所有子目录和文件：

```shell
git rm –r * 
```

进入某个目录中，执行此语句，会删除该目录下的所有文件和子目录。

`git mv`

> git mv 命令用于移动或重命名一个文件、目录、软连接。

```shell
git mv README.md  NEW_README.md
```



## 参考资料
