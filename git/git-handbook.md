#### Git Property
- 创始人：Linus Torvalds
- 诞生: 2005
- 完全分布式
- 对非线性开发模式的强力支持

#### Difference with other CVCS
- 对所有文件制作一个快照来保存信息，没有变化的文件在快照中只保留一个链接之前之前存储的文件。
- 本地有项目的完整历史，基本所有操作都可以本地执行，不需要网络开销，速度快。
- 在数据存储前会做校验和（SHA-1散列），当作索引。

#### Config
- git config --list 查看配置列表
- .git/config , ~/.gitconfig, /etc/gitconfig 一层层覆盖

#### Command
- git status -s 查看简明的状态信息
- git diff 查看工作区变化
- git diff -cached/-staged 查看暂存区变化
- git difftool 使用差异工具查看变化
- git mergetool 使用可视化合并工具进行合并操作
- git commit -a 跳过暂存步骤，直接将所有已跟踪的文件暂存起来并提交
- git rm --cached filename 从仓库删除，但不删除本地工作目录的文件
- git mv file_from file_to 文件重命名
- git log 输出方式
  - -p 按补丁格式显示每个更新之间的差异。
  - --stat 显示每次更新的文件修改统计信息。
  - --decorate 显示分支指向的提交对象
  - --shortstat 只显示 --stat 中最后的行数修改添加移除统计。
  - --name-only 仅在提交信息后显示已修改的文件清单。
  - --name-status 显示新增、修改、删除的文件清单。
  - --abbrev-commit 显示 SHA-1 的前几个字符，而非所有的 40 个字符。
  - --relative-date 使用较短的相对时间显示（比如，“2 weeks ago”）。 
  - --graph 显示 ASCII 图形表示的分支合并历史。
  - --pretty 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。
  - --pretty=format, 格式参数如下：
    - %H    提交对象（commit）的完整哈希字串
    - %h    提交对象的简短哈希字串
    - %T    树对象（tree）的完整哈希字串
    - %t    树对象的简短哈希字串
    - %P    父对象（parent）的完整哈希字串
    - %p    父对象的简短哈希字串
    - %an   作者（author）的名字
    - %ae   作者的电子邮件地址
    - %ad   作者修订日期（可以用 --date= 选项定制格式）
    - %ar   作者修订日期，按多久以前的方式显示
    - %cn   提交者（committer）的名字
    - %ce   提交者的电子邮件地址
    - %cd   提交日期
    - %cr   提交日期，按多久以前的方式显示
    - %s    提交说明
- git log 输出范围
  - -(n) 仅显示最近的 n 条提交
  - --since, --after 仅显示指定时间之后的提交。
    - 2.weeks 近两周内
    - 2008-01-15 某一天
    - 2 years 1 day 3 minutes ago 多久以前
  - --until, --before 仅显示指定时间之前的提交。
  - --author 仅显示指定作者相关的提交。
  - --committer 仅显示指定提交者相关的提交。
  - --grep 仅显示含指定关键字的提交
  - -S 仅显示添加或移除了某个关键字的提交
  - -- \[path\] 某路径下的提交, 必须放在最后
- git commit --amend 修改上次提交内容信息
- git reset HEAD \<file\>... 取消暂存
- git checkout -- \<file\>... 丢弃本地工作目录中的文件修改
- git remote 列出远程仓库列表
- git remote -v 远程仓库名及URL列表
- git remote add \<shortname\> \<url\> 添加远程仓库
- git fetch \[remote-name\] 拉取远程仓库所有分支数据，需手动合并本地更改
- git clone 会默认将远程仓库命名为 origin，并自动设置本地master分支跟踪远程master分支
- git pull 拉取本地分支跟踪的远端分支并合并到本地
- git push \[remote-name\] \[branch-name\] 推送项目分支到远端仓库
- git remote show \[remote-name\] 查看远程仓库信息 
- git remote rename \[oldname\] \[newname\] 重命名远程仓库
- git remote rm \[remote-name\] 移除远程仓库
- git tag 列出已有标签
- git tag -l 'v1.8.5*' 只列出固定版本系列标签
- git show v1.4 显示某一标签的详细信息
- git tag -a v1.2 9fceb02 -m '' 对某一版本打标签
- git push \[remote-name\] \[tagname\] 推送标签到远程仓库
- git tag -d \<tagname\> 删除本地标签
- git push \<remote-name\> :refs/tags/\<tagname\>更新远程仓库标签
- git checkout tagname 检出标签版本，这样检出后仓库会处于detacthed HEAD（分离头指针）状态，提交的修改不会反应到标签版本名，而是会提交到一个无法访问的地方。
- git checkout -b branchname tagname, 会创建基于标签的分支，用来修复旧版本问题
- git checkout branchname 切换分支
- git checkout -b branchname origin/branchname 新建本地分支跟踪远程分支
- git branch branchname 创建分支
- git branch -d branchname 删除分支
- git push origin --delete branchname 删除远程分支
- git merge branchname 合并分支
- git branch --merged 查看已合并的分支，通常可以删除掉
- git branch --no-merged 查看未合并的分支，不能被删除掉
- git branch -vv 查看本地分支所有跟踪分支
- git ls-remote 显示远程分支完整列表
- git 命令别名：
  - git config --global alias.co checkout
  - git config --global alias.br branch
  - git config --global alias.ci commit
  - git config --global alias.st status
  - git config --global alias.unstage 'reset HEAD --'
  - git config --global alias.last 'log -1 HEAD'

#### rebase
变基，另一种合并，可以使提交历史更加整洁，但要保证被变基的分支除了自己的仓库外没有其他副本。
##### 普通变基合并
1. git rebase master branch1 将branch1从与master共同的祖先到最新提交的提交历史，在master分支上重放
2. git checkout master
3. git merge branch1
##### 截取变基合并
1. git rebase --onto master branch1 branch2 将branch2从与branch1共同祖先到最新提交的提交历史，在master重放
2. git checkout master
3. git merge branch2
##### 在一个被变基后强制推送的分支上再次变基
这是在没有符合`保证被变基的分支除了自己的仓库外没有其他副本`的前提下进行变基后，副本引用者有基于变基前版本的提交 的解决办法。
1. git rebase origin/master
2. git pull --rebase 等同于 git fetch 然后 git rebase origin/master
   
*金科定律：只要你把变基命令当作是在推送前清理提交使之整洁的工具，并且只在从未推送至共用仓库的提交上执行变基命令，就不会有事。*
#### .gitignore 的格式规范如下：

- 所有空行或者以 ＃ 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式（shell 所使用的简化了的正则表达式）匹配。
- 匹配模式可以以（/）开头防止递归。
- 匹配模式可以以（/）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

#### Problem & Solution
- [没有改变文件， 但却是已修改状态](https://dzone.com/articles/git-showing-file-modified-even)