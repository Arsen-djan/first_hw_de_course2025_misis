# Как работать с репо
Данный репозиторий публичный, для того чтобы сдать дз необходимо сделать Fork репозитория и загрузить sql-скрипт с решениями задач. Решение всех зада разместить в одном скрипте. По Вашим форкам найдем сданные работы и проверим. Удачи!

# Домашнее задание №1. PostgreSQL
В данной домашней работе вам предстоит создать свою БД в постгресе по заданной схеме и составить нужные запросы. Результат работы нужно сохранить в виде файла в формате `file_name.sql`. Перед каждым скриптом нужно написать номер задания в виде комментария. Пример:  
```
-- 1. Создание таблиц --
CREATE TABLE awesome_table (
    id SERIAL primary key,
    name VARCHAR(32) NOT NULL,
    description TEXT
)

-- 2. Глупенькие селектики --
SELECT * FROM awesome_table
```
Скрипты вставки данных, которые будут даны в этом файле, ВСТАВЛЯТЬ В ВАШ ФАЙЛ НЕ НАДО


## 1. Инициализация схемы БД
Итак, это первый этап данного домашнего задания. Дана схема простой базы данных с информацией о заказах. 
Вам нужно написать SQL запросы для инициализации всех таблиц (типы и названия даны, их менять не нужно), 
самим придумать возможные ограничения на столбцы. Сильно мудрить не нужно, главное - правильно проставьте 
внешние ключи, по логике можете добавить ограничения на NULL, DEFAULT, CHECK (например, для размера платежа, 
он явно не может быть отрицательным). Далее заполните эти таблицы с помощью запросов, которые будет даны далее.

### 1. users

| Столбец     | Тип                   |
|------------|----------------------|
| id         | integer              |
| name       | character varying(100) |
| email      | character varying(150) |
| created_at | timestamp            |

```sql
INSERT INTO users (id, name, email, created_at) VALUES
(1, 'Иван Иванов', 'ivan@example.com', '2023-01-15 10:00:00'),
(2, 'Петр Петров', 'petr@example.com', '2023-02-10 11:00:00'),
(3, 'Сидор Сидоров', 'sidor@example.com', '2023-03-05 12:00:00'),
(4, 'Анна Аннова', 'anna@example.com', '2023-04-20 13:00:00'),
(5, 'Мария Морина', 'maria@example.com', '2023-05-25 14:00:00')
(6, 'Дементий Деменьтев', 'dementiy@example.com', '2023-01-07 17:00:00');
```

### 2. categories

| Столбец     | Тип                   |
|------------|----------------------|
| id         | integer              |
| name       | character varying(100) |

```sql
INSERT INTO categories (id, name) VALUES
(1, 'Электроника'),
(2, 'Одежда'),
(3, 'Книги'),
(4, 'Мебель'),
(5, 'Спорт');
```

### 3. products

| Столбец      | Тип                  |
|-------------|-----------------------|
| id          | integer               |
| name        | character varying(100)|
| price       | numeric(10,2)         |
| category_id | integer               |


```sql
INSERT INTO products (id, name, price, category_id) VALUES
(1, 'Смартфон', 29999.99, 1),
(2, 'Ноутбук', 59999.99, 1),
(3, 'Футболка', 1999.99, 2),
(4, 'Джинсы', 4999.99, 2),
(5, 'Книга "SQL для начинающих"', 999.99, 3),
(6, 'Книга "Python для профессионалов"', 1499.99, 3),
(7, 'Диван', 29999.99, 4),
(8, 'Стул', 4999.99, 4),
(9, 'Велосипед', 19999.99, 5),
(10, 'Гантели', 2999.99, 5);
```

### 4. orders

| Столбец    | Тип                  |
|------------|----------------------|
| id         | integer              |
| user_id    | integer              |
| status     | character varying(50)|
| created_at | timestamp            |

```sql
INSERT INTO orders (id, user_id, status, created_at) VALUES
(1, 1, 'Оплачен', '2023-01-20 10:00:00'),
(2, 2, 'Ожидает оплаты', '2023-02-15 11:00:00'),
(3, 3, 'Доставлен', '2023-03-10 12:00:00'),
(4, 4, 'Оплачен', '2023-04-05 13:00:00'),
(5, 5, 'Ожидает оплаты', '2023-05-01 14:00:00'),
(6, 1, 'Доставлен', '2023-01-25 15:00:00'),
(7, 2, 'Оплачен', '2023-02-20 16:00:00'),
(8, 3, 'Ожидает оплаты', '2023-03-15 17:00:00'),
(9, 4, 'Доставлен', '2023-04-10 18:00:00'),
(10, 5, 'Оплачен', '2023-05-05 19:00:00'),
(11, 1, 'Ожидает оплаты', '2023-01-30 20:00:00'),
(12, 2, 'Доставлен', '2023-03-25 21:00:00'),
(13, 3, 'Оплачен', '2023-03-20 22:00:00'),
(14, 4, 'Ожидает оплаты', '2023-04-15 23:00:00'),
(15, 5, 'Доставлен', '2023-05-10 10:00:00'),
(16, 1, 'Оплачен', '2023-01-05 11:00:00'),
(17, 2, 'Ожидает оплаты', '2023-02-28 12:00:00'),
(18, 3, 'Доставлен', '2023-03-25 13:00:00'),
(19, 4, 'Оплачен', '2023-04-20 14:00:00'),
(20, 5, 'Ожидает оплаты', '2023-06-15 15:00:00');
```

### 5. order_items

| Столбец    | Тип                   |
|-----------|----------------------|
| id        | integer              |
| order_id  | integer              |
| product_id| integer              |
| quantity  | integer              |

```sql
INSERT INTO order_items (id, order_id, product_id, quantity) VALUES
(1, 1, 1, 1),
(2, 1, 2, 1),
(3, 2, 3, 2),
(4, 2, 4, 1),
(5, 3, 5, 1),
(6, 3, 6, 1),
(7, 4, 7, 1),
(8, 4, 8, 2),
(9, 5, 9, 1),
(10, 5, 10, 1),
(11, 6, 1, 1),
(12, 6, 3, 1),
(13, 7, 2, 1),
(14, 7, 4, 1),
(15, 8, 5, 1),
(16, 8, 7, 1),
(17, 9, 6, 1),
(18, 9, 8, 1),
(19, 10, 9, 1),
(20, 10, 10, 1),
(21, 11, 1, 1),
(22, 11, 2, 1),
(23, 12, 3, 1),
(24, 12, 4, 1),
(25, 13, 5, 1),
(26, 13, 6, 1),
(27, 14, 7, 1),
(28, 14, 8, 1),
(29, 15, 9, 1),
(30, 15, 10, 1),
(31, 16, 1, 1),
(32, 16, 3, 1),
(33, 17, 2, 1),
(34, 17, 4, 1),
(35, 18, 5, 1),
(36, 18, 7, 1),
(37, 19, 6, 1),
(38, 19, 8, 1),
(39, 20, 9, 1),
(40, 20, 10, 1);
```

### 6. payments

| Столбец      | Тип                   |
|-------------|----------------------|
| id          | integer              |
| order_id    | integer              |
| amount      | numeric(10,2)        |
| payment_date| timestamp            |

```sql
INSERT INTO payments (id, order_id, amount, payment_date) VALUES
(1, 1, 89999.98, '2023-01-20 10:05:00'),
(2, 2, 8999.97, NULL),
(3, 3, 2499.98, '2023-03-10 12:05:00'),
(4, 4, 39999.97, '2023-04-05 13:05:00'),
(5, 5, 22999.98, NULL),
(6, 6, 31999.98, '2023-01-25 15:05:00'),
(7, 7, 64999.98, '2023-02-20 16:05:00'),
(8, 8, 30999.98, NULL),
(9, 9, 6499.98, '2023-04-10 18:05:00'),
(10, 10, 22999.98, '2023-05-05 19:05:00'),
(11, 11, 89999.98, NULL),
(12, 12, 6999.98, '2023-02-25 21:05:00'),
(13, 13, 2499.98, '2023-03-20 22:05:00'),
(14, 14, 34999.98, NULL),
(15, 15, 22999.98, '2023-05-10 10:05:00'),
(16, 16, 31999.98, '2023-01-05 11:05:00'),
(17, 17, 64999.98, NULL),
(18, 18, 30999.98, '2023-03-25 13:05:00'),
(19, 19, 6499.98, '2023-04-20 14:05:00'),
(20, 20, 22999.98, NULL);
```

## 2. Что как делаем
Задач будет немного - 8 штук, для их решения надо знать следующие темы:
- SELECT, выборка полей и алиасы
- Фильтрация с WHERE
- GROUP BY, фильтрация с HAVING, агрегационные функции (SUM, AVG, etc.)
- ORDER BY
- JOIN'ы
- SUBQUERY, они же подзапросы
- Самое страшное - оконные функции

После каждой задачи будет дан результат, который должен у вас получиться. Названия столбцов также должны совпадать.
Все дробные значения в запросах округлять до двух знаков после запятой.

### Задача 1. Средняя стоимость заказа по категориям товаров
Вывести среднюю cуммарную стоимость товаров в заказе для каждой категории товаров, учитывая только заказы, созданные в марте 2023 года. 

Результат:
|category_name|avg_order_amount  | 
|-------------|------------------|
|Книги|	1749.99|
|Мебель|	29999.99|
|Одежда|	6999.98|

### Задача 2. Рейтинг пользователей по сумме оплаченных заказов
Вывести топ-3 пользователей, которые потратили больше всего денег на оплаченные заказы. Учитывать только заказы со статусом "Оплачен".
В отдельном столбце указать, какое место пользователь занимает

|user_name|total_spent|user_rank|
|-|-|-|
|Иван Иванов	|121999.96|	1|
|Петр Петров	|64999.98|	2|
|Анна Аннова	|46499.95|	3|

### Задача 3. Количество заказов и сумма платежей по месяцам
Вывести количество заказов и общую сумму платежей по каждому месяцу в 2023 году.
|month|total_orders|total_payments|
|-|-|-|
|2023-01|	4|	243999.92|
|2023-02|	3|	138999.93|
|2023-03|	5|	73999.90|
|2023-04|	4|	87999.91|
|2023-05|	3|	68999.94|
|2023-06|	1|	22999.98|

### Задача 4. Рейтинг товаров по количеству продаж
Вывести топ-5 товаров по количеству продаж, а также их долю в общем количестве продаж. 
Долю округлить до двух знаков после запятой
|product_name|total_sold|sales_percantage|
|-|-|-|
|Футболка	|5|	11.90|
|Стул	|5|	11.90|
|Смартфон	|4|	9.52|
|Книга "Python для профессионалов"	|4|	9.52|
|Книга "SQL для начинающих"	|4|	9.52|

### Задача 5. Пользователи, которые сделали заказы на сумму выше среднего
Вывести пользователей, общая сумма оплаченных заказов которых превышает 
среднюю сумму оплаченных заказов по всем пользователям.
|user_name|total_spent|
|-|-|
|Иван Иванов	|121999.96|
|Петр Петров	|64999.98|

### Задача 6. Рейтинг товаров по количеству продаж в каждой категории
Для каждой категории товаров вывести топ-3 товара по количеству проданных единиц. 
Используйте оконную функцию для ранжирования товаров внутри каждой категории.

|category_name|product_name|total_sold|
|-|-|-|
|Книги|	Книга "Python для профессионалов"	|4|
|Книги|	Книга "SQL для начинающих"	|4|
|Мебель|	Стул	|5|
|Мебель|	Диван	|4|
|Одежда|	Футболка	|5|
|Одежда|	Джинсы	|4|
|Спорт|	Гантели	|4|
|Спорт|	Велосипед	|4|
|Электроника|	Смартфон	|4|
|Электроника|	Ноутбук	|4|

### Задача 7. Категории товаров с максимальной выручкой в каждом месяце
Вывести категории товаров, которые принесли максимальную выручку 
в каждом месяце первого полугодия 2023 года.

|month|category_name|total_revenue|
|-|-|-|
|2023-01|Электроника|239999.94|
|2023-02|Электроника|119999.98|
|2023-03|Мебель|59999.98|
|2023-04|Мебель|84999.93|
|2023-05|Спорт|68999.94|
|2023-06|Спорт|22999.98|


### Задача 8. Накопительная сумма платежей по месяцам
Вывести накопительную сумму платежей по каждому месяцу в 2023 году. 
Накопительная сумма должна рассчитываться нарастающим итогом. Подсказка: нужно понять, как работает ROWS BETWEEN,
и какое ограничение используется по умолчанию для SUM BY

|month|monthly_payments|cumulative_payments|
|-|-|-|
|2023-01	|153999.94	|153999.94|
|2023-02	|71999.96	|225999.90|
|2023-03	|35999.94	|261999.84|
|2023-04	|52999.93	|314999.77|
|2023-05	|45999.96	|360999.73|
