# ubuntu给当前用户设置免密码sudo
---
方法1

# 备份 /etc/sudoers
sudo cp /etc/sudoers .
#打开 /etc/sudoers
sudo visudo
# 在文件末尾加入
kube ALL=NOPASSWD:ALL

方法2

1. 备份sudo文件

sudo cp /etc/sudoers .

2. 添加当前用户到sudo组

注意，此文件只能用vi编辑

先尝试使用visudo编辑/vi//sudoers

sudo visudo

如果以上指令失败则使用vi打开编辑

sudo vi /etc/sudoers

找到 root　　ALL=(ALL:ALL) ALL，在下边添加类似的一行

kube　　ALL=(ALL:ALL) ALL

3. 设置当前登陆用户免密

使用visudo打开sudoers并编辑

sudo visudo

在刚才编辑的内容中加上NOPASSWD:

kube　　ALL=(ALL:ALL) NOPASSWD: ALL

 

4. 重新登录测试

sudo ls

如果不提示输入密码则配置成功

5. 通过以上步骤，Ubuntu Desk版本sudo可以免密了，如果是server版本还需要在编辑一下

sudo visudo

修改%sudo这一样，让所有sudo指令免密

%sudo    ALL=(ALL:ALL) NOPASSWD: ALL

再次重新登录验证一下。

 

kube ALL=NOPASSWD:ALL