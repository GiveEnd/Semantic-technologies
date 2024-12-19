## Лабораторная 

 - Создание реляционной базы данных.
 - Создание запросов к реляционной базе данных.
 - Создание индексов к реляционной базе данных.
 - Сбор статистики.
 -  Анализ производительности запросов и индексов с применением встроенных функций СУБД.
 
 **Создание базы данных**

     CREATE TABLE products (
        product_id SERIAL PRIMARY KEY,
        product_name VARCHAR(100) NOT NULL,
        price DECIMAL(10, 2) NOT NULL
    );
    
    CREATE TABLE orders (
        order_id SERIAL PRIMARY KEY,
        product_id INT REFERENCES products(product_id),
        quantity INT NOT NULL,
        order_date TIMESTAMP DEFAULT NOW()
    );


**Заполнение базы данных**

    INSERT INTO products (product_name, price)
    VALUES 
    ('Product A', 10.50),
    ('Product B', 20.00),
    ('Product C', 15.75);
    
    INSERT INTO orders (product_id, quantity)
    VALUES 
    (1, 2),
    (2, 1),
    (1, 5),
    (3, 3);

**Создание запросов**
 Запрос для получения списка всех заказов с их общей стоимостью.

    SELECT o.order_id, p.product_name, o.quantity, (o.quantity * p.price) AS total_price
    FROM orders o
    JOIN products p ON o.product_id = p.product_id;

![enter image description here](https://i.imgur.com/KNLgXvo.png)
Запрос для получения товаров с ценой выше 15

    SELECT * FROM products WHERE price > 15;

![enter image description here](https://i.imgur.com/ytAyhEh.png)

**Создание индекса**
Индексы на таблице `orders` для ускорения выборок.

    CREATE INDEX idx_product_id ON orders (product_id);
    CREATE INDEX idx_order_date ON orders (order_date);

![enter image description here](https://i.imgur.com/Wyal6il.png)

**Сбор статистики и анализ производительности**
План выполнения запроса.

    EXPLAIN SELECT o.order_id, p.product_name, o.quantity, (o.quantity * p.price) AS total_price
    FROM orders o
    JOIN products p ON o.product_id = p.product_id;

![enter image description here](https://i.imgur.com/v4Gvb3s.png)
Анализ результата.

    EXPLAIN ANALYZE SELECT * FROM orders WHERE product_id = 1;
![enter image description here](https://i.imgur.com/aEVdwEt.png)

Сравнение по времени с индексом и без.
Без индекса
![enter image description here](https://i.imgur.com/VjX9HKz.png)
С индексом
![enter image description here](https://i.imgur.com/WDt5Pey.png)

Вывод
Добавление индексов уменьшило Planning Time на 0.452 ms.
