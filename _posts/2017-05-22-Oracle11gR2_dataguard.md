---
layout: post
title: Oracle11gR2手工配置dataguard
date: 2017-05-22
tags: oracle 
description: "Oracle11gR2手工配置dataguard"
---

#### 网上有很多Oracle Dataguard的配置教程，但不难发现，很多采用的是rman duplicate这种方法，尽管此种方法较为简便。但在某种程度上，却也误导了初学者，虽说也能配置成功，但只知其然不知其所以然，Dataguard的本质没有吃透，也不利于其维护和调优。
#### 本配置基于Oracle官方文档，目的在于加深对于Dataguard的了解。　
#### 本配置的结果是最大性能模式下的异步传输 ，因此在参数文件中，只涉及基本的主备参数，没有考虑switchover和最大性能模式下的real time apply。在监听的配置中，也没有考虑Data Guard Broker的应用情况

## 配置环境：
```
主库： 备库：
操作系统版本： Oracle Linux 6.3 Oracle Linux 6.3
数据库版本： Oracle 11.2.0.1.0 Oracle 11.2.0.1.0
主机名： node1.being.com node2.being.com
IP： 192.168.1.11 192.168.1.12
db_name orcl victor
db_unique_name orcl orcl
instance_name orcl victor
service_names orcl victor
```
## 注意：
1. Dataguard中只需要db_unique_name保持一致即可
2. 主库中除了安装Oracle软件以外，还需要dbca建库。而备库中，只需要安装Oracle软件即可，即在./runInstaller安装过程中，第三步选择install software only即可
3. 主备库的ORACLE_BASE=/u01/app/oracle,ORACLE_HOME=$ORACLE_BASE/product/11.2.0.1/db_1

### 一、配置监听
#### 1> 主库上
```
[oracle@node1 ~]$ cd $ORACLE_HOME/network/admin/
[oracle@node1 admin]$ cat tnsnames.ora
TO_VICTOR =
(DESCRIPTION =
(ADDRESS_LIST =
(ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.12)(PORT = 1521))
)
(CONNECT_DATA =
(SERVER = DEDICATED)
(SERVICE_NAME = victor)
)
)
```
其中to_victor为网络服务名，在后面配置log_archive_dest_2和fal_server中会用到
#### 2> 备库上
```
[oracle@node2 ~]$ cd $ORACLE_HOME/network/admin/
[oracle@node2 admin]$ cat tnsnames.ora
TO_ORCL =
(DESCRIPTION =
(ADDRESS_LIST =
(ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.11)(PORT = 1521))
)
(CONNECT_DATA =
(SERVER = DEDICATED)
(SERVICE_NAME = orcl)
)
)
```
注意：该配置只是基于基本的Dataguard配置，没有考虑Dataguard broker的配置
### 二、主库环境准备 -->> 在node1上操作
#### 1> 将数据库设置为归档模式
```
SQL> archive log list -->>若Database log mode为No Archive Mode，则表示该数据库运行在非归档模式下。进行以下操作
SQL> shutdown immediate
SQL> startup mount
SQL> alter database archivelog;
SQL> alter database open;
```
#### 2> 将数据库设置为Force Logging模式
```
SQL> select force_logging from v$database; -->>若为NO，则进行以下操作
SQL> alter database force logging;
```
#### 3> 修改主库参数文件
```
SQL> alter system set log_archive_config='dg_config=(orcl,victor)';
-->> 代表该Dataguard是两个节点，一主一从，若要配置多个节点，则需要在此处添加。
SQL> alter system set log_archive_dest_1='location=USE_DB_RECOVERY_FILE_DEST valid_for=(online_logfiles,primary_role) db_unique_name=orcl';
-->> location代表本地归档。在这里我们使用闪回区作为在线日志文件的归档目录，在实际生产环境中，如果归档日志是归档在本地文件系统上，不建议使用闪回区，因为闪回区和数据库软件是在同一个目录下，如果归档日志过多，闪回区空间增长过快，容易造成磁盘空间不足，这样容易使数据库挂掉。valid_for代表该归档目录只有在该库为主库，归档在线日志文件时才有效。
SQL> alter system set log_archive_dest_2='service=to_victor async valid_for=(online_logfiles,primary_role) db_unique_name=victor';
-->> service后面接的是网络服务名
SQL> alter system set log_archive_dest_state_1='enable';
SQL> alter system set log_archive_dest_state_2='enable';
SQL> alter system set remote_login_passwordfile=exclusive scope=spfile; -->> 设置密码文件的权限，该设置需重启数据库才能生效
SQL> alter system set log_archive_format='%t_%s_%r.arc' scope=spfile; -->> 设置归档日志的格式，该设置需重启数据库才能生效
-->> 最后两项可不用显性设定
```
### 三、 为备库创建各种文件 -->> 在node1上操作
#### 1> 密码文件
```
[oracle@node1 ~]$ cd $ORACLE_HOME/dbs/
[oracle@node1 dbs]$ orapwd file=orapworcl entries=5 force=y password=oracle
```
#### 2> 参数文件
```
SQL> create pfile='/home/oracle/orcl.ora' from spfile;
```
#### 3> 备份数据库
对数据库做备份有多种办法，包括冷备、在线热备、RMAN备份，在这里我们使用RMAN备份
```
[oracle@node1 ~]$ mkdir /home/oracle/rman
[oracle@node1 ~]$ rman target /
RMAN> backup database format '/home/oracle/rman/full_%U';
```
#### 4> 备份控制文件
```
SQL> alter system switch logfile;
SQL> alter database create standby controlfile as '/home/oracle/victor.ctl';
```
### 四、将上述四类文件copy到备库
```
[oracle@node1 ~]$ scp $ORACLE_HOME/dbs/orapworcl oracle@192.168.1.12:/u01/app/oracle/product/11.2.0.1/db_1/dbs/orapwvictor
[oracle@node1 ~]$ scp /home/oracle/orcl.ora oracle@192.168.1.12:/home/oracle/victor.ora
[oracle@node1 ~]$ scp -r /home/oracle/rman/ oracle@192.168.1.12:/home/oracle/rman
[oracle@node1 ~]$ scp /home/oracle/victor.ctl oracle@192.168.1.12:/home/oracle
```
### 五、 创建备库
#### 1> 修改参数文件
```
[oracle@node2 ~]$ vim victor.ora
*.audit_file_dest='/u01/app/oracle/admin/victor/adump'
*.audit_trail='db'
*.compatible='11.2.0.0.0'
*.control_files='/u01/app/oracle/oradata/victor/control01.ctl'
*.db_block_size=8192
*.db_domain=''
*.db_name='orcl'
db_unique_name=victor
*.diagnostic_dest='/u01/app/oracle'
*.dispatchers='(protocol=TCP)'
*.log_archive_config='dg_config=(orcl,victor)'
*.log_archive_dest_1='location=/u01/archivelog valid_for=(standby_logfiles,standby_role) db_unique_name=victor'
*.log_archive_dest_state_1='enable'
*.memory_target=471859200
*.open_cursors=300
*.processes=150
*.remote_login_passwordfile='EXCLUSIVE'
*.shared_servers=1
*.undo_tablespace='UNDOTBS1'
fal_server=to_orcl
db_file_name_convert='/u01/app/oracle/oradata/orcl/','/u01/app/oracle/oradata/victor/'
log_file_name_convert='/u01/app/oracle/oradata/orcl/','/u01/app/oracle/oradata/victor/’
```
#### 2> 创建参数文件中相应的目录
```
[oracle@node2 ~]$ mkdir -p /u01/app/oracle/admin/victor/adump
[oracle@node2 ~]$ mkdir /u01/archilog
[oracle@node2 ~]$ mkdir -p /u01/app/oracle/oradata/victor
[oracle@node2 ~]$ cp /home/oracle/victor.ctl /u01/app/oracle/oradata/victor/control01.ctl
```
#### 3> 创建备库
```
[oracle@node2 ~]$ sqlplus / as sysdba
SQL> create spfile from pfile='/home/oracle/victor.ora';
SQL> startup mount
[oracle@node2 ~]$ rman target /
RMAN> restore database;
RMAN> alter database open;
SQL> alter database recover managed standby database disconnect from session;
```
### 六、测试
#### 1> 在备库上查询归档日志的序列号
```
SQL> select sequence# from v$archived_log;
```
#### 2> 在主库上切换一次日志
```
SQL> alter system switch logfile;
```
#### 3> 在备库上查询归档日志的序列号,看是否有增加
```
SQL> select sequence# from v$archived_log;
```
#### 4> 在备库上查询归档日志是否被应用
```
SQL> select sequence#,applied from v$archived_log;
```
当然，也可以用具体案例进行测试，譬如在主库中新建一张表，对表进行增、删、改，然后切换日志，看备库能否应用。