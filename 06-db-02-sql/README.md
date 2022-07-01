# Домашнее задание к занятию "6.2. SQL"

## Введение

Перед выполнением задания вы можете ознакомиться с 
[дополнительными материалами](https://github.com/netology-code/virt-homeworks/tree/master/additional/README.md).

## Задача 1


```bash
root@server1:/opt/stack# cat docker-compose.yaml 
version: "3.9"
services:
  db:
    image: postgres:12
    restart: always
    container_name: postgres
    environment:
      POSTGRES_DB: postgres
      POSTGRES_USER: postgres
      PGDATA: /var/lib/postgresql/data/
    volumes:
    - db-data:/var/lib/postgresql/data
    - db-backup:/var/lib/backup
    network_mode: "host"

volumes:
  db-data:
  db-backup:
root@server1:/opt/stack# docker ps

CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                         PORTS     NAMES
c49faf6dbc1d   postgres:12   "docker-entrypoint.s…"   39 seconds ago   Restarting (1) 7 seconds ago             postgres
```

## Задача 2
- итоговый список БД после выполнения пунктов выше,
```bash
test_db=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 test_db   | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
(4 rows)
```
- описание таблиц (describe)
```bash
test_db=# \d+
                       List of relations
 Schema |  Name   | Type  |  Owner   |    Size    | Description 
--------+---------+-------+----------+------------+-------------
 public | clients | table | postgres | 8192 bytes | 
 public | orders  | table | postgres | 8192 bytes | 
(2 rows)
```
- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db
```bash
test_db=# SELECT * FROM information_schema.table_privileges WHERE grantee IN ('test-admin-user','test-simple-user'); 
 grantor  |     grantee      | table_catalog | table_schema | table_name | privilege_type | is_grantable | with_hierarchy 
----------+------------------+---------------+--------------+------------+----------------+--------------+----------------
 postgres | test-admin-user  | test_db       | public       | orders     | INSERT         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | orders     | SELECT         | NO           | YES
 postgres | test-admin-user  | test_db       | public       | orders     | UPDATE         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | orders     | DELETE         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | orders     | TRUNCATE       | NO           | NO
 postgres | test-admin-user  | test_db       | public       | orders     | REFERENCES     | NO           | NO
 postgres | test-admin-user  | test_db       | public       | orders     | TRIGGER        | NO           | NO
 postgres | test-simple-user | test_db       | public       | orders     | INSERT         | NO           | NO
 postgres | test-simple-user | test_db       | public       | orders     | SELECT         | NO           | YES
 postgres | test-simple-user | test_db       | public       | orders     | UPDATE         | NO           | NO
 postgres | test-simple-user | test_db       | public       | orders     | DELETE         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | clients    | INSERT         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | clients    | SELECT         | NO           | YES
 postgres | test-admin-user  | test_db       | public       | clients    | UPDATE         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | clients    | DELETE         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | clients    | TRUNCATE       | NO           | NO
 postgres | test-admin-user  | test_db       | public       | clients    | REFERENCES     | NO           | NO
 postgres | test-admin-user  | test_db       | public       | clients    | TRIGGER        | NO           | NO
 postgres | test-simple-user | test_db       | public       | clients    | INSERT         | NO           | NO
 postgres | test-simple-user | test_db       | public       | clients    | SELECT         | NO           | YES
 postgres | test-simple-user | test_db       | public       | clients    | UPDATE         | NO           | NO
 postgres | test-simple-user | test_db       | public       | clients    | DELETE         | NO           | NO
(22 rows)
```
- список пользователей с правами над таблицами test_db
```bash
test_db=# \du+
                                              List of roles
    Role name     |                         Attributes                         | Member of | Description 
------------------+------------------------------------------------------------+-----------+-------------
 postgres         | Superuser, Create role, Create DB, Replication, Bypass RLS | {}        | 
 test-admin-user  |                                                            | {}        | 
 test-simple-user |
```

## Задача 3

```bash
test_db=# insert into orders VALUES (1, 'Шоколад', 10), (2, 'Принтер', 3000), (3, 'Книга', 500), (4, 'Монитор', 7000), (5, 'Гитара', 4000);
INSERT 0 5
test_db=# insert into clients VALUES (1, 'Иванов Иван Иванович', 'USA'), (2, 'Петров Петр Петрович', 'Canada'), (3, 'Иоганн Себастьян Бах', 'Japan'), (4, 'Ронни Джеймс Дио', 'Russia'), (5, 'Ritchie Blackmore', 'Russia');
INSERT 0 5
test_db=# select count (*) from orders;
 count 
-------
     5
(1 row)

test_db=# select count (*) from clients;
 count 
-------
     5
(1 row)

```
## Задача 4

```bash
test_db=# update  clients set booking = 3 where id = 1;
UPDATE 1
test_db=# update  clients set booking = 4 where id = 2;
UPDATE 1
test_db=# update  clients set booking = 5 where id = 3;
UPDATE 1
test_db=# select * from clients where booking is not null;
 id |       lastname       | country | booking 
----+----------------------+---------+---------
  1 | Иванов Иван Иванович | USA     |       3
  2 | Петров Петр Петрович | Canada  |       4
  3 | Иоганн Себастьян Бах | Japan   |       5
(3 rows)
```

## Задача 5

```bash
test_db=# EXPLAIN SELECT * from clients WHERE booking is not NULL;
                        QUERY PLAN                         
-----------------------------------------------------------
 Seq Scan on clients  (cost=0.00..18.10 rows=806 width=72)
   Filter: (booking IS NOT NULL)
(2 rows)
```
```
Показывает время на выполнения запросов.
```

## Задача 6

```bash
root@server1:/# pg_dumpall -U postgres >/var/lib/backup/backup.dmp
root@server1:/opt/stack# docker-compose down
Removing postgres ... done
root@server1:/opt/stack# docker volume rm  stack_db-data
stack_db-data
root@server1:/opt/stack# docker-compose up
root@server1:/# psql -U postgres /var/lib/backup/backup.dmp
```
