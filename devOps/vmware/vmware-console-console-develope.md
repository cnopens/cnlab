# vmware-console-console-develope.md


***Compatibility Notices***

The HTML Console SDK has been tested with the following Web browsers on Windows, Mac OS X, and Linux:

    Google Chrome 30+
    Microsoft Internet Explorer 10+
    Mozilla Firefox 24+
    Safari 6.1+

***Web console Develope*** 


47.92.2x2.2x8:11021

Connect-VIServer -Server 47.92.2x2.2x8 -username administrator@vsphere -Password 1qaz@WSX

Get-vmguest -Server 47.92.2x2.2x8 -vm Linux-oracle|select vmid,vmname,state,nics,ipaddress,OSFullName |ConvertTo-Json


/v1/omm



Ticket        : 8f4f597e987fd40e
CfgFile       : /vmfs/volumes/5da1cafe-609f329d-51a9-001b210fda10/Ceph03/Ceph03.vmx
Host          : 192.168.31.112
Port          : 443
SslThumbprint : EC:E6:76:06:32:7B:31:7D:E8:3D:41:DA:E6:8D:8B:6B:AA:AB:71:16
Url           : 



http://www.bubuko.com/infodetail-2966221.html?tdsourcetag=s_pcqq_aiomsg


$VCenter = "vc的FQDN"
Connect-VIServer $VCenter -User administrator@vsphere.local -Password  vc密码
$Vm = Get-VM VM名字
$Ticket = $Vm.ExtensionData.AcquireTicket("webmks")
$ESXHost = $Ticket.host
$TicketNumber = $Ticket.ticket