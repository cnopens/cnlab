# oracle basic operations 
---
####
添加字段的语法：alter table tablename add (字段名 number(2) not null);
alter table tablename add (字段名 number(2),字段名 varchar2(13));
修改字段的语法：alter table tablename modify (column datatype [default value][null/not null],….);
删除字段的语法：alter table tablename drop (column);
修改字段的名：alter table 表名 rename column 原子段 to 新字段;
添加时如果有数据不能设置not null.
删除不可再sys下使用。