#vmware-vsphere-iaas.md


https://www.altaro.com/vmware/vsphere-vm-templates-a-complete-guide-part-2/


## VMScript 专题
https://4sysops.com/archives/using-invoke-vmscript-for-running-remote-scripts-on-vms-with-powercli/

https://www.lucd.info/2019/11/17/invoke-vmscriptplus-v3/


ChangeLinuxIP_PowerCLI.ps1
#Get Guest OS Credential
$Credential=Get-Credential
#Get VM Names (As Name Template) - Example Test-2*
$VM=Read-Host "Please Enter The VMs Template Name"
#Get IP Address Range - Example 10.1.1 or 192.168 - This IP Range Will Be Used For Replacing
$NIPAddress=Read-Host "Please Enter New IP Range"
$OIPAddress=Read-Host "Please Enter Current IP Range"
#Replace New IP Range With Old IP Range
Get-VM "$VM" | Invoke-VMScript -ScriptText 'sed -ri "s|$NIPAddress|$OIPAddress|" /etc/sysconfig/network-scripts/ifcfg-eth0 && cat /etc/sysconfig/network-scripts/ifcfg-eth0' -GuestCredential $credential
Get-VM "$VM" | Invoke-VMScript -ScriptText 'sed -ri "s|$NIPAddress|$OIPAddress|" /etc/fstab && cat /etc/fstab' -GuestCredential $credential