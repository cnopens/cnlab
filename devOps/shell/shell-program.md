# shell-program.md

## sys command

*用du命令看下文件或目录的大小

du -sh /* | sort -nr /tmp


*您还可以通过命令行(即Ubuntu终端)查看系统日志。

打开终端并输入以下命令：

$ dmesg

为了搜索包含特定关键字的消息，请使用以下命令：

$ dmesg |grep [keyword]

例如，如果要搜索所有包含单词core的消息，则可以使用以下命令：

$ dmesg |grep core

*cat

cat |grep [keyword] [location]

cat |less [keyword] 
note: grep vs less


*写入系统日志


有时，我们需要在故障排除过程中将自定义消息写入系统日志。 Gnome Log和Log File Viewer程序均构建为显示可通过终端写入的自定义消息。

打开Ubuntu终端，然后键入以下命令：

$ logger “This is a custom message”

logger -t scriptname “This is a custom message”

*清理日志

sudo -i 进入root
然后输入密码，执行： 
echo > /var/log/syslog

echo > /var/log/kern.log




## shell脚本修改jar包内文件