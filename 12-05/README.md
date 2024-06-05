# Домашнее задание к занятию "`Индексы`" - `Кандала Кирилл`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

`При необходимости прикрепитe сюда скриншоты

![Задание 1](https://github.com/wintercomesX/12-05/blob/main/12-05/img/task1.PNG)`


---

### Задание 2

`Приведите ответ в свободной форме........`

1. `Узкие места:
   1. Полный скан таблицы (Full Table Scan): Если в таблице payment нет индекса по payment_date, то запрос будет сканировать всю таблицу, что может быть очень затратным.
   2. Перекрёстное соединение (Cross Join): Использование явного JOIN улучшает читаемость и выполнение запроса по сравнению с написанным через запятую.
   3. Отсутствие индексов: Отсутствие индексов по полям, участвующим в соединении (JOIN) и фильтрации (WHERE), сильно замедляет выполнение запросов.`
2. `Оптимизация запроса:
   1. Обновление синтаксиса JOIN: Использование явных JOIN улучшает читаемость и план выполнения запроса.
   2. Добавление индексов: Индексы на полях payment_date, rental_date, customer_id, inventory_id, и film_id могут значительно улучшить производительность.`

```
Поле для вставки кода...

#Добавляем индексы для улучшения производительности

CREATE INDEX idx_payment_date ON payment (payment_date);
CREATE INDEX idx_rental_date ON rental (rental_date);
CREATE INDEX idx_customer_id ON rental (customer_id);
CREATE INDEX idx_inventory_id ON rental (inventory_id);
CREATE INDEX idx_film_id ON inventory (film_id);

#Оптимизированный запрос

EXPLAIN FORMAT=JSON
SELECT DISTINCT 
    CONCAT(c.last_name, ' ', c.first_name) AS customer_name, 
    SUM(p.amount) OVER (PARTITION BY c.customer_id, f.title) AS total_amount
FROM 
    payment p
    JOIN rental r ON p.rental_id = r.rental_id
    JOIN customer c ON r.customer_id = c.customer_id
    JOIN inventory i ON r.inventory_id = i.inventory_id
    JOIN film f ON i.film_id = f.film_id
WHERE 
    p.payment_date >= '2005-07-30' 
    AND p.payment_date < DATE_ADD('2005-07-30', INTERVAL 1 DAY);

```

`EXPLAIN ANALYZE

![Задание 1](https://github.com/wintercomesX/12-05/blob/main/12-05/img/task2.PNG)`

