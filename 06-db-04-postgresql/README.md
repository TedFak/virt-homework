# Домашнее задание к занятию "6.4. PostgreSQL"

## Задача 1

**Найдите и приведите** управляющие команды для:
- вывода списка БД
- подключения к БД
- вывода списка таблиц
- вывода описания содержимого таблиц
- выхода из psql
```bash
\l[+]   [PATTERN]      list databases
\c[onnect] {[DBNAME|- USER|- HOST|- PORT|-] | conninfo}
                         connect to new database (currently "postgres")
\dt[S+] [PATTERN]      list tables
\d[S+]  NAME           describe table, view, sequence, or index
\q                     quit psql
```
## Задача 2

**Приведите в ответе** команду, которую вы использовали для вычисления и полученный результат.
```bash
test_database=# select attname, avg_width from pg_stats where tablename='orders';
 attname | avg_width 
---------+-----------
 id      |         4
 title   |        16
 price   |         4
(3 rows)
```
## Задача 3

Предложите SQL-транзакцию для проведения данной операции.
```bash
test_database=# CREATE TABLE orders_1 (LIKE orders);
CREATE TABLE
test_database=# CREATE TABLE orders_2 (LIKE orders);
CREATE TABLE
test_database=# INSERT INTO orders_1 SELECT * FROM orders WHERE price > 499;
INSERT 0 3
test_database=# DELETE FROM orders WHERE price > 499;
DELETE 3
test_database=# 
test_database=# INSERT INTO orders_2 SELECT * FROM orders WHERE price <= 499;
INSERT 0 5
test_database=# DELETE FROM orders WHERE price <= 499;
DELETE 5
```
Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?
```
Да, при проектирование таблицы надо было сделать сразу секционированние.
```
## Задача 4

Используя утилиту `pg_dump` создайте бекап БД `test_database`.
```bash
postgres@15f403725740:~$ pg_dump -d test_database >/var/lib/postgresql/test1_dump.sql
```

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца `title` для таблиц `test_database`?
```python
CREATE TABLE public.orders (
    id  integer NOT NULL,
    title integer UNIQUE,
    price numeric NOT NULL CHECK (price > 0),
);
```