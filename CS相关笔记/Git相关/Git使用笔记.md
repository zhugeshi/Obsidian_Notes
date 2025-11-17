本篇文档,(不)完全使用ai编写(笑),用于记录我使用git的一些过程
# 常规远程同步流程
- git init (用于初始化本地仓库)
- git add . (将当前仓库下所有的文件添加到git缓冲区)
- git commit (提交修改,并编写注释commit)
- 在github上新建仓库
- git remote add orgin <仓库地址(https)> (给当前本地git仓库添加一个远程仓库,并取名为origin)
	- **git remote -v 可以显示这一步我们添加的对应远程仓库**
- git push -u origin main (将当前分支和origin/main分支绑定,并推送到远程仓库)
# Git log的使用
- `git log` 默认查看历史提交。
- 常用简化：`git log --oneline`
- 可视化：`git log --graph --oneline --all`
- 可加过滤：`--author`、`--grep`、`--since`、`-- <file>`

```shell
将命令映射一下,可以少敲一点代码:
git config --global alias.lg "log --oneline --graph --all --decorate"

现在输入以下命令即可:
git lg
```

# 踩的一些坑
```shell
PS D:\Obsidian\mynote> 
git push fatal: The current branch master has no upstream branch. To push the current branch and set the remote as upstream, use git push --set-upstream origin master To have this happen automatically for branches without a tracking upstream, see 'push.autoSetupRemote' in 'git help config'.
```

这段的意思是当前分支(master)没有远程分支(upstream branch)对应,需要绑定关系才能知道push的是哪个分支

方案一:

```shell
首次进行这个命令
git push --set-upstream(或者-u) origin master

今后只需要即可
git push
```

但是如果远程仓库的名字是origin main,就需要进行映射

```shell
语法是:
git push <远程名> <本地分支>:<远程分支>

所以执行一下命令:
git push --set-upstream(或者-u) origin master:main
```

或者重命名当前本地分支

```shell
重命名
git branch -M main

然后再绑定推送到远程分支:
git push -u origin main
```