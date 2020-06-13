#centos7-3行命令安装powershell.md

---

## Register the Microsoft RedHat repository
curl https://packages.microsoft.com/config/rhel/7/prod.repo | sudo tee /etc/yum.repos.d/microsoft.repo

## Install PowerShell
sudo yum install -y powershell

yum install -y https://github.com/PowerShell/PowerShell/releases/download/v7.0.0/powershell-lts-7.0.0-1.rhel.7.x86_64.rpm


# #Start PowerShell
pwsh


---

#Ubuntu install powershell
snap install powershell --classic


