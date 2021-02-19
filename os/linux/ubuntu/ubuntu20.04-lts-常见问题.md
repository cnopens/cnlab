# ubuntu20.04-lts-常见问题.md

## 常见基本问题
1. root账号启用
第一步： 在终端输入命令：sudo gedit /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf

greeter-show-manual-login=true
all-guest=false

第二步： 在终端输入命令：sudo gedit /etc/pam.d/gdm-autologin 打开文件

前面加 # 注释掉第三行的 auth required pam_succeed_if.so user != root quiet_success

第三步：修改 gdm-password 文件
在终端输入命令：sudo gedit /etc/pam.d/gdm-password 打开文件

前面加 # 注释掉第三行的 auth required pam_succeed_if.so user != root quiet_success

第四步：修改 /root/.profile 文件

在终端输入命令：sudo gedit /root/.profile 打开文件

将文件末尾的 mesg n 2> /dev/null || true 这一行修改成
tty -s&&mesg n || true