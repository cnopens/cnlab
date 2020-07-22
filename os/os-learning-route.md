#os-learning-route.md

## linux disk space command
---
df -hl 查看磁盘剩余空间

df -h 查看每个根路径的分区大小

du -sh [目录名] 返回该目录的大小

du -sm [文件夹] 返回该文件夹总M数

du -h [目录名] 查看指定文件夹下的所有文件大小（包含子文件夹）

## 解决每次都必须source .profile才能运行问题

解决登录linux环境后每次都要source /etc/profile使环境变量生效问题
Lemon_MY 2020-07-09 16:47:24 93 收藏
分类专栏： Linux
版权

解决办法：
1.编辑~/.bashrc文件

vim ~/.bashrc

    1

2.在末尾添加如下代码

if [ -f /etc/profile ]; then
. /etc/profile
fi
