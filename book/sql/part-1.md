### oracle ###
* 远程登录
```sql
sqlplus username/password@//host:port/sid
```
* 赋予scott用户dba权限
```sql
sqlplus /nolog
conn /as sysdba;
select username from dba_users;  --查找用户名，需要dba权限
grant dba to scott;   --赋予scott用户dba权限
revoke dba from scott;  --解除scott用户dba权限
```
* command命令
```sql
desc tablename;  --描述一张表
```
* 解锁用户scott
```sql
sqlplus /nolog
conn /as sysdba;
alter user username account unlock;
alter user username identified by password;   --将用户密码改成password
```
* select语句
```sql
SELECT * FROM USER_TABLES; 
--查看用户有哪些表 
select 2*3 from dual; --dual表
select sysdate from dual; --sysdate
select ename,sal*12 annual_sal from emp; --别名
select ename,sal*12 "annual sal" from emp;
--别名保持空格和小写,此时用双引号",而不使用单引号'
select sal*12+comm from emp;
--含有null表达式的字符运算为空值，即comm为null会使整个结果为null
select sal||ename from emp; 
--||连接符,将两个字段拼接在一起,可以拼接各种字段包括日期,值为空值时拼接后为不同于运算变成null
select sal||'hjhds' from emp; 
--拼接字符串，使用单引号',而非双引号",若字符中包含',则用两个单引号代替，如sal||'hj''hds'
select distinct comm from emp;
--去掉重复的列，null也是一列
select distinct sal,comm from emp;
--去sal和comm组合后的重
select ename from emp where ename like '_A%';
--_代表一个字母,%代表0个或多个字母，如果查询%可用转义字符\% 
--还可以用escape，如select ename from emp where ename like '%$%%' escape '$';
```
* function
```sql
select lower(rowname) from tablename;
--lower转成小写
select rowname from emp where lower(rowname) like '_a%';
--筛选第二个字母为A或者a的列
select rowname from emp where rowname like '_a%' or rowname like '_A%';
--筛选第二个字母为A或者a的列
select upper('aaa') from dual;
--upper转成大写
select substr(ename,2,3) from emp;
--从第二字符截,一共截三个字符
select chr(65) from dual;
--将ascii码转成字母mysql为char()
select ascii('A') from dual;
--将字母转成ascii码
select round(12.343) from dual;
--round四舍五入成整数
select round(12.343,2) from dual;
--保留小数点后两位
select to_char(sal,'$999,999.999') from emp;
--数字转成带美元符号格式的字符串
select to_char(sal,'L999,999.999') from emp;
--L本地化格式select to_char(sal,'L000,000.000') from emp;0可以补位对齐，9不补位
```
* oracle日期
```sql
select to_char(sysdate ,'YYYY-MM-DD HH:MM:SS') from emp;
--格式化日期,'HH24:MM:SS'为24小时制，'HH:MM:SS'为12小时制
```

### mysql ###
* 查看状态命令
```sql
systemctl status mysqld.service 
```
* 忘记密码时重置root密码
```
vi /etc/my.cnf
```
```
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html

[mysqld]
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M
#
# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
```
[mysqld]下面添加skip-grant-tables
```
service mysqld restart 重启mysql服务
mysql -u root
mysql>update mysql.user set authentication_string=password('newpasssword') where user='root';
mysql>flush privileges;
mysql>quit
```
将/etc/my.cnf还原
```
service mysqld restart 重启mysql服务
```
* 允许外部IP访问
```
mysql -u root -p
mysql>grant all privileges on *.* to 'root'@'%' identified by 'password' with grant option;
mysql>flush privileges;  --刷新权限
```
* MySQL 5.7.4版开始，设置用户密码过期
```sql
ALTER USER 'root'@'localhost' PASSWORD EXPIRE INTERVAL 250 DAY;
ALTER USER 'root'@'localhost' PASSWORD EXPIRE NEVER;

```
