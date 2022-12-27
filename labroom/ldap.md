# ldap.md


## 部分LDAP专用名词的解释

```
Objectclass
LDAP对象类，是LDAP内置的数据模型。每种objectClass有自己的数据结构，比如我们有一种叫“电话薄”的objectClass，肯定会内置很多属性(attributes)，如姓名(uid)，身份证号(uidNumber)，单位名称(gid)，家庭地址(homeDirectory)等，同时，还有一种叫“同学录”的objectClass，具备“电话薄”里的一些attributes(如uid、homeDirectory)，还会具有“电话薄”没有的attributes(如description等)。

Entry
entry可以被称为条目，一个entry就是一条记录，是LDAP中一个基本的存储单元；也可以被看作是一个DN和一组属性的集合。注意，一条entry可以包含多个objectClass，例如zhang3可以存在于“电话薄”中，也可以同时存在于“同学录”中。

DN
Distinguished Name，LDAP中entry的唯一辨别名，一条完整的DN写法：uid=zhang3(or cn=...),ou=People,dc=163,dc=com。LDAP中的entry只有DN是由LDAP Server来保证唯一的。

LDAP Search filter
使用filter对LDAP进行搜索。 Filter一般由 (attribute=value) 这样的单元组成，比如：(&(uid=ZHANGSAN)(objectclass=person)) 表示搜索用户中，uid为ZHANGSAN的LDAP Entry．再比如：(&(|(uid= ZHANGSAN)(uid=LISI))(objectclass=person))，表示搜索uid为ZHANGSAN, 或者LISI的用户；也可以使用*来表示任意一个值，比如(uid=ZHANG*SAN)，搜索uid值以 ZHANG开头SAN结尾的Entry。更进一步，根据不同的LDAP属性匹配规则，可以有如下的Filter： (&（createtimestamp>=20050301000000）(createtimestamp<=20050302000000))，表示搜索创建时间在20050301000000和20050302000000之间的entry。

Filter中 “&” 表示“与”；“!”表示“非”；“|”表示“或”。根据不同的匹配规则，我们可以使用“=”，“~=”，“>=”以及“<=”，更多关于LDAP Filter读者可以参考LDAP相关协议。

Base DN
一条Base DN可以是“dc=163,dc=com”，也可以是“dc=People,dc=163,dc=com”。执行LDAP Search时一般要指定basedn，由于LDAP是树状数据结构，指定basedn后，搜索将从BaseDN开始，我们可以指定Search Scope为：只搜索basedn（base），basedn直接下级（one level），和basedn全部下级（sub tree level）。

````


## objectClass介绍

````
LDAP中，一个条目(Entry)必须包含一个对象类(objectClass)属性，且需要赋予至少一个值。每一个值将用作一条LDAP条目进行数据存储的模板；模板中包含了一个条目必须被赋值的属性和可选的属性。

objectClass有着严格的等级之分，最顶层是top和alias。例如，organizationalPerson这个objectClass就隶属于person，而person又隶属于top。

objectClass可分为以下3类：
结构型（Structural）：如account、inetOrgPerson、person和organizationUnit；
辅助型（Auxiliary）：如extensibeObject；
抽象型（Abstract）：如top，抽象型的objectClass不能直接使用。

每种objectClass有自己的数据结构，比如我们有一种叫“电话薄”的objectClass，肯定会内置很多属性(attributes)，如姓名(uid)，身份证号(uidNumber)，单位名称(gid)，家庭地址(homeDirectory)等，这些属性(attributes)中，有些是必填的，例如，account就要求userid是必填项，而inetOrgPerson则要求cn(common name,常用名称)和sn(sure name,真实名称)是必填项。

accout内置的attributes有：userid、description、host、localityName、organizationName、organizationalUnitName、seeAlso；

inetOrgPerson内置的attributes有cn、sn、description、seeAlso、telephoneNumber、userPassword、destinationIndicator、facsimileTelephoneNumber、internationaliSDNNumber、l、ou、physicalDeliveryOfficeName、postOfficeBox、postalAddress、postalCode、preferredDeliveryMethod、registeredAddress、st、street、telephoneNumber、teletexTerminalIdentifier、telexNumber、title、x121Address、audio、usinessCategory、carLicense、departmentNumber、isplayName、employeeNumber、employeeType、givenName、homePhone、homePostalAddress、initials、jpegPhoto、labeledURI、mail、manager、mobile、o、pager、photo、preferredLanguage、roomNumber、secretary、uid、userCertificate等；

由上可见，accout仅仅预置了几个必要且实用的属性（完成登陆验证肯定是够了），而inetOrgPerson内置了非常之多的属性，例如电话号码、手机号码、街道地址、邮箱号码，邮箱地址，房间号码，头像，经理，雇员号码等等。

因此，在配置LDAP时，如果仅仅是基于验证登陆的目的，建议将objectClass类型设置为accout，而如果希望打造一个大而全的员工信息宝库，建议将objectClass设置为inetOrgPerson。

当然，对于一个Entry来说，仅仅有accout或者inetOrgPerson是不够的，在安装配置LDAP时，本文介绍了使用migrationtools工具，将Linux系统用户转化为ldif格式的文件，进而导入到LDAP中。在这个过程里，导出的user，会同时具备accout、posixAccount、shadowAccount、top这4个objectClass，而导出的group，则会同时具备和posixGroup、top两个objectClass。

上面已经写出，account的必要属性是userid，而
posixAccount的必要属性是cn、gidNumber、homeDirectory、uid、uidNumber；
shadowAccount的必要属性是uid，可选属性有shadowExpire、shadowInactive、shadowMax、shadowMin、userPassword等；
top必要属性是objectClass（可见，top和其它objectClass是继承的关系）
````

## ldap server 数据相关配置

```
openldap-server增加group分组
openldap-server安装好之后，用ldapadd增加几个目录为了管理用户和分组分组，然后就可以用ldapadmin登陆管理openldap了。

1、ldapadd -x -D cn=Manager,dc=looyu,dc=com -W -f 1basedomain.ldif
2、cat 1basedomain.ldif
##################################
dn: dc=looyu,dc=com
o: com
dc: looyu
objectClass: top
objectClass: dcObject
objectclass: organization

dn: cn=Manager,dc=looyu,dc=com
cn: Manager
objectClass: organizationalRole
description: Directory Manager

dn: ou=People,dc=looyu,dc=com
ou: People
objectClass: top
objectClass: organizationalUnit

dn: ou=Group,dc=looyu,dc=com
ou: Group
objectClass: top
objectClass: organizationalUnit
##################################
或者
2basedomain.ldif
###################################
# replace to your own domain name for "dc=***,dc=***" section
dn: dc=looyu,dc=com
objectClass: top
objectClass: dcObject
objectclass: organization
o: com
dc: looyu

dn: cn=Manager,dc=looyu,dc=com
objectClass: organizationalRole
cn: Manager
description: Directory Manager

dn: ou=Users,dc=looyu,dc=com
objectClass: organizationalUnit
ou: Users

dn: ou=Group,dc=looyu,dc=com
objectClass: organizationalUnit
ou: Group

dn: ou=Sudoers,dc=looyu,dc=com
objectClass: organizationalUnit
ou: sudoers
```1
