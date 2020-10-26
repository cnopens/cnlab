# Oracle实用操作记录.md
--  
   ## 获取表字段：  
      
    select *   
    from user_tab_columns   
    where Table_Name='用户表'   
    order by column_name  
--     
    ## 获取表注释：  
     
    select *   
    from user_tab_comments   
    where Table_Name='用户表'  
      
    order by Table_Name  
--      
    ## 获取字段注释：  
      
    select *   
    from user_col_comments   
    where Table_Name='用户表'  
      
    order by column_name  
    
  
    ## 获取表：
 --     
    select table_name from user_tables; //当前用户的表        
      
    select table_name from all_tables; //所有用户的表    
      
    select table_name from dba_tables; //包括系统表  
      
    select table_name from dba_tables where owner='zfxfzb'  
      
    /*   
    user_tables：  
      
    table_name,tablespace_name,last_analyzed等  
      
    dba_tables：  
      
    ower,table_name,tablespace_name,last_analyzed等  
      
    all_tables：  
      
    ower,table_name,tablespace_name,last_analyzed等  
      
    all_objects：  
      
    ower,object_name,subobject_name,object_id,created,last_ddl_time,timestamp,status等   
    */  
      
    /*  获取表字段：*/  
      
    select * from user_tab_columns where Table_Name='用户表';  
      
    select * from all_tab_columns where Table_Name='用户表';  
      
    select * from dba_tab_columns where Table_Name='用户表';  
      
    /* user_tab_columns：  
      
    table_name,column_name,data_type,data_length,data_precision,data_scale,nullable,column_id等  
      
    all_tab_columns ：  
      
    ower,table_name,column_name,data_type,data_length,data_precision,data_scale,nullable,column_id等  
      
    dba_tab_columns：  
      
    ower,table_name,column_name,data_type,data_length,data_precision,data_scale,nullable,column_id等   
    */  
      
    /*  获取表注释：*/  
      
    select * from user_tab_comments  
      
    /*   
    user_tab_comments：table_name,table_type,comments  
      
    相应的还有dba_tab_comments，all_tab_comments，这两个比user_tab_comments多了ower列。   
    */  
      
    /* 获取字段注释：*/  
      
    select * from user_col_comments  
      
    /*  
      
    user_col_comments：table_name,column_name,comments  
      
    相应的还有dba_col_comments，all_col_comments，这两个比user_col_comments多了ower列。   
    */  
    查询表名及表注释

    SELECT T.TABLE_NAME, P.COMMENTS
    FROM ALL_ALL_TABLES T, USER_TAB_COMMENTS P
    WHERE T.TABLE_NAME LIKE 'PMS_WSH%'
    AND T.TABLE_NAME = P.TABLE_NAME
    GROUP BY T.TABLE_NAME, P.COMMENTS


系统操作：

oracle用户解锁语句

解决过程：
使用有alter user数据库权限的用户登陆，角色选sysdba，执行以下命令：
解锁命令： SQL> ALTER USER 用户名 ACCOUNT UNLOCK;
锁定用户命令：SQL> ALTER USER 用户名 ACCOUNT LOCK;


注：登陆用户没有alter user数据库权限，使用拥有dba角色的用户登陆执行以下命令：
SQL> grant alter user to 用户名;
这样，对应的需要登录sqlplus的用户就可以去解锁其它用户了。直接使用具有dba角色就是的用户登陆解锁就OK了，因为dba角色拥有alter user权限。
 
查看数据库中所有角色和对应权限的语句：select * from role_sys_privs;
查看当前登陆用户拥有的角色的语句：select * from user_role_privs