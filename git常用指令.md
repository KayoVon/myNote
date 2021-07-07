# git常用命令

## 下载安装

```js
具体可参考https: //www.runoob.com/git/git-install-setup.html
```

## 全局配置环境

```js
配置个人用户名和电子邮箱
git config– globle user.name "runoob"
git config– globle user.email text @runoob.com

查看配置信息
git config--list
git config user.name
git config user.email
```

## 本地建仓

```js
在文件夹下创建新仓库
git init

克隆本地仓库
git clone / path / to / repository 克隆本地仓库
```

## 工作流

本地仓库由git维护的"三棵树"组成.
工作区|暂存区|当前分支
------|----|-----
本地做修改|git add file 把文件添加到暂存区|git commit -m "描述" 把暂存区的内容提交到当前分支

* 第一棵树: 工作目录, 指实际文件.
* 第二棵树: 暂存区, 临时缓存改动
    

```js
    // 添加全部文件或指定文件到暂存区
    git add .
    git add test.js
```

* 第三棵树: 当前分支, 保存着每一次提交记录
    

```js
    // 提交暂存区内容
    git commit - m "description"
```

## 查看工作区状态

```js
git status

1） 状态一： 修改了， 没有添加到缓存区（ 红色）， 此时可以通过 `git diff`
查看修改了的内容
2） 状态二： 修改了， 添加到缓存区（ 绿色）
3） 状态三： On branch master nothing to commit, work tree clean 表明无修改内容
```

## 版本回退

* 显示提交日志

```js
git log[可选参数：--pretty = oneline]

1） 显示由最近到最远的提交记录
2） 一大串十六进制表示每次提交的版本号
3） HEAD指针 指向当前版本(commit)
4） 上一个版本就是 HEAD ^ 或 HEAD~1
```

* 回退到上一个版本

```js
git reset--hard HEAD~1
```

* 回退之后要找回之前的最新版本

```js
git reset--hard 版本号
版本号不用写全， git会自动去找

如果不知道版本号， 可以查看命令历史
git reflog
```

## 撤销修改

* 场景一: 修改了文件但是未被 add 到暂存区

```js
git checkout-- < file > 工作区的修改撤销到最后一次add前
```

* 场景二: 修改了工作区内容, 还 add 到了暂存区时, 想丢弃修改, 分两步

```js
git reset HEAD < file > 撤销本文件在暂存区的所有保存， 回到场景一
git checkout-- < file > 工作区的修改撤销回上一次提交版本
```

* 场景三: 修改文件已被commit, 但是没有推送到远程库, 想要撤销本次提交, 要回退到上一版本

```js
git reset--hard HEAD ^ 回退到上一次提交的版本
```

* 场景四: 想丢弃本地所有改动和提交, 可以去服务器获取最新的版本历史, 并将本地主分支指向它

```js
git fetch origin
git reset--hard origin / master
```

## 删除文件

* 从版本库删除文件

```js
git rm < file > 删除本地文件
git commit - m "description"
提交删除
```

* 误删, 版本库中还没提交删除, 需要复原

```js
git checkout-- < file >
```

## 远程库

* ssh配置

* 关联一个远程库:origin是默认习惯的远程仓库的别名

```js
git remote add origin < 远程库地址 >
```

* 如果项目是多人合作的, 那么就需要在拉去别人更新的代码获取(fetch)到本地. Git会自动合并(merge) 到当前分支(master).

```js
git pull
```

* 把本地库的所有内容推送到远程库

```js
git push - u origin master
第一次推送， - u把origin指定为默认主机， 之后就可以简化指令 git push
```

* 删除远程库关联

```js
git remote - v 查看远程库信息
git remote rm < name > 根据库名字删除
```

* 克隆远程库

```js
git clone < 远程库地址 >
```

## 分支管理

* 本地库里, 每次提交, Git都把它们串成一条时间线, 这条时间线就是一个分支.
* 创建仓库的时候默认主分支 **master**.
* **master** 指向最新提交, **HEAD** 指向当前分支 **master**(所以HEAD是间接指向最新提交的), 每次提交, master分支都会向前移一步.

* 创建并切换到新分支: 创建 **新分支dev**, 新建了一个dev指针指向master相同提交, 再把HEAD指向dev, 表示当前正在dev分支上
    

```js
    git checkout - b dev
```

* 接着 add 和 commit 都是针对dev分支, master指针不会走
* 切换回主分支, 并将dev合并进来: 让master成为主分支, HEAD指针指向master, 合并分支即 **让master指向dev所指的提交**
    

```js
    git checkout master
    git merge dev

    这是一种快速合并 ** fast forward ** ，
        因为master还在原来的位置，
    所以合并分支只需要加上dev这一段。
    如果master也有更新， 就可能出现冲突
```

* 删除分支
    

```js
    git branch - d dev
```

## 解决冲突

会发生冲突的情况:

> 原来是master分支, 现在开发新分支dev, 在新分支上提交 file.txt 的修改; 
>
> 切换回master分支, 在master分支上提交 不同的 file.txt 的修改
>
> 这时候如果去合并 `git merge dev` 就会显示有冲突, 打开冲突的文件里面会提示哪些地方有冲突
>
> 需要手动修改之后重新提交, 然后合并. `git log --graph [可选:--pretty=oneline --abbrev-commit]` 可以看到分支合并情况

## 分支管理策略

1、 **合并**

**fast forward**:
* 合并分支时, 默认 `fast forward` 模式(直接让master指针指向新分支), 这样删除分支后, 会丢失分支信息, 看不出来曾经做过合并

**禁用fast forward**:
* 禁用 **快速合并**, 在 merge 时会生成新 commit, 会保留分支信息
* merge 的时候, 加上参数 `--no-ff` 禁用快速合并. 因为要生成新的commit, 所以要加上 `-m` 参数写上描述信息
    

```js
    ( * master dev)
    git merge--no - ff - m "merge with no-ff"
    dev
```

2、 **突发情况处理**

* 哪条分支需要修复bug, 就在这条分支上创建bug分支, 修复之后合并
    

```js
    git
    switch master 切换到需要修复bug的分支
    git checkout - b bug - 1 创建bug分支
        ...做修改、 add、 commit
    git
    switch master
    git merge--no - ff - m "merge bug-1 fix"
    bug - 1 合并bug分支
```

* 这之前如果工作区还有未保存的分支dev, 可以暂存, 等突发情况解决了再取出
    

```js
    git stash
        ...解决bug
    git
    switch dev
    git stash list 查看暂存工作现场
    git stash apply[可指定某条stash] 恢复工作
    git stash drop 删除dash内容
    或者
    git stash pop
```

* 如果master上的bug, dev也有, 要把修复bug的 commit 复制到dev,  `cherry-pick` 指令
    

```js
    （
    master
        *
        dev）
    git cherry - pick < commit id >
```

3、 增加新功能
* 另开分支, 然后add、 commit、 合并
* 新功能取消了, 强行删除未合并的分支
    

```js
    git branch - D newBranch
```

4、 多人协作

* 获取远程库信息

```js
git remote - v
```

* 多人协作模式

```
1)尝试推送本地分支
git push origin <branch-name> 

2)如果远程分支比本地更新就无法推送，先拉过来尝试本地合并
git pull

3)如果提示本地分支dev和远程origin/dev没有链接，则创建链接
git branch --set-upstream-to=origin/dev dev
    或者最开始本地创建dev分支的时候就直接关联
    git checkout -b dev origin/dev

4)创建好链接再 git pull 抓取远程的新提交

5)解决冲突，重新commit

6)git push origin <branch-name>  推送dev分支
```

5、 rebase

[参考链接](https://www.liaoxuefeng.com/wiki/896043488029600/1216289527823648)

分支多了之后git提交历史交叉难看, rebase可以让提交历史变成一条直线

```js
git rebase
```

## 标签管理

标签其实就是指向某一次 commit 的指针, 为了方便地找出某一版本

```js
git checkout < branchname > 切换到需要打标签的分支
git tag < tagname | 默认HEAD > [可选： 某次commit id][可选： - m“ description”]

git tag 查看所有标签
git show < tagname > 查看具体标签
git tag - d < tagname > 删除标签
git push origin < tagname > 推送标签
git push origin--tags 一次性推送
git push origin: refs / tags / < tagname > 删除远程标签
```

## 总结

当前分支作业时

```js
1) 查看分支： git branch
2) 创建新分支： git branch < name >
    3) 切换分支： git checkout < name > 或 git
switch master
4) 创建 + 切换到新分支： git checkout - b < name > 或 git
switch -c dev
5) 合并某分支到 当前 分支： git merge < name > （可能会冲突）
6) 删除分支git branch - d < name >
```

临时切换分支作业时

```js
1) 暂存分支工作状态： git stash
2) 查看分支存储的工作状态： git stash list
3) 恢复分支工作状态： git stash apply
4) 删除分支存储的工作状态： git stash drop
5) 恢复并删除分支存储工作状态： git stash pop
```

切换远程分支
: 当前分支branch1工作, 现在需要在分支branch2上工作, 则需要切换

```js
git fetch origin branch2(分支名)
git checkout branch2
```
