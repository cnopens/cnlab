# oracle basic operations 
---
## Often use ->
添加字段的语法：alter table tablename add (字段名 number(2) not null);
alter table tablename add (字段名 number(2),字段名 varchar2(13));
修改字段的语法：alter table tablename modify (column datatype [default value][null/not null],….);
删除字段的语法：alter table tablename drop (column);
修改字段的名：alter table 表名 rename column 原子段 to 新字段;
添加时如果有数据不能设置not null.
删除不可再sys下使用。

## oracle special command

Oracle中如何查询一个表的所有字段名和数据类型

查询语法

select A.COLUMN_NAME,A.DATA_TYPE  from user_tab_columns A
where TABLE_NAME='表名'

查询例子

select A.COLUMN_NAME,A.DATA_TYPE  from user_tab_columns A
where TABLE_NAME='PUB_GOODS'

添加排序后例子

select A.COLUMN_NAME,A.DATA_TYPE  from user_tab_columns A
where TABLE_NAME='PUB_GOODS'
order by A.COLUMN_ID asc



select *
from user_col_comments
where Table_Name='DSI_VM_INFO'
order by column_name;



select
    ut.COLUMN_NAME,--字段名称
    uc.comments,--字段注释
    ut.DATA_TYPE,--字典类型
    ut.DATA_LENGTH,--字典长度
    ut.NULLABLE--是否为空
from user_tab_columns  ut
         inner JOIN user_col_comments uc
                    on ut.TABLE_NAME  = uc.table_name and ut.COLUMN_NAME = uc.column_name
where ut.Table_Name='DSI_VM_INFO'
order by ut.column_name