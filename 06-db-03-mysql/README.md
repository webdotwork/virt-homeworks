# Домашнее задание к занятию "6.3. MySQL"

## Введение

Перед выполнением задания вы можете ознакомиться с 
[дополнительными материалами](https://github.com/netology-code/virt-homeworks/tree/master/additional/README.md).

## Задача 1

Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.
```
version: '3.1'

volumes:
  data: {}
  backup: {}

services:

  db:
    image: mysql:8.0-oracle
    container_name: mysql
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    volumes:
     - ./data:/var/lib/mysql
     - ./backup:/media/mysql/backup
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: test_db
```

Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-03-mysql/test_data) и 
восстановитесь из него.
```
mysql -u root -p ${test_db} < test_dump.sql
```
Перейдите в управляющую консоль `mysql` внутри контейнера.
```
mysql -h 127.0.0.1 -u root -p
```
Используя команду `\h` получите список управляющих команд.

Найдите команду для выдачи статуса БД и **приведите в ответе** из ее вывода версию сервера БД.
![image](https://user-images.githubusercontent.com/40559167/169691190-6b9b7588-9e0a-47c3-a32e-341330a2f624.png)


Подключитесь к восстановленной БД и получите список таблиц из этой БД.
![image](https://user-images.githubusercontent.com/40559167/169691129-3beff22e-0c93-4fc8-87de-fc5537b7bcad.png)

**Приведите в ответе** количество записей с `price` > 300.
```
select * from orders where price > 300;
```
![image](https://user-images.githubusercontent.com/40559167/169691286-0edd1b12-7d4f-4a4d-8e5f-d45ab17d42a6.png)


В следующих заданиях мы будем продолжать работу с данным контейнером.

## Задача 2

Создайте пользователя test в БД c паролем test-pass, используя:
- плагин авторизации mysql_native_password
- срок истечения пароля - 180 дней 
- количество попыток авторизации - 3 
- максимальное количество запросов в час - 100
- аттрибуты пользователя:
    - Фамилия "Pretty"
    - Имя "James"

Предоставьте привелегии пользователю `test` на операции SELECT базы `test_db`.
    
Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю `test` и 
**приведите в ответе к задаче**.

## Задача 3

Установите профилирование `SET profiling = 1`.
Изучите вывод профилирования команд `SHOW PROFILES;`.

Исследуйте, какой `engine` используется в таблице БД `test_db` и **приведите в ответе**.

Измените `engine` и **приведите время выполнения и запрос на изменения из профайлера в ответе**:
- на `MyISAM`
- на `InnoDB`

## Задача 4 

Изучите файл `my.cnf` в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):
- Скорость IO важнее сохранности данных
- Нужна компрессия таблиц для экономии места на диске
- Размер буффера с незакомиченными транзакциями 1 Мб
- Буффер кеширования 30% от ОЗУ
- Размер файла логов операций 100 Мб

Приведите в ответе измененный файл `my.cnf`.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
