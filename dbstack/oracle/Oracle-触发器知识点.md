#Oracle-触发器知识点.md

-------------------------------------------

oracle触发器加条件判断
oracle触发器加条件判断，如果某个字段，isnode=0，那么不执行下面的方法，数据如下：

create or replace trigger tr_basestation_insert_emp
before insert on BJLT.BASESTATION
REFERENCING NEW AS new_val OLD AS old_val --在这里设置名字,然后可引用新值,旧值
for each row
  when (new_val.isnode = 0)
declare
    --local variables here
begin
    insert into BSMS.BS_INFO@TOBSMS_BETTERY_LOCAL.REGRESS.RDBMS.DEV.US.ORACLE.COM(INFOID, INFONAME, GROUPID, ADDRESS, BUILDDATE, MAINTENANCER, TEL, TEMPERATURE, RECTIFIERCUR, 
      OUTVOL, CREATETIME, SORTID, ONEOFFVOL, TWOOFFVOL, ISNODE, NODENUM, ONOFFPOWER, ONOFFPOWERMODEL, 
      POWERA, POWERB, POWERC, POWEROUT, POWERACUR, POWERBCUR, POWERCCUR, POWERAVOL, POWERBVOL, 
      POWERCVOL, DOOROPEN, HS, YANGAN, SHUIJIN, HONGWAI, KONGTIAO, VERID)
      values ('1','1','8a82fcd4-eba9-4f83-82d6-8ded897f3f10', 
       '1',sysdate,'1', 
        '1',0,'1', 
        1,sysdate,1, 
        1,1,1, 
        1,'1','1', 
        -1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,0,-1,-1,-1,-1,
        1);
end;
 

参考自：http://blog.csdn.net/weiwenhp/article/details/9179891

目录(?)[-]

什么样的操作触发trigger
使用触发器注意事项
创建触发器
row级别trigger旧值新值
 
条件判断
trigger和procedure,function类似,只不过它不能被显示调用,只能被某个事件触发然后oracle自动去调用.常用的一般 是针对一个表或视图创建一个trigger,然后对表或视图做某些操作时触发trigger.当然除此之外还有,schema,database级别的 trigger.

 

什么样的操作触发trigger
常见的是DML(insert,update,delete) , DDL(create,alter,drop)语句.

不常见的是schema级别trigger在session连接或断开时触发.database级别trigger在系统启动或退出时触发.

你可能很容易发现如果是select查询操作就没法用到trigger,而有时可能我们想监测谁查看了一些敏感信息,此时只能用到一个叫FGA的东东,可以创建一个审计策略(可以看成加强版的trigger,FGA介绍详见:http://blog.csdn.net/weiwenhp/article/details/7165660)

 

 

使用触发器注意事项
1.触发器不接受参数,一个表最多可有12个触发器(触发器类型刚好是12种),并且同一时间,同一事件,同一类型的触发器只能有一个(保证触发器操作不冲突嘛).

2.触发器最大为32KB,由于大小受到限制自然也不能使用long,blob这样的大变量.如果实在是有复杂的逻辑,要弄个很复杂的触发器,可以通过procedure或function实现一部分功能,然后调用

3.因为触发器实际上可以看作触发语句的一部分.所以得遵循一些约束条件,比如不能有事务控制语句 (commit,rollback,savepoint),DDL语句.为啥子呢,这些特殊语句与一般sql语句的最主要区别是涉及到commit的问 题.所以如果触发语句只是一般语句的话自然不能因为trigger的操作带有commit这样的特性了.

 

创建触发器
针对表或视图的触发器格式如下:

 

CREATE [OR REPLACE] TRIGGER trigger_name

{BEFORE | AFTER }

{INSERT | DELETE | UPDATE [OF column [, column …]]}

[OR {INSERT | DELETE | UPDATE [OF column [, column …]]}...]

ON [schema.]table_name | [schema.]view_name

[REFERENCING {OLD [AS] old | NEW [AS] new| PARENT as parent}]

[FOR EACH ROW ]

[WHEN condition]

PL/SQL_BLOCK | CALL procedure_name;

 

先来看一个简单的,statement级别的trigger怎么创建.假如有表tb1,每insert一点就通过trigger在tblog中记录一些信息.

create or replace trigger tb1_trigger

after insert on tb1

referencing new as new old as old

 

declare

v_info varchar2(100);

begin

v_info := "do a insert";

insert into tblog(info) values(v_info);

end;

 

trigger的创建中有个不太容易理解的内容:一针对row级别的trigger旧值新值问题

row级别trigger旧值新值
针对一个表或视图创建trigger时分为statement级别与row级别的trigger.所谓statement级别是说一个sql语句触发一次trigger,而如果是row级别则一个sql语句涉及到多行数据则trigger会被触发多次.

而旧值就是指要更改的那一行数据在被改之前的值,新值就是用户更新后值.假如表tt只有一列一行数据:88.然后用户执行语句update tt set id = 99 where id = 88;

则旧值指88,新值指99.那你们可能会问用什么方式去得到旧值或新值啊.来举例看下

假如有表tb(eno int); 和表tblog( info varchar2(100)); 假如在tb上创建trigger,tb每update一次则在tblog中记录旧值就更改后的新值.

 

CREATE OR REPLACE TRIGGER tb_trigger

BEFORE UPDATE

ON tb

REFERENCING NEW AS new_val OLD AS old_val  --在这里设置名字,然后可引用新值,旧值.如果不指定默认值为new ,old.可以通过:new或:old去引用

FOR EACH ROW

DECLARE

v_info varchar2(100);

BEGIN

v_info := 'old value:' ||to_char( :old_val.eno) || 'new value:' || to_char(:new_val.eno);

insert into tblog values(v_info);

END;

 

 
条件判断
假如只有在涉及到某一行的操作时触发trigger,假如该触发器是针对updat,delete,insert都触发的情形.咋整呢,自然是多用些when去判断啊.

例如

CREATE OR REPLACE TRIGGER tb_trigger

BEFORE UPDATE or insert or delete

ON tb

REFERENCING NEW AS new_val OLD AS old_val --在这里设置名字,然后可引用新值,旧值

FOR EACH ROW

when (old_val.eno = 99)

DECLARE

v_info varchar2(100);

BEGIN

 

case

when updating then

v_info := 'old value:' ||to_char( :old_val.eno) || 'new value:' || to_char(:new_val.eno);

insert into tblog values(v_info);

 

when inserting then

null;

 

when deleting then

null;

 

end case;

END;


##我的应用

CREATE OR REPLACE
TRIGGER TR_CONFIRM_NOTIFY BEFORE
INSERT
	ON
	IAUSER.CREATING_VM FOR EACH ROW
DECLARE
		use VARCHAR2;
BEGIN
	SELECT
	wm_concat(dd.DIC_NAME)
INTO
	use
FROM
	ZXUSER.DSI_DICT dd
WHERE
	dd.DIC_TYPE = 'resource_area'
	AND dd.DIC_VALUE = 'true';
IF
	INSTR(use,:new.VM_APP_TYPE)>0 THEN :new.CONFIRM_STATUS := '1';
END
IF;
END TR_CONFIRM_NOTIFY;




## 加入条件

传入ip如果为Null,正常走，否则调过ip检测



{
  "clmVmId": "string",
  "cluster_id": "string",
  "cluster_name": "string",
  "confirmStatus": "string",
  "creatorEmail": "string",
  "creatorName": "string",
  "creatorNo": "string",
  "creatorPhone": "string",
  "defaultGateway": "string",
  "dns": "string",
  "docCreateTime": "string",
  "docId": "string",
  "docStatus": "string",
  "host_id": "string",
  "host_name": "string",
  "id": "string",
  "portgroup": "string",
  "reqVmStatus": "string",
  "subnetMask": "string",
  "syncRemark": "string",
  "template": "string",
  "upddateTime": "string",
  "vc": "string",
  "vc_password": "string",
  "vc_username": "string",
  "vmAppType": "string",
  "vmBusinessType": "string",
  "vmCostDetail": "string",
  "vmCpu": "string",
  "vmExtraDisk": "string",
  "vmHostName": "string",
  "vmLocation": "string",
  "vmMemory": "string",
  "vmNetArea": "string",
  "vmOs": "string",
  "vmSysIp": "string",
  "vmSysName": "string",
  "vmSysType": "string",
  "vmUses": "string",
  "vmVmPassword": "string",
  "vmVmUsername": "string",
  "vmip": "string"
}


{
	"creatorEmail": "46808456@qq.com",
	"creatorName": "caixiaofeng",
	"creatorNo": "admin",
	"creatorPhone": "18910321412",
	"reqVmStatus": "no_create",
	"vmAppType": "测试",
	"vmBusinessType": "核心业务",
	"vmCpu": "2C",
	"vmExtraDisk": "50G",
	"vmHostName": "Centos7.1",
	"vmLocation": "北京",
	"vmMemory": "2G",
	"vmNetArea": "for test_192.168.31",
	"vmOs": "Centos7.1",
	"vmSysName": "零售关系信息",
	"vmVmUsername": "root"
}