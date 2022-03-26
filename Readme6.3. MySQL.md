### Домашнее задание к занятию "6.3. MySQL"  

Введение  
Перед выполнением задания вы можете ознакомиться с дополнительными материалами.  

## Задача 1  
Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.  
  ```bash
  root@trusty:~# docker pull mysql
  root@trusty:~# docker run -v /mydat:/datin --name mysql1 -d -e MYSQL_ROOT_PASSWORD=mysql mysql
  root@trusty:~# docker exec -it mysql1 bash
  ```

Изучите бэкап БД и восстановитесь из него. 
   ```bash
  root@750287e19fe4:/datin# mysql -uroot -pmysql test_db < test_dump.sql
  ```
Перейдите в управляющую консоль mysql внутри контейнера.  
  ```bash
 root@750287e19fe4:/datin# mysql -hlocalhost -uroot -pmysql
  ...
  ```
Используя команду \h получите список управляющих команд.  
```bash
mysql> \h

For information about MySQL products and services, visit:
   http://www.mysql.com/
For developer information, including the MySQL Reference Manual, visit:
   http://dev.mysql.com/
To buy MySQL Enterprise support, training, or other products, visit:
   https://shop.mysql.com/

List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Don't write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
system    (\!) Execute a system shell command.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.
resetconnection(\x) Clean session context.
query_attributes Sets string parameters (name1 value1 name2 value2 ...) for the next query to pick up.

For server side help, type 'help contents'
```
Найдите команду для выдачи статуса БД и приведите в ответе из ее вывода версию сервера БД.  
```bash
mysql> \s
--------------
mysql  Ver 8.0.28 for Linux on x86_64 (MySQL Community Server - GPL)

```
Подключитесь к восстановленной БД и получите список таблиц из этой БД.  
```bash
mysql> use test_db;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+-------------------+
| Tables_in_test_db |
+-------------------+
| orders            |
+-------------------+
1 row in set (0.00 sec)
```
Приведите в ответе количество записей с price > 300.  
```bash
mysql> select count(*) from orders where price > 300;
+----------+
| count(*) |
+----------+
|        1 |
+----------+
1 row in set (0.00 sec)
```
В следующих заданиях мы будем продолжать работу с данным контейнером.  

## Задача 2
Создайте пользователя test в БД c паролем test-pass, используя:

плагин авторизации mysql_native_password
срок истечения пароля - 180 дней
количество попыток авторизации - 3
максимальное количество запросов в час - 100
аттрибуты пользователя:
Фамилия "Pretty"
Имя "James"
Предоставьте привелегии пользователю test на операции SELECT базы test_db.

Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю test и приведите в ответе к задаче.  
```bash
mysql> CREATE USER 'test'@'localhost' IDENTIFIED BY 'myparol';
Query OK, 0 rows affected (0.01 sec)
mysql> select * from INFORMATION_SCHEMA.USER_ATTRIBUTES;
+------------------+-----------+-----------+
| USER             | HOST      | ATTRIBUTE |
+------------------+-----------+-----------+
| root             | %         | NULL      |
| mysql.infoschema | localhost | NULL      |
| mysql.session    | localhost | NULL      |
| mysql.sys        | localhost | NULL      |
| root             | localhost | NULL      |
| test             | localhost | NULL      |
+------------------+-----------+-----------+
6 rows in set (0.00 sec)

mysql> ALTER USER 'test'@'localhost'
    -> IDENTIFIED BY 'myparol'
    -> WITH
    -> MAX_QUERIES_PER_HOUR 100
    -> PASSWORD EXPIRE INTERVAL 180 DAY
    -> FAILED_LOGIN_ATTEMPTS 3 PASSWORD_LOCK_TIME UNBOUNDED
    -> ATTRIBUTE '{"fam": "Pretty", "name": "James"}';
Query OK, 0 rows affected (0.02 sec)
mysql> select * from INFORMATION_SCHEMA.USER_ATTRIBUTES;
+------------------+-----------+------------------------------------+
| USER             | HOST      | ATTRIBUTE                          |
+------------------+-----------+------------------------------------+
| root             | %         | NULL                               |
| mysql.infoschema | localhost | NULL                               |
| mysql.session    | localhost | NULL                               |
| mysql.sys        | localhost | NULL                               |
| root             | localhost | NULL                               |
| test             | localhost | {"fam": "Pretty", "name": "James"} |
+------------------+-----------+------------------------------------+
6 rows in set (0.01 sec)

mysql> GRANT Select ON test_db.orders TO 'test'@'localhost';
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> SELECT * FROM INFORMATION_SCHEMA.USER_ATTRIBUTES WHERE USER='test';
+------+-----------+------------------------------------+
| USER | HOST      | ATTRIBUTE                          |
+------+-----------+------------------------------------+
| test | localhost | {"fam": "Pretty", "name": "James"} |
+------+-----------+------------------------------------+
1 row in set (0.00 sec)

```

Задача 3
Установите профилирование SET profiling = 1. Изучите вывод профилирования команд SHOW PROFILES;.

Исследуйте, какой engine используется в таблице БД test_db и приведите в ответе.

Измените engine и приведите время выполнения и запрос на изменения из профайлера в ответе:

на MyISAM
на InnoDB
Задача 4
Изучите файл my.cnf в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):

Скорость IO важнее сохранности данных
Нужна компрессия таблиц для экономии места на диске
Размер буффера с незакомиченными транзакциями 1 Мб
Буффер кеширования 30% от ОЗУ
Размер файла логов операций 100 Мб
Приведите в ответе измененный файл my.cnf.
