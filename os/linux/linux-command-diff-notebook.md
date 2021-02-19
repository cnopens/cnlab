# linux-command-diff-notebook.md

***Resources***
1. [linoxide] (https://linoxide.com/linux-command/linux-commands-os-version/)



***不同类型linux的命令异同***
----------------------------------------------------------------------------------------
1. passwd user --stdin support redhat/centos etc,but not support ubt/debian etc?

normal : echo ${VPN_PASSWORD} | passwd "${VPN_USER}" --stdin

ubuntu : echo "username:cleartext_password" | sudo chpasswd

for example: 

We add a new user and attempt to assign this password to it:

echo "Adding Unix user ${VPN_USER}"
useradd -G vpnusers -m -s /sbin/nologin $VPN_USER
if [ "$?" != 0 ]
then
    echo "Could not create user. (Are you sudo?!)"
    exit 1
fi

echo ${VPN_PASSWORD} | passwd "${VPN_USER}" --stdin 
echo "User created."



2. 随机产生密码定义 

VPN_PASSWORD=$(cat /dev/urandom | tr -dc 'A-Z0-9' | head -c 8)





