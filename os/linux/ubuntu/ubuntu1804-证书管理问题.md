
#证书问题

ubuntu系统上如何添加新的根证书

如果自己部署了一个CA系统，或者使用openssl生成了一个自签名的证书，如何让ubuntu系统信任这些证书呢

添加证书：
首先，复制pem格式的根证书，重命名为 .crt格式
然后，执行下边的命令
$sudo cp 证书路径.crt /usr/local/share/ca-certificates
$sudo update-ca-certificates

update-ca-certificates命令将PEM格式的根证书内容附加到/etc/ssl/certs/ca-certificates.crt ，而/etc/ssl/certs/ca-certificates.crt 包含了系统自带的各种可信根证书.

删除证书：
$sudo rm -f /usr/local/share/ca-certificates/证书名称.crt
$sudo update-ca-certificates
