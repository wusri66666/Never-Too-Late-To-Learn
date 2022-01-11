# Git

## 常用命令

| 命令                                                         | 解释                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| git checkout -b 分支名 origin                                | 基于origin分支创建并切换分支                                 |
| git add 文件(只执行下面命令只会修改提交信息)<br/>git commit --amend（会和上一次提交合并） | 本地commit之后发现漏掉文件没有添加，或者提交信息写错了       |
| git rebase -i 最近第四次commit_id<br/>把pick改成squash，保存退出后编辑commit message | 本地多次commit合并（例如把最近三次合成一次）                 |
| git reflog                                                   | 查看本地git操作记录                                          |
| git reset --mixed/--soft/--hard HEAD^/commit_id	(写几个^就是撤销几次commit操作，不写默认撤销上一次) | 撤销commit操作（--soft取消了commit ，不取消add，不取消源代码修改；--mixed是默认参数，取消commit ，取消add，不取消源代码修改；--hard取消了commit ，取消了add，取消源文件修改） |
| git checkout 文件名                                          | 撤销对文件的修改（只能撤销没有add的文件）                    |
| git cherry-pick <commitHash>                                 | 将指定的提交（commit）应用于其他分支                         |
| git cherry-pick <HashA> <HashB>                              | 将多个提交（commit）应用于其他分支                           |
| git cherry-pick <HashA>..<HashB>                             | 将一系列的连续提交（commit）应用于其他分支，提交 A 将不会包含在 Cherry pick 中。如果要包含提交 A，可以使用（git cherry-pick <HashA>^..<HashB>） |

## 协同开发

~~~
先选定远程分支master，new branch from selected创建新分支a
代码开发完成后，push到远程分支a
a分支代码没问题，本地切换到master分支，选远程分支a，merge into current，把远程a分支代码合并到本地master，再push
如果同时还有b分支在开发提交，且需要合并最新的master分支，在b分支 git rebase master，解决冲突提交
~~~

## 注意

~~~
主分支合子分支时，如果和子分支在一条线上，用merge和rebase都可以，不在一条线上,如果直接merge，会产生合并记录，会有交叉的提交记录，如果不想产生上述情况，可以先把子分支rebase主分支，然后force push子分支，然后主分支去merge或者rebase子分支即可。
~~~

