# SQL Joins and Views – Task 7

## Objective

This project demonstrates how to create a database, insert sample data, perform various SQL joins, and create views for reusable and abstracted queries.

---

## 1. Database and Tables Creation

```sql
CREATE DATABASE JOINS;
USE JOINS;

CREATE TABLE ADDRESSES (
    ADDRESS_ID INT PRIMARY KEY,
    STREET VARCHAR(60) NOT NULL,
    CITY VARCHAR(40) NOT NULL,
    STATE VARCHAR(40),
    ZIP_CODE VARCHAR(10)
);

CREATE TABLE CUSTOMERS (
    CUSTOMER_ID INT PRIMARY KEY,
    CUSTOMER_NAME VARCHAR(30) NOT NULL,
    MOBILE_NO BIGINT UNIQUE,
    EMAIL_ID VARCHAR(40) UNIQUE,
    ADDRESS_ID INT NOT NULL,
    FOREIGN KEY (ADDRESS_ID) REFERENCES ADDRESSES(ADDRESS_ID)
);

CREATE TABLE ORDERS (
    ORDER_ID INT PRIMARY KEY,
    ORDER_TYPE VARCHAR(50) NOT NULL,
    CUSTOMER_ID INT NOT NULL,
    ADDRESS_ID INT NOT NULL,
    FOREIGN KEY (CUSTOMER_ID) REFERENCES CUSTOMERS(CUSTOMER_ID),
    FOREIGN KEY (ADDRESS_ID) REFERENCES ADDRESSES(ADDRESS_ID)
);
```

---

## 2. Data Insertion

```sql
INSERT INTO ADDRESSES VALUES
(1, '123 Main St', 'New York', 'NY', '10001'),
(2, '456 Oak Ave', 'Los Angeles', 'CA', '90001'),
(3, '789 Pine Rd', 'Chicago', 'IL', '60601'),
(4, '321 Maple Ln', 'Houston', 'TX', '77001'),
(5, '654 Cedar St', 'Phoenix', 'AZ', '85001'),
(6, '987 Birch Blvd', 'Philadelphia', 'PA', '19101'),
(7, '147 Spruce Dr', 'San Antonio', 'TX', '78201'),
(8, '258 Elm Ct', 'San Diego', 'CA', '92101'),
(9, '369 Willow Way', 'Dallas', 'TX', '75201'),
(10, '159 Ash St', 'San Jose', 'CA', '95101');

INSERT INTO CUSTOMERS VALUES
(1, 'Alice Johnson', 9876543210, 'alice.j@example.com', 1),
(2, 'Bob Smith', 9123456780, 'bob.s@example.com', 2),
(3, 'Charlie Davis', 9988776655, 'charlie.d@example.com', 3),
(4, 'Diana King', 9871234560, 'diana.k@example.com', 4),
(5, 'Ethan Wright', 9765432109, 'ethan.w@example.com', 5),
(6, 'Fiona Scott', 9654321098, 'fiona.s@example.com', 6),
(7, 'George Harris', 9543210987, 'george.h@example.com', 7),
(8, 'Hannah Clark', 9432109876, 'hannah.c@example.com', 8),
(9, 'Ian Lee', 9321098765, 'ian.l@example.com', 9),
(10, 'Julia Turner', 9210987654, 'julia.t@example.com', 10);

INSERT INTO ORDERS VALUES
(1, 'Electronics', 1, 1),
(2, 'Furniture', 2, 2),
(3, 'Clothing', 3, 3),
(4, 'Books', 4, 4),
(5, 'Groceries', 5, 5),
(6, 'Toys', 6, 6),
(7, 'Sports Equipment', 7, 7),
(8, 'Kitchenware', 8, 8),
(9, 'Stationery', 9, 9),
(10, 'Beauty Products', 10, 10),
(11, 'Electronics', 1, 1),
(12, 'Furniture', 2, 2),
(13, 'Clothing', 3, 3),
(14, 'Books', 4, 4),
(15, 'Groceries', 5, 5),
(16, 'Toys', 6, 6),
(17, 'Sports Equipment', 7, 7),
(18, 'Kitchenware', 8, 8),
(19, 'Stationery', 9, 9),
(20, 'Beauty Products', 10, 10),
(21, 'Home Decor', 1, 1),
(22, 'Pet Supplies', 2, 2),
(23, 'Jewelry', 3, 3),
(24, 'Gardening Tools', 4, 4),
(25, 'Car Accessories', 5, 5);
```

---

## 3. Join Queries

```sql
-- INNER JOIN
SELECT O.ORDER_ID, O.ORDER_TYPE, C.CUSTOMER_NAME
FROM ORDERS O
INNER JOIN CUSTOMERS C ON O.CUSTOMER_ID = C.CUSTOMER_ID;

-- LEFT JOIN
SELECT C.CUSTOMER_NAME, O.ORDER_TYPE
FROM CUSTOMERS C
LEFT JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID;

-- RIGHT JOIN
SELECT C.CUSTOMER_NAME, O.ORDER_TYPE
FROM CUSTOMERS C
RIGHT JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID;

-- CROSS JOIN
SELECT C.CUSTOMER_NAME, O.ORDER_TYPE
FROM CUSTOMERS C
CROSS JOIN ORDERS O;

-- FULL JOIN using UNION
SELECT C.CUSTOMER_NAME, O.ORDER_TYPE
FROM CUSTOMERS C
LEFT JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
UNION
SELECT C.CUSTOMER_NAME, O.ORDER_TYPE
FROM CUSTOMERS C
RIGHT JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID;
```

---

## 4. Views Creation and Usage

```sql
CREATE VIEW view_customer_orders AS
SELECT C.CUSTOMER_NAME, O.ORDER_TYPE
FROM CUSTOMERS C
INNER JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID;

CREATE VIEW view_high_order_customers AS
SELECT CUSTOMER_ID, COUNT(*) AS total_orders
FROM ORDERS
GROUP BY CUSTOMER_ID
HAVING COUNT(*) > 2;

CREATE VIEW view_city_highest_orders AS
SELECT A.CITY, COUNT(O.ORDER_ID) AS order_count
FROM ADDRESSES A
JOIN ORDERS O ON A.ADDRESS_ID = O.ADDRESS_ID
GROUP BY A.CITY
ORDER BY order_count DESC
LIMIT 1;

-- View usage
SELECT * FROM view_customer_orders;
SELECT * FROM view_high_order_customers;
SELECT * FROM view_city_highest_orders;
```
---

## Key Concepts

* **Views** simplify complex queries and improve reusability.
* **Joins** combine related data from multiple tables.
* **Aggregations** allow data summarization and filtering.
* **Data Abstraction** hides complexity and improves readability.

