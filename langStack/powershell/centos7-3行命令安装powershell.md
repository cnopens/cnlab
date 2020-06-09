#centos7-3行命令安装powershell.md

---

## Register the Microsoft RedHat repository
curl https://packages.microsoft.com/config/rhel/7/prod.repo | sudo tee /etc/yum.repos.d/microsoft.repo

## Install PowerShell
sudo yum install -y powershell

# #Start PowerShell
pwsh

---