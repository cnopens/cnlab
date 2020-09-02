#snmp-network-notebook.md

##  install

### 安装yum源安装SNMP软件包

    1、更新yum源：

    yum clean all

    yum makecache

     yum repolist

### service install

 yum -y install net-snmp net-snmp-utils

snmpd -v

change team name:
vi /etc/snmp/snmpd.conf
 com2sec notConfigUser  default  public

 to 

 com2sec nontConfigUser  defautl  passw0rd (此为需要验证的团体名)

重启服务：
        systemctl start snmpd.service    #启动SNMP服务
        systemctl enable snmpd.service  #开机启动SNMP服务
添加防火墙端口

        firewall-cmd --state    #查看防火墙状态

        firewall-cmd --list-all

    vi /etc/firewalld/zones/public.xml

    <port protocol="udp" port="161"/>

    systemctl restart firewalld.service   #重启防火墙服务

    firewall-cmd --list-all

systemctl restart snmpd.service   #重启SNMP服务