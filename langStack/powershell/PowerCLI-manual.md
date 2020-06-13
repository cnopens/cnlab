#PowerCLI-manual.md
---
一、使用指定模板批量创建虚拟机

#定义参数
param(
[string]$VMname,[string]$vmhostname,[string]$datastore,
[string]$template
)

#在命令窗口中添加powercli模块
try{
add-pssnapin vmware.vimautomation.core -ErrorAction SilentlyContinue
}
catch{}

#连接Vsphere
Connect-VIServer -server Vsphere -Protocol https -User user -Password password

foreach ($i in 1..5)
{
$fullname = $VMname +"-"+ $i
new-vm -name $fullname -template $template -host $vmhostname -datastore $datastore
}

disconnect-viserver -confirm:$false

执行文件时
.\scriptfile.ps1 VMname vmhost datastore template
#不声明参数时，必须按照param指定的顺序输入参数
or
.\screptfile.ps1 -VMname  vmname  -template template -vmhostname vmhost -datastore datastore
#对参数声明时，参数顺序可随意变动

二、批量重启正在运行具有名字相似可以进行匹配的的虚拟机

#在命令窗口中添加powercli模块
try{
add-pssnapin vmware.vimautomation.core -ErrorAction SilentlyContinue
}
catch{}

#连接Vsphere
Connect-VIServer -server Vsphere -Protocol https -User user -Password password

#定义正则表达式
$matchname="^[a-zA-Z]+\d{5}([a-zA-Z]{1,4})?(\w)?([a-zA-Z]{3})?(\d+)?"

#重启匹配的虚拟机
Get-Cluster -Name cluster |Get-VM |where {$_.Name -match $matchname -and $_.PowerState -eq "PoweredOn"} | Restart-VM -RunAsync

disconnect-viserver -confirm:$false