#powercli-ps-script-practice.md

## Resources 

1. https://adamtheautomator.com/powercli-tutorial/
2. [模板参数部署ubt/debian] https://letitknow.wordpress.com/2014/12/15/vmware-powercli-deploy-ubuntudebian-vm-from-template-and-set-basic-parameters/





## PowerCLI Scripts and Modules for use with VCenter and ESXI

ResetHostPassword.psm1

----
Function Reset-HostPasswords {

param (
    [Parameter(Mandatory=$true)][string]$VCenter,
    [Parameter(Mandatory=$true)][string]$CurrentPassword,
    [Parameter(Mandatory=$true)][string]$NewPassword
 )

 write-output "VCenter Server: $vcenter"

# Connect to vCenter or ESXi Host and enumerate hosts to be updated
Connect-VIServer -Server $vcenter
$hosts = @()
Get-VMHost | sort | Get-View | Where {$_.Summary.Config.Product.Name -match "i"} | % { $hosts+= $_.Name }
Disconnect-VIServer -Server $vcenter -confirm:$false


foreach ($vmhost in $hosts) {
Connect-VIServer -Server $vmhost -User root -Password $CurrentPassword
Set-VMHostAccount -UserAccount root -Password $NewPassword
Disconnect-VIServer -Server $vmhost -Confirm:$False

if($?) {
Write-Output "Root password was updated on ESXi host - $vmhost"
    }
else {
Write-Output "Error: Root password was *not* changed on the ESXi host - $vmhost"
    }
}
}
----


