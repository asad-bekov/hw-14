# Домашнее задание к занятию «SQL. Часть 2»
*Асадбеков Асадбек*

## Задание 1

** Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:**

- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

```bash
SELECT 
    CONCAT(s.first_name, ' ', s.last_name) AS staff_name,
    c.city AS store_city,
    COUNT(cu.customer_id) AS customer_count
FROM 
    store st
JOIN 
    staff s ON st.manager_staff_id = s.staff_id
JOIN 
    address a ON st.address_id = a.address_id
JOIN 
    city c ON a.city_id = c.city_id
JOIN 
    customer cu ON st.store_id = cu.store_id
GROUP BY 
    st.store_id
HAVING 
    customer_count > 300;
```

![alt text](https://github.com/asad-bekov/hw-14/blob/main/img/1.png)

## Задание 2

** Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.**

```bash
SELECT 
    COUNT(*) AS films_above_average
FROM 
    film
WHERE 
    length > (SELECT AVG(length) FROM film);
```
![alt text](https://github.com/asad-bekov/hw-14/blob/main/img/2.png)

## Задание 3

** Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.**

```bash
SELECT 
    DATE_FORMAT(p.payment_date, '%Y-%m') AS payment_month,
    SUM(p.amount) AS total_payments,
    COUNT(r.rental_id) AS rental_count
FROM 
    payment p
JOIN 
    rental r ON p.rental_id = r.rental_id
GROUP BY 
    payment_month
ORDER BY 
    total_payments DESC
LIMIT 1;
```
![alt text](https://github.com/asad-bekov/hw-14/blob/main/img/3.png)

## Задание 4

**Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».**

Сформируйте вывод в результат таким образом:

- все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,

- замените буквы 'll' в именах на 'pp'.

```bash
SELECT 
    s.staff_id,
    CONCAT(s.first_name, ' ', s.last_name) AS staff_name,
    COUNT(p.payment_id) AS sales_count,
    CASE 
        WHEN COUNT(p.payment_id) > 8000 THEN 'Да'
        ELSE 'Нет'
    END AS Премия
FROM 
    staff s
JOIN 
    payment p ON s.staff_id = p.staff_id
GROUP BY 
    s.staff_id;
```
![alt text](https://github.com/asad-bekov/hw-14/blob/main/img/4.png)

## Задание 5*

**Найдите фильмы, которые ни разу не брали в аренду.**

```bash
SELECT 
    f.film_id,
    f.title
FROM 
    film f
LEFT JOIN 
    inventory i ON f.film_id = i.film_id
LEFT JOIN 
    rental r ON i.inventory_id = r.inventory_id
WHERE 
    r.rental_id IS NULL;
```
![alt text](https://github.com/asad-bekov/hw-14/blob/main/img/5.png)


---