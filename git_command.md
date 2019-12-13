### config
git config --global user.name "John Doe" // 配置全局用户名称

git config --global user.email johndoe@example.com // 配置全局用户邮箱

git config --system --unset credential.helper // 清除凭证，之后每次push/pull都要认证

git config credential.helper store // 记住凭证用户名密码

### repository operation

git remote add origin git@server-name:path/repo-name.git // 本地创建了一个仓库，然后关联一个远程库<br/>

git push -u origin master // 第一次推送master分支的所有内容<br/>
git push origin branch-name // 从本地推送分支

git pull // 抓取远程的新提交

git remote -v // 查看远程库信息

git init // 把当前目录变成Git可以管理的仓库

git log // 查看提交历史

git log --graph // 查看分支合并图

git reflog // 查看命令历史，以便确定要回到未来的哪个版本

git status // 查看仓库当前的状态

git add <file>

git commit -m <message>

### remote

git remote // 列出已经存在的远程分支<br/>
git remote -v | --verbose // 列出详细信息

git remote add [shortname] // 要添加一个新的远程仓库,可以指定一个简单的名字,以便将来引用,运行

### fetch

git fetch origin 远程分支名:本地分支名 // 从远程仓库中拉取指定分支(该方式不会主动切本地分支)

### restore

git checkout -- <file> // 改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令(实际是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。)

git reset --hard commit_id // 指向某个版本(例: git reset --hard HEAD^指向上一个版本)

git reset HEAD <file> // 不但改乱了工作区某个文件的内容，还添加到了暂存区时，该命令可以清除缓存区的记录，而 checkout -- <file> 可以丢弃工作区的修改<br/>
git reset --hard HEAD // 撤销工作目录中所有未提交文件的修改内容

git commit --amend // 修改最后一次提交注释

### branch operation

git checkout -b <name> // 创建+切换分支

git branch --set-upstream branch-name origin/branch-name // 建立本地分支和远程分支的关联

git checkout -b branch-name origin/branch-name // 在本地创建和远程分支对应的分支, 本地和远程分支的名称最好一致

git branch // 查看分支<br/>
git branch -a // 查看本地和远程分支

git branch <name> // 创建分支

git checkout <name> // 切换分支

git merge <name> // 合并某分支到当前分支

git branch -d <name> // 删除分支<br/>
git branch **-D** <name> // 强行丢弃一个没有被合并过的分支

git merge --no-ff -m "merge with no-ff" dev // 加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

### file operation
git mv -f oldfolder newfolder // 重命名文件和文件夹<br>
git add -u newfolder // -u 选项会更新已经追踪的文件和文件夹。<br>
git mv foldername tempname && git mv tempname folderName // 在大小写不敏感的系统中，如windows，重命名文件的大小写,使用临时文件名<br>
git mv -n foldername folderName // 显示重命名会发生的改变，不进行重命名操作

---

git diff // 查看修改内容<br>
git diff filename // 查看尚未暂存的某个文件更新了哪些<br>
git diff –cached // 查看已经暂存起来的文件和上次提交的版本之间的差异<br>
git diff –cached filename // 查看已经暂存起来的某个文件和上次提交的版本之间的差异<br>
git diff ffd98 b8e7b // 查看某两个版本之间的差异<br>
git diff ffd98:filename b8e7b:filename 查看某两个版本的某个文件之间的差异<br>

git rm <file> // 删除一个文件, 需要commit

git stash // 把当前工作现场储藏起来<br>
git stash list // 显示储藏的工作内容表<br>
git stash apply stash@{0} // 恢复指定的储藏内容，但是恢复后，stash内容并不删除<br>
git stash drop stash@{0} // 删除指定的储藏内容<br>
git stash clear // 删除所有缓存的stash<br>
git stash pop stash@{0} // 恢复指定的储藏内容，并把stash内容删除

