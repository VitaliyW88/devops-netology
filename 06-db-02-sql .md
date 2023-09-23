Задача 1

Используя Docker, поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.

Приведите получившуюся команду или docker-compose-манифест.

Ответ:

1.Cоздадим необходимы папки:

$ mkdir /home/vagrant/postgresql
$ mkdir /home/vagrant/postgresql/data
$ mkdir /home/vagrant/postgresql/backup

2.Cоздадим docker-compose.yml файл:

![](Screenshots/6.2.13.png)

3.Проверим что сервис собирается и стартует:

$ sudo docker compose build

$ sudo docker compose create

![](Screenshots/6.2.1.png)

4. Запустим контейнеры:

$ sudo docker compose up -d

5. Смотрим что контейнер запустился и работает:

$ sudo docker compose ps

![](Screenshots/6.2.11.png)

Задача 2

В БД из задачи 1:

создайте пользователя test-admin-user и БД test_db;

в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже);

предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db;

создайте пользователя test-simple-user;

предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE этих таблиц БД test_db.

Таблица orders:

id (serial primary key);
наименование (string);
цена (integer).
Таблица clients:

id (serial primary key);
фамилия (string);
страна проживания (string, index);
заказ (foreign key orders).
Приведите:

итоговый список БД после выполнения пунктов выше;
описание таблиц (describe);
SQL-запрос для выдачи списка пользователей с правами над таблицами test_db;
список пользователей с правами над таблицами test_db.

Ответ:

1.Заходим в контейнер и создаём пользователя test-admin-user и БД test_db:

$ sudo docker exec -it docker-compose-yam-db-1 bash

chown postgres:postgres /var/db-data/

psql -U postgres

CREATE USER "test-admin-user";

CREATE TABLESPACE "test-tablespace" OWNER CURRENT_USER
LOCATION '/var/db-data'
;

CREATE DATABASE "test_db" WITH TABLESPACE = "test-tablespace";

![](Screenshots/6.2.21.png)

2.В БД test_db создайте таблицу orders и clients:

\connect test_db

CREATE TABLE orders(
    id          serial primary key,
    title       text NOT NULL,
    price       integer
);

CREATE TABLE clients(
    id            serial primary key,
    surname       text NOT NULL,
    from_country_id  integer NOT NULL,
    from_country_name text NOT NULL,
    order_id      integer REFERENCES orders
);

![](Screenshots/6.2.22.png)

3.Предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db:

GRANT ALL PRIVILEGES ON DATABASE test_db to "test-admin-user";

GRANT ALL PRIVILEGES ON TABLE orders, clients  to "test-admin-user";

![](Screenshots/6.2.23.png)

4.Создайте пользователя test-simple-user:

CREATE USER "test-simple-user";

![](Screenshots/6.2.24.png)

5.Предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE данных таблиц БД test_db:

GRANT SELECT, INSERT, UPDATE, DELETE ON TABLE orders, clients  to "test-simple-user";

![](Screenshots/6.2.25.png)

6.Итоговый список БД после выполнения пунктов выше:

\list

![](Screenshots/6.2.261.png)

7.Описание таблиц (describe):

\d orders

\d clients

![](Screenshots/6.2.27.png)

8.SQL-запрос для выдачи списка пользователей с правами над таблицами test_db:

SELECT distinct grantee
FROM information_schema.role_table_grants
WHERE table_name in ('orders','clients');

![](Screenshots/6.2.28.png)

9.Список пользователей с правами над таблицами test_db:

SELECT grantee, table_name, privilege_type
FROM information_schema.role_table_grants
WHERE table_name in ('orders','clients');

![](Screenshots/6.2.29.png)

Задача 3
Используя SQL-синтаксис, наполните таблицы следующими тестовыми данными:

Таблица orders

| Наименование | цена |
| --- | --- |
| Шоколад | 10 |
| Принтер | 3000 |
| Книга | 500 |
| Монитор | 7000 |
| Гитара | 4000 |
| Таблица | clients |

| ФИО | Страна проживания |
| --- | --- |
| Иванов Иван Иванович | USA |
| Петров Петр Петрович | Canada |
| Иоганн Себастьян Бах | Japan |
| Ронни Джеймс Дио | Russia |
| Ritchie Blackmore | Russia |

Используя SQL-синтаксис:

вычислите количество записей для каждой таблицы.
Приведите в ответе:

- запросы,
- результаты их выполнения.

Ответ:

