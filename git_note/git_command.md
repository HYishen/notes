### git config
```
// 配置全局用户名称
git config --global user.name "John Doe"

// 配置全局用户邮箱
git config --global user.email johndoe@example.com

// 清除凭证，之后每次push/pull都要认证
git config --system --unset credential.helper

// 记住凭证用户名密码
git config credential.helper store
```

### repository operation

```
git init // 把当前目录变成Git可以管理的仓库

git log // 查看提交历史

git log --graph // 查看分支合并图

git reflog // 查看命令历史，以便确定要回到未来的哪个版本

git status // 查看仓库当前的状态

git add \<file\>

git commit -m \<message\>
```

### git远程仓库操作相关

#### git remote
```
// 查看远程库信息
git remote -v

// 解除了本地和远程的绑定关系。此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。
git remote rm origin

// 关联一个远程仓库，关联一个远程库时必须给远程库指定一个名字，origin是默认习惯命名；
git remote add origin git@server-name:path/repo-name.git
```

#### git push
```
// 第一次推送代码到远程分支
git push -u origin <branch name>

// 推送指定分支到远程
git push origin <branch name>
```

#### git pull
```
git pull // 抓取远程的新提交
```

#### git fetch
```
git fetch origin 远程分支名:本地分支名 // 从远程仓库中拉取指定分支(该方式不会主动切本地分支)
```

#### 创建和远程对应的分支
```
// 在本地创建和远程分支对应的分支，本地和远程分支的名称最好一致；
git checkout -b branch-name origin/branch-name
```

#### 建立本地分支和远程分支的关联
```
// 建立本地分支和远程分支的关联
git branch --set-upstream branch-name origin/branch-name
```

### 回退修改 or commit相关
```
git checkout -- \<file\> // 改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令(实际是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。)

git reset --hard commit_id // 指向某个版本(例: git reset --hard HEAD^指向上一个版本)

git reset HEAD \<file\> // 不但改乱了工作区某个文件的内容，还添加到了暂存区时，该命令可以清除缓存区的记录，而 checkout -- \<file\> 可以丢弃工作区的修改
git reset --hard HEAD // 撤销工作目录中所有未提交文件的修改内容

git commit --amend // 修改最后一次提交注释
```

### branch operation

#### git branch
```
// 建立本地分支和远程分支的关联
git branch --set-upstream branch-name origin/branch-name

// 查看分支
git branch

// 查看本地和远程分支
git branch -a

// 创建分支
git branch <branch name>

// 删除分支
git branch -d <branch name>

// 强行丢弃一个没有被合并过的分支
git branch -D <branch name>
```

#### git checkout
```
// 创建+切换分支
git checkout -b <branch name>

// 在本地创建和远程分支对应的分支, 本地和远程分支的名称最好一致
git checkout -b branch-name origin/branch-name

// 切换分支
git checkout <branch name>

// 合并某分支到当前分支
git merge <branch name>

// 加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
git merge --no-ff -m "merge with no-ff" dev
```

#### git switch
```
// 切换分支
git switch <name>

// 创建并切换分支
git switch -c <name>
```

### tag
注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

```
// 查看所有标签
git tag 

// 新建一个标签，默认为HEAD，也可以指定一个commit id
git tag <tagname>

// 新建一个标签并指定标签信息
git tag -a <tagname> -m "blablabla..."

// 删除一个本地标签
git tag -d <tagname>

// 推送一个本地标签
git push origin <tagname>
// 推送全部未推送过的本地标签
git push origin --tags
// 删除一个远程标签
git push origin :refs/tags/<tagname>
```

### file operation
```
git mv -f oldfolder newfolder // 重命名文件和文件夹
git add -u newfolder // -u 选项会更新已经追踪的文件和文件夹。
git mv foldername tempname && git mv tempname folderName // 在大小写不敏感的系统中，如windows，重命名文件的大小写,使用临时文件名
git mv -n foldername folderName // 显示重命名会发生的改变，不进行重命名操作
```

#### git diff
```
git diff // 查看修改内容
git diff filename // 查看尚未暂存的某个文件更新了哪些
git diff –cached // 查看已经暂存起来的文件和上次提交的版本之间的差异
git diff –cached filename // 查看已经暂存起来的某个文件和上次提交的版本之间的差异
git diff ffd98 b8e7b // 查看某两个版本之间的差异
git diff ffd98:filename b8e7b:filename 查看某两个版本的某个文件之间的差异
```

#### git rm
```
// 删除一个文件, 需要commit
git rm <file>
```

#### git stash
```
git stash // 把当前工作现场储藏起来
git stash list // 显示储藏的工作内容表
git stash apply stash@{0} // 恢复指定的储藏内容，但是恢复后，stash内容并不删除
git stash drop stash@{0} // 删除指定的储藏内容
git stash clear // 删除所有缓存的stash
git stash pop stash@{0} // 恢复指定的储藏内容，并把stash内容删除
```
