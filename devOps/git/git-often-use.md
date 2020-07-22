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