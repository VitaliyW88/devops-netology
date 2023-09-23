Задача 1

Используя Docker, поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.

Приведите получившуюся команду или docker-compose-манифест.

Ответ:

1.Cоздадим необходимы папки:

$ mkdir /home/vagrant/postgresql
$ mkdir /home/vagrant/postgresql/data
$ mkdir /home/vagrant/postgresql/backup

2.Cоздадим docker-compose.yml файл:

version: '2.1'
services:
  db:
    image: postgres:12
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - /home/vagrant/postgresql/data:/var/db-data
      - /home/vagrant/postgresql/backup:/var/db-backup

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