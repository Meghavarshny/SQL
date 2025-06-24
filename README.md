
# MySQL Task-1

Requirements:

Create a database named ecommerce.
Create three tables: customers, orders, and products.
Insert some sample data into the tables.

Table Structure:

customers
id (primary key, auto-increment): unique identifier for each customer
name: customer's name
email: customer's email address
address: customer's address

orders
id (primary key, auto-increment): unique identifier for each order
customer_id (foreign key referencing customers.id): a customer who placed the order
order_date: date the order was placed
total_amount: total amount of the order

products
id (primary key, auto-increment): unique identifier for each product
name: product's name
price: product's price
description: product's description



Queries to Write:
1.Retrieve all customers who have placed an order in the last 30 days.

SELECT DISTINCT c.name, c.email
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.order_date >= CURDATE() - INTERVAL 30 DAY;

2.Get the total amount of all orders placed by each customer.

SELECT c.name, SUM(o.total_amount) AS total_spent
FROM customers c
JOIN orders o ON c.id = o.customer_id
GROUP BY c.name
ORDER BY total_spent DESC;

3.Update the price of Product C to 45.00.

UPDATE products
SET price = 45.00
WHERE name = 'Product C';

4.Add a new column discount to the products table.

ALTER TABLE products
ADD COLUMN discount DECIMAL(5, 2) DEFAULT 0.00;

5.Retrieve the top 3 products with the highest price.

SELECT name, price
FROM products
ORDER BY price DESC
LIMIT 3;

6.Get the names of customers who have ordered Product A.

SELECT DISTINCT c.name
FROM customers c
JOIN orders o ON c.id = o.customer_id
JOIN order_items oi ON o.id = oi.order_id
JOIN products p ON oi.product_id = p.id
WHERE p.name = 'Product A';
   
7.Join the orders and customers tables to retrieve the customer's name and order date for each order. 

SELECT c.name AS customer_name, o.order_date, o.total_amount
FROM orders o
JOIN customers c ON o.customer_id = c.id
ORDER BY o.order_date DESC;

8.Retrieve the orders with a total amount greater than 150.00.

SELECT id, customer_id, order_date, total_amount
FROM orders
WHERE total_amount > 150.00;

9.Normalize the database by creating a separate table for order items and updating the orders table to reference the order_items table.

CREATE TABLE IF NOT EXISTS order_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    price_at_order DECIMAL(10, 2) NOT NULL, -- Price of the product when it was ordered
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

10.Retrieve the average total of all orders.

SELECT AVG(total_amount) AS average_order_total
FROM orders;




