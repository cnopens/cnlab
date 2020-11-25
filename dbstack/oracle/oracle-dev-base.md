#oracle-dev-base.md


## 基础知识 

***语法

<!-- if else  -->


1. IF 语法

IF 表达式 THEN
    ...
END IF;
例如：

复制代码
set serverout on
declare
   v_name varchar2(20):='&name';
begin
   if v_name='kiki' then
      dbms_output.put_line('登录成功');
   end if;
end;
/
--------执行内容结果如下-------
输入 name 的值:  kiki
原值    2:    v_name varchar2(20):='&name';
新值    2:    v_name varchar2(20):='kiki';
登录成功
复制代码
2. IF .. ELSE 语法：

IF  条件表达式  THEN
　　...
ELSE
　　...
END IF;
例如：

复制代码
set serverout on
declare
   v_name student.sname%type:='&name';
begin
  if v_name='kiki' then 
     dbms_output.put_line('登录成功!');
  else
     dbms_output.put_line('登录失败');
  end if;
end;
/

--------执行内容结果如下-------
输入 name 的值:  kiki
原值    2:    v_name student.sname%type:='&name';
新值    2:    v_name student.sname%type:='kiki';
登录失败

3. IF ... ELSIF ... ELSE 嵌套结构


IF 条件表达式  THEN
    ...
ELSIF 条件表达式 THEN
    ...
ELSE
    ...
END  IF ;
复制代码
例如：

复制代码
--1.if-else结构
set serverput on  --打开oracle自带的输出方法dbms_output
declare --声明
   v_name varchar2(20):='&name'; --定义需要手动输入的变量
   v_password number(10):='&password';
begin --开始
   if v_name='kikiwen' and v_password=123 then --条件判断
      dbms_output.put_line('登录成功');--输出语句
   elsif v_name='kiki' and v_password=123 then
      dbms_output.put_line('登录' || v_name || '账号成功');
   else
      dbms_output.put_line('登录失败！'|| v_name || '账号或者密码不正确');
   end if;
end;--结束
/
--------执行内容结果如下-------
输入 name 的值:  kiki
原值    2:    v_name varchar2(20):='&name';
新值    2:    v_name varchar2(20):='kiki';
输入 password 的值:  123
原值    2:    v_name varchar2(20):='&password';
新值    2:    v_name varchar2(20):=123;
登录kiki账号成功
复制代码