---
title: 转：Oracle ID自增长的设置
tags:
    - Oracle  
---
1.创建表
```sql
create table tablename(
  id number(20) NOT NULL primary key,/*主键，自动增加*/
  name varchar2(20)， 
  password varchar2(20)
)
```
2.创建自动增长序列
```sql
Create Sequence addAuto_Sequence
Increment by 1     -- 每次加几个 
start with 1       -- 从1开始计数     
nomaxvalue         -- 不设置最大值,设置最大值：maxvalue 9999  
nocycle            -- 一直累加，不循环    
cache 10;  
```
3.创建触发器
```sql
 Create trigger addAuto before 
 insert on tablename for each row /*对每一行都检测是否触发*/
 begin
 select addAuto_Sequence.nextval into:New.id from dual;
 end; 
``` 
4、提交 commit;
5、测试 insert into tablename values('rena','123456');