Домашнее задание к занятию «SQL. Часть 2» Макин Олег


Задание можно выполнить как в любом IDE, так и в командной строке.

Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

фамилия и имя сотрудника из этого магазина;
город нахождения магазина;
количество пользователей, закреплённых в этом магазине.

Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

Дополнительные задания (со звёздочкой*)

Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

### Задание 1


    SELECT CONCAT(s.first_name, ' ', s.last_name) AS name, c.city, COUNT(customer_id)
    FROM staff s
    JOIN address a ON a.address_id = s.address_id
    JOIN city c ON c.city_id = a.city_id
    JOIN store s2 ON s2.store_id = s.store_id
    JOIN customer c2 ON c2.store_id = s.store_id
    GROUP BY s.staff_id
    HAVING COUNT(c2.customer_id) > 300;


![img](https://github.com/01eg8/SQL.part2/blob/main/img/Screenshot%20from%202025-03-13%2000-36-28.png)

### Задание 2


    SELECT COUNT(film_id)
    FROM film
    WHERE length > (SELECT AVG(length) FROM film)


![img](https://github.com/01eg8/SQL.part2/blob/main/img/Screenshot%20from%202025-03-13%2000-36-37.png)

### Задание 3

    SELECT date_format(p.payment_date, '%Y-%M') as month1,
    SUM(p.amount) as сумма, COUNT(r.rental_id) as количество
    FROM payment p
    JOIN rental r ON r.rental_id = p.rental_id
    GROUP BY month1
    ORDER BY SUM(p.amount) DESC
    LIMIT 1


![img](https://github.com/01eg8/SQL.part2/blob/main/img/Screenshot%20from%202025-03-13%2000-36-49.png)

### Задание 4

    SELECT staff_id, COUNT(payment_id),
    CASE 
    	WHEN COUNT(payment_id) > 8000 THEN 'YES'
        WHEN COUNT(payment_id) < 8000 THEN 'NO'
    END AS ПРЕМИЯ
    FROM payment
    GROUP BY staff_id
    ORDER BY COUNT(payment_id) DESC


![img](https://github.com/01eg8/SQL.part2/blob/main/img/Screenshot%20from%202025-03-13%2000-37-01.png)

### Задание 5

    select f.title
    from film f
    left join inventory i on i.film_id = f.film_id
    left join rental r on r.inventory_id = i.inventory_id
    WHERE r.rental_id is null


![img](https://github.com/01eg8/SQL.part2/blob/main/img/Screenshot%20from%202025-03-13%2000-37-42.png)

