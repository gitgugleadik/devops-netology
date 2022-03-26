### Домашнее задание к занятию "6.4. PostgreSQL"  
## Задача 1  
Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

Подключитесь к БД PostgreSQL используя psql.

Воспользуйтесь командой \? для вывода подсказки по имеющимся в psql управляющим командам.
```bash
root@bento:~# docker pull postgres:13
root@bento:~# docker volume create vol1
vol1
root@bento:~# docker run --name postgres1 -e POSTGRES_PASSWORD=postgres -d -p 5432:5432 -v vol1:/var/lib/postgresql/data postgres:13
7c5004c4397b79930b8e4d40e0a9933abe8ca4de2fbcbcde00cb4dd21a47a60f
root@bento:~# docker exec -it postgres1 bash
root@7c5004c4397b:/# psql -U postgres
psql (13.6 (Debian 13.6-1.pgdg110+1))
Type "help" for help.

postgres=#
```


Найдите и приведите управляющие команды для: 

вывода списка БД
подключения к БД
вывода списка таблиц
вывода описания содержимого таблиц
выхода из psql
```bash

 \l[+]   [PATTERN]      list databases
\c[onnect] {[DBNAME|- USER|- HOST|- PORT|-] | conninfo}     connect to new database (currently "postgres")
 \dt[S+] [PATTERN]      list tables
   \d[S+]  NAME           describe table, view, sequence, or index
   \q
   ```
## Задача 2 
Используя psql создайте БД test_database.

Изучите бэкап БД.

Восстановите бэкап БД в test_database.

Перейдите в управляющую консоль psql внутри контейнера.

Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.

Используя таблицу pg_stats, найдите столбец таблицы orders с наибольшим средним значением размера элементов в байтах.

Приведите в ответе команду, которую вы использовали для вычисления и полученный результат.

## Задача 3 
Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.

Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

## Задача 4 
Используя утилиту pg_dump создайте бекап БД test_database.

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?
