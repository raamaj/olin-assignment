# 1. Schema Table
```
CREATE TABLE users (id INT PRIMARY KEY, name VARCHAR(255), email
VARCHAR(255));

CREATE TABLE orders (id INT PRIMARY KEY, user_id INT, amount DECIMAL(10,2),
created_at TIMESTAMP);

CREATE TABLE order_items (id INT PRIMARY KEY, order_id INT, product_name
VARCHAR(255), price DECIMAL(10,2), quantity INT);

INSERT INTO users (id, name, email) VALUES (1, 'John Doe',
'johndoe@example.com'), (2, 'Jane Smith', 'janesmith@example.com'), (3, 'Bob Johnson', 'bobjohnson@example.com');

INSERT INTO orders (id, user_id, amount, created_at) VALUES (1, 1, 100.00, '2022-01-02 10:30:00'), 
(2, 2, 50.00, '2022-01-03 09:00:00'), (3, 1, 150.00, '2022-01-04 14:15:00'), 
(4, 3, 200.00, '2022-01-05 17:45:00'), (5, 2, 75.00, '2022-01-06 11:20:00');

INSERT INTO order_items (id, order_id, product_name, price, quantity) VALUES (1, 1,
'T-Shirt', 25.00, 2), (2, 1, 'Jeans', 50.00, 1), (3, 2, 'Socks', 10.00, 5), (4, 3, 'Shoes',
75.00, 2), (5, 4, 'Jacket', 100.00, 1), (6, 5, 'Sweater', 25.00, 3);
```

### Create Index on Unique Values
```
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_orders_created_at ON orders(created_at);
CREATE INDEX idx_users_id ON users(id);
```

### Select Query
```
SELECT us.id as user_id, us.name as name, us.email as email, SUM(ord.amount) as total_order_amount 
FROM users us 
JOIN orders ord ON us.id = ord.user_id
WHERE ord.created_at >= '2022-01-01' 
GROUP BY us.id
HAVING SUM(ord.amount) >= 100;
```

# 2. Table Schema
```
CREATE DATABASE first;
CREATE TABLE users (id SERIAL PRIMARY KEY, name VARCHAR(255), email
VARCHAR(255));
INSERT INTO users (name, email) VALUES ('John Doe', 'john.doe@example.com'),
('Jane Smith', 'jane.smith@example.com');

CREATE DATABASE second;
CREATE TABLE orders (id SERIAL PRIMARY KEY, user_id INT, amount NUMERIC(10,2),
created_at TIMESTAMP);
INSERT INTO orders (user_id, amount, created_at) VALUES (1, 100.00, now()), (2,
200.00, now()), (1, 50.00, now());
```

### FOREIGN DATA WRAPPER SETUP
```
-- Enable Extension FOREIGN DATA WRAPPER
CREATE EXTENSION postgres_fdw;

-- Define Foreign Server
CREATE SERVER localsrv
FOREIGN DATA WRAPPER postgres_fdw
OPTIONS (host 'localhost', dbname 'first', port '5432');

-- Mapping user foreign user and local user
CREATE USER MAPPING FOR postgres
SERVER localsrv
OPTIONS (user 'postgres', password 'password');

-- Import foreign schema
IMPORT FOREIGN SCHEMA public
FROM SERVER localsrv 
INTO public;
```

### Select Query
```
select users.name, sum(orders.amount) as total_amount , orders.created_at
from users join orders on users.id = orders.user_id
group by users.name, orders.created_at;
```

### Note
> For better permofmace i think use multi schema instead of multi database
