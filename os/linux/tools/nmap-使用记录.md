# nmap-使用记录.md

## 查询主机类型和开发端口
1. specify dn
sudo nmap -o -Pn domainName

2. specify ip
sudo nmap -o -Pn ip

3. 查询局域网内所有主机和IP
sudo nmap -o -Pn ip(192.168.0.0/24)

***Note:*** 
Port state: open,close,filtered(被防火墙的),unfiltered