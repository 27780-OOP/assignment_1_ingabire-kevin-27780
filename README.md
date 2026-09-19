# PLSQL Assignment One - Sunrise Supermarket

## Student Information
- **Name:** Ingabire Kevin
- **Student ID:** 27780

## Summary of Work
For this assignment, I designed and populated a relational database for "Sunrise Supermarket" using Oracle SQL (specifically Oracle Live SQL). I created four tables (`customers`, `products`, `orders`, `order_items`) and populated them with data meeting the minimum requirements (5 customers, 8 products, 15 orders, 25 order items). I then wrote and executed a series of analytical queries using INNER JOINs, LEFT JOINs, Common Table Expressions (CTEs), and Window Functions to answer specific business questions.

## How to Run
To run this project, simply copy the SQL scripts in the order provided below into any Oracle SQL environment (like Oracle Live SQL or SQL Developer).

1. **Schema Creation:** Copy the `CREATE TABLE` statements provided below.
2. **Data Population:** Copy the `INSERT` statements provided in the "Data Insertion" section below.
3. **Analysis Queries:** Run each of the numbered queries below to see the results.

## Business Scenario
Sunrise Supermarket sells products to customers, who place orders containing one or more items. Management wants to understand who their customers are, what they buy, and how sales are trending over time.

---

## Schema Creation

```sql
CREATE TABLE customers (
    customer_id NUMBER PRIMARY KEY,
    customer_name VARCHAR2(100),
    email VARCHAR2(100),
    city VARCHAR2(50)
);

CREATE TABLE products (
    product_id NUMBER PRIMARY KEY,
    product_name VARCHAR2(100),
    category VARCHAR2(50),
    price NUMBER(10,2)
);

CREATE TABLE orders (
    order_id NUMBER PRIMARY KEY,
    customer_id NUMBER REFERENCES customers(customer_id),
    order_date DATE
);

CREATE TABLE order_items (
    order_item_id NUMBER PRIMARY KEY,
    order_id NUMBER REFERENCES orders(order_id),
    product_id NUMBER REFERENCES products(product_id),
    quantity NUMBER
);
-- Insert Customers (Note: Customer 5 has no orders to test LEFT JOIN later)
INSERT INTO customers VALUES (1, 'Alice Smith', 'alice@email.com', 'New York');
INSERT INTO customers VALUES (2, 'Bob Johnson', 'bob@email.com', 'Chicago');
INSERT INTO customers VALUES (3, 'Charlie Brown', 'charlie@email.com', 'New York');
INSERT INTO customers VALUES (4, 'Diana Prince', 'diana@email.com', 'Los Angeles');
INSERT INTO customers VALUES (5, 'Evan Wright', 'evan@email.com', 'Chicago');

-- Insert Products (8 products across 3 categories)
INSERT INTO products VALUES (101, 'Laptop', 'Electronics', 1200.00);
INSERT INTO products VALUES (102, 'Smartphone', 'Electronics', 800.00);
INSERT INTO products VALUES (103, 'Headphones', 'Electronics', 150.00);
INSERT INTO products VALUES (104, 'Coffee Maker', 'Home Appliances', 80.00);
INSERT INTO products VALUES (105, 'Blender', 'Home Appliances', 45.00);
INSERT INTO products VALUES (106, 'Desk Lamp', 'Home Appliances', 25.00);
INSERT INTO products VALUES (107, 'T-Shirt', 'Clothing', 20.00);
INSERT INTO products VALUES (108, 'Jeans', 'Clothing', 50.00);

-- Insert Orders (15 orders across different dates for the same customers)
INSERT INTO orders VALUES (1001, 1, TO_DATE('2026-08-01', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1002, 1, TO_DATE('2026-08-15', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1003, 2, TO_DATE('2026-08-02', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1004, 3, TO_DATE('2026-08-05', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1005, 3, TO_DATE('2026-08-20', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1006, 4, TO_DATE('2026-08-10', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1007, 4, TO_DATE('2026-08-25', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1008, 1, TO_DATE('2026-09-01', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1009, 2, TO_DATE('2026-09-02', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1010, 3, TO_DATE('2026-09-05', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1011, 4, TO_DATE('2026-09-10', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1012, 1, TO_DATE('2026-09-12', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1013, 2, TO_DATE('2026-09-15', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1014, 3, TO_DATE('2026-09-18', 'YYYY-MM-DD'));
INSERT INTO orders VALUES (1015, 4, TO_DATE('2026-09-19', 'YYYY-MM-DD'));

-- Insert Order Items (25 items in total)
INSERT INTO order_items VALUES (1, 1001, 101, 1);
INSERT INTO order_items VALUES (2, 1001, 103, 2);
INSERT INTO order_items VALUES (3, 1002, 107, 3);
INSERT INTO order_items VALUES (4, 1003, 102, 1);
INSERT INTO order_items VALUES (5, 1003, 104, 1);
INSERT INTO order_items VALUES (6, 1004, 105, 1);
INSERT INTO order_items VALUES (7, 1004, 106, 2);
INSERT INTO order_items VALUES (8, 1005, 108, 1);
INSERT INTO order_items VALUES (9, 1005, 107, 1);
INSERT INTO order_items VALUES (10, 1006, 101, 1);
INSERT INTO order_items VALUES (11, 1006, 102, 1);
INSERT INTO order_items VALUES (12, 1007, 103, 1);
INSERT INTO order_items VALUES (13, 1007, 104, 1);
INSERT INTO order_items VALUES (14, 1007, 105, 1);
INSERT INTO order_items VALUES (15, 1008, 106, 4);
INSERT INTO order_items VALUES (16, 1009, 107, 2);
INSERT INTO order_items VALUES (17, 1009, 108, 1);
INSERT INTO order_items VALUES (18, 1010, 101, 1);
INSERT INTO order_items VALUES (19, 1011, 102, 2);
INSERT INTO order_items VALUES (20, 1011, 103, 1);
INSERT INTO order_items VALUES (21, 1012, 104, 1);
INSERT INTO order_items VALUES (22, 1013, 105, 2);
INSERT INTO order_items VALUES (23, 1014, 106, 1);
INSERT INTO order_items VALUES (24, 1014, 107, 1);
INSERT INTO order_items VALUES (25, 1015, 108, 2);

