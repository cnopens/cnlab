## ubuntu安装应用问题.md




workstation安装:


0 将VMware的 .bundle 文件放在当前用户的根目录下（和桌面同级）。

1 赋予 .bundle 文件执行权限

sudo chmod +x VMware-Workstation-Full-14.1.1-7528167.x86_64.bundle

    1

2 安装

sudo ./VMware-Workstation-Full-14.1.1-7528167.x86_64.bundle

    1

3 卸载

sudo vmware-installer -u vmware-workstation


错误:
Gtk-Message: Failed to load module "canberra-gtk-module"解决方案?
安装gtk和gtk3即可
sudo apt install libcanberra-gtk-module libcanberra-gtk3-module



## Sorry,ubuntu18.04 experienced an internal error

sudo apt --reinstall install signon-ui
sudo apt --reinstall install signon-ui-x11
sudo apt --reinstall install signon-ui-service
sudo reboot