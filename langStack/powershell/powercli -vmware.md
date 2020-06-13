#powercli -vmware.md


##
pwsh  /opt/test/dsicloud-rsmgr-1.0-SNAPSHOT.jar!/BOOT-INF/classes!/datacenterlist.ps1 192.168.31.33 administrator@vsphere.local 1qaz@WSX


## install PowerCLI on LINUX
http://nokitel.im/index.php/2018/03/01/installing-vmware-powercli-10-0-0-on-linux/

1. auto install

As the PowerCLI modules have been for some time on the PowerShell Gallery we can simply install them with one command:
 	
Install-Module -Name VMware.PowerCLI -Scope CurrentUser


As this version of PowerCLI handles a bit differently self-signed certs (or invalid ones) we need to ignore them:


Set-PowerCLIConfiguration -InvalidCertificateAction Ignore

Once all the above is done, here is the moment of truth – connecting as you would’ve from your Windows environment: 

Connect-VIServer ip

2. install checking
---
在一个PowerShell会话，您可以通过运行以下命令来检查的PowerShell的版本。

$PSVersionTable.PSVersion

Set-PSRepository -Name "PSGallery" -InstallationPolicy "Trusted"

接下来，运行以下命令来安装VMware.PowerCLI模块。这将发现和在PSGallery安装最新版本的模块

Find-Module "VMware.PowerCLI" | Install-Module -Scope "CurrentUser" -AllowClobber

完成后，通过运行以下命令进行检查，以确保该模块安装。

Get-Module "VMware.PowerCLI" -ListAvailable | FT -Autosize

如果你想看到所有的VMware安装的模块，运行以下命令。

Get-Module "VMware.*" -ListAvailable | FT -Autosize


当VMware.PowerCLI发布新版本，你可以运行下面的命令来更新。

Update-Module "VMware.PowerCLI"

VMware.PowerCLI现在已安装完成，可以连接到您的vCenter Server或ESXi主机

Set-PowerCLIConfiguration -InvalidCertificateAction "Ignore"


