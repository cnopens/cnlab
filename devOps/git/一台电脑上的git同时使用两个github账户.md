#一台电脑上的git同时使用两个github账户.md
---
解决方案
一、生成两个SSH key
这里以两个账号的配置为例，多于两个账号的情况配置和两个账号一样，所以学会了两个账号怎么配置了，再多账号也是一样可以顺利配置成功的。

根据你的Github账号，分别生成对应的key。为了方便举例，这里使用“one”和“two”两个账户。下同。

生成SSH key的具体命令如下：

ssh-keygen -t rsa -C "cxf@gmail.com"
ssh-keygen -t rsa -C "ds@gmail.com"

ssh-keygen是linux命令，可以让两个机器之间使用ssh而不需要用户名和密码。

运行上面命令需要注意几点：

*运行命令后不要一路回车，分别在第一次对话出现“Enter file in which to save the key”的时候输入文件名（此处文件名为id_rsa和id_rsa_two）
第二次会话是让你输密码，一般回车密码设置为空就好了
第三次再次确认密码，同样回车.

两份包含私钥和公钥的4个文件，后缀为.pub的文件为公钥文件。

2.linux或mac用户一定要在~/.ssh路径下运行命令行，不然生成的文件不会出现在当前目录，Windows用户则在“C:\Users\用户名\.ssh”目录下运行命令行。

二、创建config文件并配置

继续在.ssh目录下创建config文件，在config文件中添加以下内容：
# one(one@gmail.com)
Host one.github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_one
User one
    
# two(two@gmail.com)
Host two.github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_two
User two

这里说明一下配置各字段的含义
---
Host myhost（这里是自定义的host简称，以后连接远程服务器就可以用命令ssh myhost）
HostName 主机名可用ip也可以是域名(如:github.com或者bitbucket.org)
Port 服务器open-ssh端口（默认：
每个账号单独配置一个Host，每个Host要取一个别名，一般为每个Host主要配置HostName和IdentityFile两个属性，配置完保存即可。22,默认时一般不写此行）
PreferredAuthentications   配置登录时用什么权限认证--可设为publickey,password publickey,keyboard-interactive等
IdentityFile 证书文件路径（如~/.ssh/id_rsa_*)
User 登录用户名(如：git)

每个账号单独配置一个Host，每个Host要取一个别名，一般为每个Host主要配置HostName和IdentityFile两个属性，配置完保存即可。


for example :
ssh-keygen -o -t rsa -b 4096 -C "caixiaofeng@dashuoinfo.cn"



gpg --full-gen-key