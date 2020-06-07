#powershell-program.md


linux，*unix 发行版的儿子（默认 shell ）都是谁？
答：
freebsd 的默认 shell=sh
ubuntu=dash
红帽等=bash
osx=我不知道，但是 zsh 将继位。
最新消息，下一个版本 osx，zsh 将是默认 shell。

---
在 Linux 中，如何以 100%兼容 bash 命令和脚本的情况下，主用 powershell (个人评价： 在linux下用powershell,我是安装powervmware-cli)

------ [概述] ------

问：如何看待 bash，及 linux shell 脚本将来的地位，命运？
问：powershell 在 linux 中的前景如何？
答：
就好像 [气泵射钉枪] 必将取代 [锤子] 一样，先进生产力必然代替落后的。
就好像面向对象的 powershell，必然取代面向字符的 bat 那样。
powershell 发展成熟后。以 bat，bash 为代表的，上一代面向字符串的脚本语言，面向字符串的命令，难免被边缘化。
过几年后，开机启动脚本，特简单的脚本中，或许还残留有 bat，bash，字符串命令的身影。



问：为什么要主用 powershell，边缘化 bash ？
答：
1 解决 bash 的癌症。
2 增加功能。 [强] 
3 简化语法。 [简] 
4 速度快。 [快] 
新建万个空文本文件，linux 中的 ps，比 linux 中的 touch，快大约 10 倍 
http://tieba.baidu.com/p/5895236874

搜并看：
百万简单循环耗时，powershell 为 bash 的 3.4%，或者反过来说 bash 耗时是 powershell 的 28 倍。





问：请举个例子？
答：
bash 有杀不死的 sleep。这本身是 bash 架构问题。
很多 bash 难解决的问题，只要把部分代码改成，调用 ps1 脚本返回结果，就根本不存在了。



问：在 linux 中主用 powershell 难么？
问：在 linux 中主用 powershell 改动大么？
答：
不难，改动小。
1 简单来讲，bash 中只有语法，没有命令和库。
2 bash 只有 1%的语法功能，powershell 实现不了。这很正常，世界上没有两片叶子是完全一样的。


问：世界上有 100%兼容 bash 的 shell 吗？
答：
世界上没有两片叶子是完全一样的。sh 和 bash 也不能 100%兼容。


问：在 linux 中主用 powershell。是否需要把默认 shell，bash 替换成 powershell ？
答：
不需要或不建议。




------ [原则和流程] ------
学用 pwoershell 语法，
学用 powershell 库，
在 bash 脚本中嵌入，powershell 命令或脚本。
在 ps1 脚本中嵌入，bash 命令或脚本。
并逐渐代用 /usr/bin/pwsh 代替 /usr/bin/bash。




------ [具体做法] ------

问：如何在.ps1 脚本中，嵌入 [ 100%兼容 bash 的] shell 命令？
答：
=====================
$bashcmd =
@'
echo '我是 bash 命令'
# linux-command |awk yyy |sed zzz
echo '命令中可以有单引号'
echo "命令中可以有双引号"
echo '如需解析变量，则用这种括号，注意头尾必须换行'
echo '@\"'
echo '$a'
echo '\"@'
'@
$powershell 变量 = /usr/bin/bash -c $bashcmd
#需要转义，有点不好
=====================
或
$powershell 变量 = 
@'
echo '我是 bash 命令'
# linux-command |awk yyy |sed zzz
echo '命令中可以有单引号'
echo "命令中可以有双引号"
echo '如需解析变量，则用这种括号，注意头尾必须换行'
echo '@"'
echo '$a'
echo '"@'
'@ | /usr/bin/bash
#不需要转义，推荐
=====================


问：如何在.sh 中，嵌入 powershell 脚本？
答：
aaa=`/usr/bin/pwsh -f xxx.ps1 -参数 1 'aaa' -参数 2 777`



问：如何在.sh 中，嵌入 powershell 命令？
答：
aaa=`/usr/bin/pwsh -c "某 powershell 命令"`



问：如何在 ssh 远程连接中，使用 powershell 对象，而不是 bash 字符串？
问：如何让 powershell 成为默认的远程连接 shell ？
答：
在这个文件中 /etc/ssh/sshd_config
添加一行：
Subsystem powershell /usr/bin/pwsh -sshs -NoLogo -NoProfile

注意：这么做，不会对你经 ssh，远程使用 bash 命令，产生任何影响！
不影响！没影响！

你不用手动添加，下面安装命令已经给你做好了。






------ [安装 linux 版 powershell ] ------

centos7 及以上，安装 powershell:
curl -o /etc/yum.repos.d/microsoft.repo https://packages.microsoft.com/config/rhel/7/prod.repo 
sudo yum remove -y powershell #删除旧版 
sudo yum install -y powershell
pwsh -c 'mkdir -p "$env:HOME/.config/powershell" '
pwsh -c 'Add-Content -Value "Set-PSReadlineOption -EditMode Windows" -LiteralPath $profile '
pwsh -c 'Add-Content -Value "`nSubsystem powershell /usr/bin/pwsh -sshs -NoLogo -NoProfile" -LiteralPath /etc/ssh/sshd_config '



ubuntu1604：
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/16.04/prod.list
sudo apt-get update
sudo apt-get remove -y powershell #删除旧版 
sudo apt-get install -y powershell
pwsh -c 'mkdir -p "$env:HOME/.config/powershell" '
pwsh -c 'Add-Content -Value "Set-PSReadlineOption -EditMode Windows" -LiteralPath $profile '
pwsh -c 'Add-Content -Value "`nSubsystem powershell /usr/bin/pwsh -sshs -NoLogo -NoProfile" -LiteralPath /etc/ssh/sshd_config '


ubuntu1804：
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
sudo apt-get update
sudo apt-get remove -y powershell #删除旧版 
sudo apt-get install -y powershell
pwsh -c 'mkdir -p "$env:HOME/.config/powershell" '
pwsh -c 'Add-Content -Value "Set-PSReadlineOption -EditMode Windows" -LiteralPath $profile '
pwsh -c 'Add-Content -Value "`nSubsystem powershell /usr/bin/pwsh -sshs -NoLogo -NoProfile" -LiteralPath /etc/ssh/sshd_config '



debian9：
sudo apt-get update
sudo apt-get install curl gnupg apt-transport-https
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-debian-stretch-prod stretch main" > /etc/apt/sources.list.d/microsoft.list'
sudo apt-get update
sudo apt-get remove -y powershell #删除旧版 
sudo apt-get install -y powershell
pwsh -c 'mkdir -p "$env:HOME/.config/powershell" '
pwsh -c 'Add-Content -Value "Set-PSReadlineOption -EditMode Windows" -LiteralPath $profile '
pwsh -c 'Add-Content -Value "`nSubsystem powershell /usr/bin/pwsh -sshs -NoLogo -NoProfile" -LiteralPath /etc/ssh/sshd_config '


安装方法：
https://docs.microsoft.com/zh-cn/powershell/scripting/setup/installing-powershell-core-on-linux?view=powershell-6


