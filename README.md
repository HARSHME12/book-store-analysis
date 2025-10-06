# 📚 Book Store Analysis

## 🧩 Project Overview
This project illustrates **SQL database design and analysis** for an **Online Bookstore**.  
It includes creating a database, designing tables, importing data from CSV files, and performing both simple and advanced SQL queries for business insights.

---

## 💾 SQL Queries of BookstoreAnalysis

### 🔹 Create Database
```sql
CREATE DATABASE BookstoreAnalysis;


\c BookstoreAnalysis;


DROP TABLE IF EXISTS Books;
CREATE TABLE Books (
    Book_ID SERIAL PRIMARY KEY,
    Title VARCHAR(100),
    Author VARCHAR(100),
    Genre VARCHAR(50),
    Published_Year INT,
    Price NUMERIC(10, 2),
    Stock INT  
);


DROP TABLE IF EXISTS Customers;
CREATE TABLE Customers (
    Customer_ID SERIAL PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100),
    Phone VARCHAR(15),
    City VARCHAR(50),
    Country VARCHAR(150)
);


DROP TABLE IF EXISTS Orders;
CREATE TABLE Orders (
    Order_ID INT PRIMARY KEY,
    Customer_ID INT REFERENCES Customers(Customer_ID),
    Book_ID INT REFERENCES Books(Book_ID),
    Order_Date DATE,
    Quantity INT,
    Total_Amount NUMERIC(10, 2)
);


SELECT * FROM Books;
SELECT * FROM Customers;
SELECT * FROM Orders;


COPY Books(book_id, title, author, genre, published_year, price, stock)
FROM 'C:\sqlproject\Books.csv'
CSV HEADER;


COPY Customers(customer_id, name, email, phone, city, country)
FROM 'C:\sqlproject\Customers.csv'
CSV HEADER;


COPY Orders(order_id, customer_id, book_id, order_date, quantity, total_amount)
FROM 'C:\sqlproject\Orders.csv'
CSV HEADER;


SELECT * FROM Books WHERE genre = 'Fiction';


SELECT * FROM Books WHERE published_year > 1950;


SELECT * FROM Customers WHERE country = 'Canada';


SELECT * FROM Orders
WHERE order_date BETWEEN '2023-11-01' AND '2023-12-01';


SELECT SUM(stock) AS total_stock FROM Books;


SELECT * FROM Books ORDER BY price DESC LIMIT 1;


SELECT * FROM Orders WHERE quantity > 1;


SELECT * FROM Orders WHERE total_amount > 20;


SELECT DISTINCT genre FROM Books;


SELECT * FROM Books ORDER BY stock LIMIT 5;


SELECT SUM(total_amount) AS total_revenue FROM Orders;


SELECT b.genre, SUM(o.quantity) AS total_books_sold
FROM Orders o
JOIN Books b ON o.book_id = b.book_id
GROUP BY b.genre;


SELECT AVG(price) AS average_price
FROM Books
WHERE genre = 'Fantasy';


SELECT customer_id, COUNT(order_id) AS order_count
FROM Orders
GROUP BY customer_id
HAVING COUNT(order_id) > 2
ORDER BY order_count;


SELECT book_id, COUNT(order_id) AS order_count
FROM Orders
GROUP BY book_id
ORDER BY order_count DESC
LIMIT 1;


SELECT * FROM Books
WHERE genre = 'Fantasy'
ORDER BY price DESC
LIMIT 3;


SELECT b.author, SUM(o.quantity) AS total_books_sold
FROM Orders o
JOIN Books b ON b.book_id = o.book_id
GROUP BY b.author;


SELECT DISTINCT c.city, total_amount
FROM Orders o
JOIN Customers c ON o.customer_id = c.customer_id
WHERE o.total_amount > 30;


SELECT c.customer_id, c.name, SUM(o.total_amount) AS total_spent
FROM Orders o
JOIN Customers c ON o.customer_id = c.customer_id
GROUP BY c.customer_id, c.name
ORDER BY total_spent DESC
LIMIT 1;


SELECT b.book_id, b.stock,
       COALESCE(SUM(o.quantity), 0) AS order_quantity,
       b.stock - COALESCE(SUM(o.quantity), 0) AS remaining_qty
FROM Books b
LEFT JOIN Orders o ON b.book_id = o.book_id
GROUP BY b.book_id
ORDER BY remaining_qty;


• Retrieve total revenue generated
• Retrieve top customers and most active buyers
• Retrieve customer insights
• Retrieve book performance
• Retrieve inventory & stock management
• Retrieve remaining stock and inventory analysis


• Proper data organization  
• Efficient business reporting  
• Informed decision-making in a retail environment


Database: PostgreSQL (pgAdmin4)
Data Source: CSV Files (Books, Customers, Orders)
Language: SQL


Harsh Kumar Verma  
Agra, India  
B.Tech in Mechanical Engineering  
Skilled in SQL and Excel
