## Домашнее задание к занятию "6.2. SQL"
Введение
Перед выполнением задания вы можете ознакомиться с дополнительными материалами.

1. Задача 1  
Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.  

Приведите получившуюся команду или docker-compose манифест.  

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

test_db=# CREATE TABLE clients (id INT PRIMARY KEY, fam char(30), land char(30), zakaz INT, FOREIGN KEY (zakaz) REFERENCES orders (id));
CREATE TABLE
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
Таблица clients

ФИО	Страна проживания
Иванов Иван Иванович	USA
Петров Петр Петрович	Canada
Иоганн Себастьян Бах	Japan
Ронни Джеймс Дио	Russia
Ritchie Blackmore	Russia
Используя SQL синтаксис:

вычислите количество записей для каждой таблицы
приведите в ответе:
запросы
результаты их выполнения.
Задача 4
Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys свяжите записи из таблиц, согласно таблице:

ФИО	Заказ
Иванов Иван Иванович	Книга
Петров Петр Петрович	Монитор
Иоганн Себастьян Бах	Гитара
Приведите SQL-запросы для выполнения данных операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.

Подсказк - используйте директиву UPDATE.

Задача 5
Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 (используя директиву EXPLAIN).

Приведите получившийся результат и объясните что значат полученные значения.

Задача 6
Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).

Остановите контейнер с PostgreSQL (но не удаляйте volumes).

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления.

