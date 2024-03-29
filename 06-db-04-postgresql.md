Задача 1

Используя Docker, поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

Подключитесь к БД PostgreSQL, используя psql.

Воспользуйтесь командой \? для вывода подсказки по имеющимся в psql управляющим командам.

Найдите и приведите управляющие команды для:

    вывода списка БД,
    подключения к БД,
    вывода списка таблиц,
    вывода описания содержимого таблиц,
    выхода из psql.

Ответ:

1.Создадим docker-compose файл.

![](Screenshots/6.4.11.png)

2.Создадим сервис, запустим его, подключимся к контейнеру и psql.

![](Screenshots/6.4.12.png)

3.Вывод списка БД.

    \l+ *

![](Screenshots/6.4.13.png)

4.Подключение к БД.

    \connect postgres;

![](Screenshots/6.4.14.png)

5.Вывод списка таблиц.

    \dtS+

![](Screenshots/6.4.15.png)

6.Вывод описания содержимого таблиц.

    \dS+ pg_user_mapping

![](Screenshots/6.4.16.png)

7.Выход из psql.

    \q

![](Screenshots/6.4.17.png)

Задача 2

Используя psql, создайте БД test_database.

Изучите бэкап БД.

Восстановите бэкап БД в test_database.

Перейдите в управляющую консоль psql внутри контейнера.

Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.

Используя таблицу pg_stats, найдите столбец таблицы orders с наибольшим средним значением размера элементов в байтах.

Приведите в ответе команду, которую вы использовали для вычисления, и полученный результат.

Ответ:

1.Присвоим права на папки для пользователя postgres.

    chown postgres:postgres /var/db-data/

    chown postgres:postgres /var/db-backup/

![](Screenshots/6.4.21.png)

2.Копируем бэкап БД в докер, создадим бд в контейнере и загрузим backup.

    CREATE TABLESPACE "test-tablespace" OWNER CURRENT_USER LOCATION '/var/db-data';

    CREATE DATABASE "test_database" WITH TABLESPACE = "test-tablespace";

    $ psql --dbname=test_database --file=test_dump.sql


![](Screenshots/6.4.22.png)

3.Подключаемся к восстановленной БД и проводим операцию ANALYZE. Используем select для выбора максимального значения.

    \connect test_database;

    analyze;

    select max(avg_width),attname from pg_stats where tablename ='orders' group by attname;

![](Screenshots/6.4.23.png)

Задача 3

Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и поиск по ней занимает долгое время. Вам как успешному выпускнику курсов DevOps в Нетологии предложили провести разбиение таблицы на 2: шардировать на orders_1 - price>499 и orders_2 - price<=499.

Предложите SQL-транзакцию для проведения этой операции.

Можно ли было изначально исключить ручное разбиение при проектировании таблицы orders?

Ответ:

В качестве решения можно использовать следующие транзакции

    CREATE TABLE orders_1 (CHECK (price > 499)) INHERITS (orders);

    CREATE TABLE orders_2 (CHECK (price < 499)) INHERITS (orders);

Информация о таблице orders после выполнения транзакции.

![](Screenshots/6.4.31.png)

Ручного разбиения таблицы можно было избежать изначально спроектировав её разделённой при помощи PARTITION BY.

Задача 4

Используя утилиту pg_dump, создайте бекап БД test_database.

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?

Ответ:

1.выгрузим dump и назавём его [dump_test_database.sql](https://github.com/VitaliyW88/devops-netology/blob/c438725835d0138bce4e0d07ded88bba97ab2465/Files/dump_test_database.sql)

    pg_dump -U postgres -d test_database > dump_test_database.sql

![](Screenshots/6.4.41.png)

2.Файл бекапа представляет из себя набор SQL команд для воссоздания таблиц. Чтобы добавить уникальность значения столбца title достаточно добавить атрибут UNIQUE и изменить соответствующие команды в бекапе базы.

Например:

было

    CREATE TABLE public.orders (
        id integer NOT NULL,
        title character varying(80) NOT NULL,
        price integer DEFAULT 0
    );

стало

    CREATE TABLE public.orders (
        id integer NOT NULL,
        title character varying(80) UNIQUE NOT NULL,
        price integer DEFAULT 0
    );