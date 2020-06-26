# nodejs-install-nvm-manager.md
---

## 使用NVM安装Node.js和NPM

安装Node.js和NPM的另一种方法是使用NVM是一个用于管理多个Node.js版本的工具。

1. 要安装NVM，请从GitHub下载安装脚本，为此，你将使用curl命令行。

如果你没有curl，运行以下命令安装它：

sudo apt install curl

按y确认安装并按Enter。

2. 现在，使用命令下载NVM安装脚本：

curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.1/install.sh | bash 

自动克隆NVM存储库并将NVM路径添加到ZSH或Bash之后，你将收到以下输出：

3. 启用nvm：
关闭并打开终端运行给定的命令

4. 最后，通过验证nvm版本来检查安装是否成功：

nvm --version

nvm ls-remote

这将列出所有可用的nvm版本，包括最新的LTS版本，如下面的图像：

注:lts代表长期支持，用于长时间维护的软件，并推荐作为最稳定的软件。
安装特定版本

Nvm是一个包管理器；它可以安装和管理多个Node.js版本。

要安装特定版本，请使用以下命令：nvm install并添加版本号。

例如：nvm install 10.15.2

要查看管理器上所有已安装的版本，请使用以下命令：

nvm ls

要检查当前使用的版本，请运行以下命令：

node -v

要切换Node.js (如果你已经在NVM上安装了它)的版本，请输入命令：

nvm use 8.11.1



## 从NodeSource存储库安装Node.js

或者，你可能想要从NodeSource存储库安装Node.js和NPM，方法是为Ubuntu添加它PPA (个人软件包存档)。

要启用NodeSource存储库，必须使用curl命令。

1. 如果系统上没有curl，请运行以下命令安装它：

sudo apt install curl

按y确认安装并按Enter。

2. 现在使用以下sudo bash脚本启用NodeSource：

curl -sL https://deb.nodesource.com/setup_11.x | sudo bash -

3. 下一步，使用一个命令安装Node.js和NPM：

sudo apt install nodejs

4. 通过检查Node.js和NPM的版本来验证安装。

运行以下命令：

nodejs --version

npm -version

注：安装NodeSource时，需要指定安装的Node.js版本，在上面的示例中，Node.js版本为10，但是版本11.8和6也可用。
安装开发工具

安装Node.js和NPM之后，安装开发工具，该自动化工具包允许从NPM编译和安装本机加载项。

要安装开发工具，请运行以下命令：

sudo apt install gcc g++ make

在Ubuntu上删除或卸载Node.js

要删除特定版本的node.js，请运行附加版本号的nvm uninstall命令。

例如，要卸载版本8.11.1，请运行以下命令：

nvm uninstall 8.11.1

要卸载node.js，但是保留它配置文件，请键入：

sudo apt remove nodejs 

要卸载Node.js并删除它配置文件，请使用以下命令：

sudo apt purge nodejs