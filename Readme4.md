## Домашнее задание к занятию "6.2. SQL"
Введение
Перед выполнением задания вы можете ознакомиться с дополнительными материалами.

1. Задача 1  
Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.  

Приведите получившуюся команду или docker-compose манифест.  
c docker пока не получается, но пытаюсь
```bash
gugl@guglVirt:~$ docker pull postgres:12

gugl@guglVirt:~$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
postgres      12        963b1b753ca6   3 days ago     373MB
hello-world   latest    feb5d9fea6a5   5 months ago   13.3kB

gugl@guglVirt:~$ docker volume create vol2
vol2
gugl@guglVirt:~$ docker volume create vol1
vol1

gugl@guglVirt:~$ docker run --name postgres-1 -e POSTGRES_PASSWORD=postgres -d -p 5432:5432 postgres:12
54a23ebfe3467907c49aa5723e8b12d8f03a1046914b77dab2ccfc10e2bd04e3

gugl@guglVirt:~$ docker exec -it postgres-1 bash

root@073b9d67e2db:/# psql -U postgres
psql (12.10 (Debian 12.10-1.pgdg110+1))
Type "help" for help.

postgres=#
``


2. Задача 2  
В БД из задачи 1:  

создайте пользователя test-admin-user и БД test_db  
в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже)  
предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db  
создайте пользователя test-simple-user  
предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE данных таблиц БД test_db  
Таблица orders:  

id (serial primary key)
наименование (string)
цена (integer)
Таблица clients:

id (serial primary key)
фамилия (string)
страна проживания (string, index)
заказ (foreign key orders)
```bash
postgres=# CREATE DATABASE test_db;
CREATE DATABASE

postgres-# \q
postgres@trusty:~$ psql -d test_db
psql (9.3.24)
Type "help" for help.

test_db=# CREATE TABLE orders (id INT PRIMARY KEY, name text, price INT);
CREATE TABLE
test_db=# \dt
         List of relations
 Schema |  Name  | Type  |  Owner
--------+--------+-------+----------
 public | orders | table | postgres
(1 row)

test_db=# CREATE TABLE clients (id INT PRIMARY KEY, fam char(30), land char(30), zakaz INT, FOREIGN KEY (zakaz) REFERENCES orders (id));
CREATE TABLE

CREATE ROLE "test-admin-user" SUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN;
test_db=# \du
                                List of roles
    Role name    |                   Attributes                   | Member of
-----------------+------------------------------------------------+-----------
 postgres        | Superuser, Create role, Create DB, Replication | {}
 test-admin-user | Superuser, No inheritance                      | {}

test_db=# CREATE ROLE "test-simple-user" NOSUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN;
CREATE ROLE
test_db=# \du
                                 List of roles
    Role name     |                   Attributes                   | Member of
------------------+------------------------------------------------+-----------
 postgres         | Superuser, Create role, Create DB, Replication | {}
 test-admin-user  | Superuser, No inheritance                      | {}
 test-simple-user | No inheritance                                 | {}

test_db=#
GRANT SELECT ON TABLE public.clients TO "test-simple-user";
GRANT INSERT ON TABLE public.clients TO "test-simple-user";
GRANT UPDATE ON TABLE public.clients TO "test-simple-user";
GRANT DELETE ON TABLE public.clients TO "test-simple-user";
GRANT SELECT ON TABLE public.orders TO "test-simple-user";
GRANT INSERT ON TABLE public.orders TO "test-simple-user";
GRANT UPDATE ON TABLE public.orders TO "test-simple-user";
GRANT DELETE ON TABLE public.orders TO "test-simple-user";
test_db=# \z
                                   Access privileges
 Schema |  Name   | Type  |        Access privileges         | Column access privileges
--------+---------+-------+----------------------------------+--------------------------
 public | clients | table | postgres=arwdDxt/postgres       +|
        |         |       | "test-simple-user"=arwd/postgres |
 public | orders  | table | postgres=arwdDxt/postgres       +|
        |         |       | "test-simple-user"=arwd/postgres |
(2 rows)

```

Приведите:

итоговый список БД после выполнения пунктов выше,  
описание таблиц (describe)  
SQL-запрос для выдачи списка пользователей с правами над таблицами test_db  
список пользователей с правами над таблицами test_db  

Задача 3
Используя SQL синтаксис - наполните таблицы следующими тестовыми данными:  

Таблица orders

Наименование	цена
Шоколад	10
Принтер	3000
Книга	500
Монитор	7000
Гитара	4000

```sql
test_db=# insert into orders VALUES (1, 'Шоколад', 10), (2, 'Принтер', 3000), (3, 'Книга', 500), (4, 'Монитор', 7000), (5, 'Гитара', 4000);
INSERT 0 5
      
test_db=# select * from orders;
 id |  name   | price
----+---------+-------
  1 | Шоколад |    10
  2 | Принтер |  3000
  3 | Книга   |   500
  4 | Монитор |  7000
  5 | Гитара  |  4000
(5 rows)
```
Таблица clients

ФИО	Страна проживания
Иванов Иван Иванович	USA
Петров Петр Петрович	Canada
Иоганн Себастьян Бах	Japan
Ронни Джеймс Дио	Russia
Ritchie Blackmore	Russia
```sql
test_db=# insert into clients VALUES (1, 'Иванов Иван Иванович', 'USA'), (2, 'Петров Петр Петрович', 'Canada'), (3, 'Иоганн Себастьян Бах', 'Japan'), (4, 'Ронни Джеймс Дио', 'Russia'), (5, 'Ritchie Blackmore', 'Russia');
INSERT 0 5
test_db=# select * from clients;
 id |              fam               |              land              | zakaz
----+--------------------------------+--------------------------------+-------
  1 | Иванов Иван Иванович           | USA                            |
  2 | Петров Петр Петрович           | Canada                         |
  3 | Иоганн Себастьян Бах           | Japan                          |
  4 | Ронни Джеймс Дио               | Russia                         |
  5 | Ritchie Blackmore              | Russia                         |
(5 rows)
```

Используя SQL синтаксис:  
вычислите количество записей для каждой таблицы    
приведите в ответе:  
запросы  
результаты их выполнения.  
```sql
test_db=# select count(*) from orders;
 count
-------
     5
(1 row)

test_db=# select count(*) from clients;
 count
-------
     5
(1 row)
```

Задача 4
Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys свяжите записи из таблиц, согласно таблице:

ФИО	Заказ
Иванов Иван Иванович	Книга
Петров Петр Петрович	Монитор
Иоганн Себастьян Бах	Гитара
Приведите SQL-запросы для выполнения данных операций.  
```bash
test_db=# update  clients set zakaz = 3 where id = 1;
UPDATE 1
test_db=# update  clients set zakaz = 4 where id = 2;
UPDATE 1
test_db=# update  clients set zakaz = 5 where id = 3;
UPDATE 1
```
Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.  
```bash
test_db=# select c.fam, o.name from clients c, orders o where c.zakaz = o.id;
              fam               |  name
--------------------------------+---------
 Иванов Иван Иванович           | Книга
 Петров Петр Петрович           | Монитор
 Иоганн Себастьян Бах           | Гитара
(3 rows)
```
Подсказка - используйте директиву UPDATE.  

Задача 5
Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 (используя директиву EXPLAIN).  
```bash
test_db=# explain select c.fam, o.name from clients c, orders o where c.zakaz = o.id;
                               QUERY PLAN
-------------------------------------------------------------------------
 Hash Join  (cost=36.10..52.75 rows=280 width=156)
   Hash Cond: (c.zakaz = o.id)
   ->  Seq Scan on clients c  (cost=0.00..12.80 rows=280 width=128)
   ->  Hash  (cost=21.60..21.60 rows=1160 width=36)
         ->  Seq Scan on orders o  (cost=0.00..21.60 rows=1160 width=36)
(5 rows)
```

Приведите получившийся результат и объясните что значат полученные значения.    

Seq Scan — последовательное, блок за блоком, чтение данных таблицы clients  
затратность операции  36.10  на получение первой строки. 52.75 — затраты на получение всех строк.  
rows всех строк обработано 280 Видимо наверное это строки блоков от Seq Scan  
width размер строки в байтах - 156 байт  
Из-за условия по двум таблицам, тут два описания затрат операций и общие затраты.  

Задача 6
Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).

Остановите контейнер с PostgreSQL (но не удаляйте volumes).

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления.
```bash
postgres@02a245eea370:~$ mkdir backup
postgres@02a245eea370:~$ cd backup
postgres@02a245eea370:~/backup$ pg_dump test_db > dump1203.dump
postgres@02a245eea370:~/backup$ ls -la
total 12
drwxr-xr-x 2 postgres postgres 4096 Mar 12 15:11 .
drwxr-xr-x 4 postgres postgres 4096 Mar 12 15:10 ..
-rw-r--r-- 1 postgres postgres 2718 Mar 12 15:11 dump1203.sql

gugl@guglVirt:~$ docker stop postgres-2
postgres-2
gugl@guglVirt:~$ docker run --name postgres-4 -e POSTGRES_PASSWORD=postgres -d -p 5432:5432 -v vol1:/var/lib/postgresql/data1 -v vol2:/var/lib/postgresql postgres:12
b66bbf531046cbadce710e86d250601eec1543c9460f6a4d6d0a9c118c477c57

gugl@guglVirt:~$ docker exec -it postgres-4 bash
root@b66bbf531046:/# su - postgres
postgres@b66bbf531046:~$ \db
-bash: db: command not found

postgres@b66bbf531046:~/backup$ mv dump1203.dump dump1203.sql
postgres@b66bbf531046:~/backup$ psql -U postgres -d test_db -f dump1203.sql
SET
SET
SET
SET
SET
 set_config 
------------
 
(1 row)

SET
SET
SET
SET
SET
SET
CREATE TABLE
ALTER TABLE
CREATE TABLE
ALTER TABLE
COPY 5
COPY 5
ALTER TABLE
ALTER TABLE
ALTER TABLE
psql -d test_db

psql (12.10 (Debian 12.10-1.pgdg110+1))
Type "help" for help.

test_db=# 
``
