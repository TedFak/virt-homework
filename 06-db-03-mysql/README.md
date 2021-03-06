# Домашнее задание к занятию "6.3. MySQL"

## Задача 1

Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.
```bash
vagrant@server1:~$ docker run --rm --name mysql -e MYSQL_ROOT_PASSWORD=test1 -ti -v volmysql:/etc/mysql/ mysql:8
```
Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-03-mysql/test_data) и 
восстановитесь из него.
```bash
root@server1:~# docker cp test_dump.sql mysql:/etc/mysql
root@server1:~# docker exec -it mysql bash

```
Перейдите в управляющую консоль `mysql` внутри контейнера.
```bash
root@server1:~# docker exec -it mysql bash
bash-4.4# mysql -u root -p mysql
Enter password: 
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 14
Server version: 8.0.29 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```
Используя команду `\h` получите список управляющих команд.
```bash
mysql> \h

For information about MySQL products and services, visit:
   http://www.mysql.com/
...
For server side help, type 'help contents'
```
Найдите команду для выдачи статуса БД и **приведите в ответе** из ее вывода версию сервера БД.
```bash
mysql> \s
--------------
mysql  Ver 8.0.29 for Linux on x86_64 (MySQL Community Server - GPL)

Connection id:		28
Current database:	test_db
Current user:		root@localhost
SSL:			Not in use
Current pager:		stdout
Using outfile:		''
Using delimiter:	;
Server version:		8.0.29 MySQL Community Server - GPL
Protocol version:	10
Connection:		Localhost via UNIX socket
Server characterset:	utf8mb4
Db     characterset:	utf8mb4
Client characterset:	latin1
Conn.  characterset:	latin1
UNIX socket:		/var/run/mysqld/mysqld.sock
Binary data as:		Hexadecimal
Uptime:			37 min 29 sec

Threads: 3  Questions: 454  Slow queries: 0  Opens: 555  Flush tables: 3  Open tables: 473  Queries per second avg: 0.201
--------------
```
Подключитесь к восстановленной БД и получите список таблиц из этой БД.
```bash
mysql> show tables;
+-------------------+
| Tables_in_test_db |
+-------------------+
| orders            |
+-------------------+
1 row in set (0.00 sec)
```
**Приведите в ответе** количество записей с `price` > 300.
```bash
mysql> select count(*) from orders where price>300;
+----------+
| count(*) |
+----------+
|        1 |
+----------+
1 row in set (0.00 sec)

```

## Задача 2
 Предоставьте привелегии пользователю test на операции SELECT базы test_db.
 ```bash
 mysql> grant select on test_db.* to 'test'@'localhost';
Query OK, 0 rows affected, 1 warning (0.02 sec)
 ```
Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю `test` и 
**приведите в ответе к задаче**.
```bash
mysql> select * from INFORMATION_SCHEMA.USER_ATTRIBUTES where user = 'test';
+------+-----------+---------------------------------------+
| USER | HOST      | ATTRIBUTE                             |
+------+-----------+---------------------------------------+
| test | localhost | {"fname": "James", "lname": "Pretty"} |
+------+-----------+---------------------------------------+
1 row in set (0.02 sec)
```
## Задача 3

Исследуйте, какой `engine` используется в таблице БД `test_db` и **приведите в ответе**.
```bash
mysql> SELECT TABLE_SCHEMA, TABLE_NAME, ENGINE FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = 'test_db';
+--------------+------------+--------+
| TABLE_SCHEMA | TABLE_NAME | ENGINE |
+--------------+------------+--------+
| test_db      | orders     | InnoDB |
+--------------+------------+--------+
1 row in set (0.00 sec)
```
Измените `engine` и **приведите время выполнения и запрос на изменения из профайлера в ответе**:
- на `MyISAM`
- на `InnoDB`
```bash
mysql> SHOW PROFILES;
+----------+------------+-------------------------------------------------------------------------------------------------------+
| Query_ID | Duration   | Query                                                                                                 |
+----------+------------+-------------------------------------------------------------------------------------------------------+
|        1 | 0.00098900 | SELECT TABLE_SCHEMA, TABLE_NAME, ENGINE FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = 'test_db' |
|        2 | 0.09525575 | ALTER TABLE orders ENGINE = MyISAM                                                                    |
|        3 | 0.04650625 | ALTER TABLE orders ENGINE = InnoDB                                                                    |
+----------+------------+-------------------------------------------------------------------------------------------------------+
3 rows in set, 1 warning (0.00 sec)
```
## Задача 4 

Приведите в ответе измененный файл `my.cnf`.
```bash
[mysqld]
datadir=/var/lib/mysql
socket=/var/run/mysqld/mysqld.sock
secure-file-priv=NULL
user=mysql

pid-file=/var/run/mysqld/mysqld.pid

innodb_flush_log_at_trx_commit = 0 
innodb_file_format=Barracuda
innodb_log_buffer_size = 1M
innodb_buffer_pool_size = 3068M
innodb_log_file_size = 100M
[client]
socket=/var/run/mysqld/mysqld.sock

!includedir /etc/mysql/conf.d/
```

