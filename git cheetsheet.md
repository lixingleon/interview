git init: 初始化git仓库

git add -A: add all changes to staging environment

git commit -m ""

git log: 查看所有commit记录

git log --pretty=oneline 查看所有commit记录并只展示一行

```
git log --graph --pretty=oneline --abbrev-commit
```

git status: 查看文件情况

git reset --hard HEAD^: 回退到上一个版本

git reset --hard HEAD^^: 回退到上上个版本

```
git reset --hard [commit id]: 回退到具体某一个版本
```



git reflog: 查看命令历史(查看未来版本的commitId)

git branch: 列出所有分支

git branch dev: 创建名叫dev的分支

git switch/checkout dev: 切换到dev分支

git checkout -b dev: 创建名叫dev的分支并切换到dev

git switch -c dev: 创建名叫dev的分支并切换到dev

git merge dev: 将dev合并到master branch上

git branch -d dev: 删除名叫dev的分支

```
git merge --no-ff -m "" dev: 合并的同时保存分支信息
```