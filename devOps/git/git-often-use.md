#git-often-use.md
---
开始一个工作区（参见：git help tutorial）
  clone      克隆一个仓库到一个新目录
  init      创建一个空的 Git 仓库或重新初始化一个已存在的仓库

在当前变更上工作（参见：git help everyday）
  add        添加文件内容至索引
  mv        移动或重命名一个文件、目录或符号链接
  reset      重置当前 HEAD 到指定状态
  rm        从工作区和索引中删除文件

检查历史和状态（参见：git help revisions）
  bisect    通过二分查找定位引入 bug 的提交
  grep      输出和模式匹配的行
  log        显示提交日志
  show      显示各种类型的对象
  status    显示工作区状态

扩展、标记和调校您的历史记录
  branch    列出、创建或删除分支
  checkout  切换分支或恢复工作区文件
  commit    记录变更到仓库
  diff      显示提交之间、提交和工作区之间等的差异
  merge      合并两个或更多开发历史
  rebase    在另一个分支上重新应用提交
  tag        创建、列出、删除或校验一个 GPG 签名的标签对象

协同（参见：git help workflows）
  fetch      从另外一个仓库下载对象和引用
  pull      获取并整合另外的仓库或一个本地分支
  push      更新远程引用和相关的对象


## git tips 

  git commit -am "" [^RUNOOB]:one time submit

  git 如何处理ignore的加在版本内文件

  git -f add xxx file 强制添加

  gitcheck-ignore (对规则进行检查那有错)

##强制更新
注意:该命令直接放弃所有修改代码,并更新到版本库最新版本代码;

1. git fetch --all
2. git reset --hard origin/master
3. git pull


##新建项目进行远程关联操作

git init
git add ./file
git commit -m "first commit"
git remote add origin https://github.com/Java-Techie-jt/docker-jenkins-integration-sample.git (key)
git push -uf origin master   -f 表示强制覆盖


常见问题：

1. Stopping at filesystem boundary (GIT_DISCOVERY_ACROSS_FILESYSTEM not set?

git init

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=<remote>/<branch> master


 新建本地仓库，同步远程仓场景，出现git branch --set-upstream-to=origin/master master 解决方法

1.本地创建一个本地仓库 
2.关联远程端:
git remote add origin git@github.com:用户名/远程库名.git
3.同步远程仓库到本地
git pull
这个时候会报错
If you wish to set tracking information for this branch you can do so with:
git branch --set-upstream-to=origin/<branch> master
再按提示执行
git branch --set-upstream-to=origin/master master
继续报错
fatal: branch 'master' does not exist
原因:
本地仓没有在master上所以报错了
解决:
4.在本地仓切换到master
git checkout master
那么刚刚同步的文件就出来了

这里实际远程端的其他分支也同步了下来的了，但是git branch 不会展示出来
直接 git checkout 分支名 就可以直接切过去了

后记---dreamy说的
这里如果 使用 15 .git clone git@github.com:用户名/ --克隆远程仓 那么master 和分支 都会展示出来

实际: 试了也是一样的 git branch 也是看不到 分支，但是实际也是下载下来了
