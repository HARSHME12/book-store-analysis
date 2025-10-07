# 📚 Book Store Analysis

## 🧩 Project Overview
This project illustrates SQL database design and analysis for an Online Bookstore.
It covers:
• Database creation
• Table configuration
• Data import from CSV files
• Execution of simple & complex SQL queries for Business Intelligence (BI)

## ________________________________________

## 🗄️ SQL Queries of BookstoreAnalysis

### 🔹 Create Database
CREATE DATABASE BookstoreAnalysis;

### Switch to the Database
\c BookstoreAnalysis;

## ________________________________________

### 🔹 Create Tables

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

## ________________________________________

### 🔹 Import Data into Books Table
COPY Books(book_id, title, author, genre, published_year, price, stock)
FROM 'C:\\sqlproject\\Books.csv'
CSV HEADER;

### 🔹 Import Data into Customers Table
COPY Customers(customer_id, name, email, phone, city, country)
FROM 'C:\\sqlproject\\Customers.csv'
CSV HEADER;

### 🔹 Import Data into Orders Table
COPY Orders(order_id, customer_id, book_id, order_date, quantity, total_amount)
FROM 'C:\\sqlproject\\Orders.csv'
CSV HEADER;

## ________________________________________

## 📊 Basic SQL Queries

### 1️⃣ Retrieve all books in the 'Fiction' genre.
        SELECT * FROM Books
        WHERE genre = 'Fiction';

### 2️⃣ Find the Books published after the year 1950.
        SELECT * FROM Books
        WHERE published_year > 1950;

### 3️⃣ List all the 'Customers' from Canada.
        SELECT * FROM Customers
        WHERE country = 'Canada';

### 4️⃣ Show Orders placed in November 2023.
        SELECT * FROM Orders
        WHERE order_date BETWEEN '2023-11-01' AND '2023-12-01';

### 5️⃣ Retrieve the total stock of books are available.
        SELECT 
           SUM (stock) AS total_stock
        FROM Books;
        
### 6️⃣ Find the details of the most expensive book.
        SELECT * FROM Books
        ORDER BY price DESC
        LIMIT 1;
        
### 7️⃣ Show all Customers who ordered more than 1 quantity of a book.
        SELECT * FROM Orders
        WHERE quantity > 1;
        
### 8️⃣ Retrieve all Orders where the total amount exceeds $20.
        SELECT * FROM Orders
        WHERE total_amount > 20;
        
### 9️⃣ List all genres available in the Books table.
        SELECT DISTINCT genre
        FROM Books;
        
### 🔟 Find the book with the lowest stock.
        SELECT * FROM Books
        ORDER BY stock LIMIT 5;
        
### 1️⃣ Calculate the total revenue generated from all orders
        SELECT
            SUM (total_amount) AS total_revenue
        FROM Orders;

## ________________________________________

## ⚙️ Advanced SQL Queries

### 1️⃣ Retrieve the total number of books sold for each genre.
        SELECT
           b.genre,
	         SUM (o.quantity) AS total_books_sold
        FROM Orders o
        JOIN Books b ON o.book_id = b.book_id
        GROUP BY b.genre;
      
### 2️⃣ Find the average price of books in the "Fantasy" genre.
        SELECT
        AVG(price) AS average_price
        FROM Books
        WHERE genre = 'Fantasy';
        
### 3️⃣ List customers who have placed more than 2 orders.
        SELECT
           customer_id,
	         COUNT(order_id) AS order_count
        FROM Orders
        GROUP BY customer_id
        HAVING COUNT(order_id) > 2
        ORDER BY order_count;
        
### 4️⃣ Find the most frequently ordered book:
        SELECT
           book_id,
	         COUNT(order_id) AS order_count
        FROM Orders
        GROUP BY book_id
        ORDER BY order_count DESC
        LIMIT 1;
      
### 5️⃣ Show the top 3 most expensive books of 'Fantasy' Genre.
        SELECT *
        FROM Books
        WHERE genre = 'Fantasy'
        ORDER BY price DESC
        LIMIT 3;
        
### 6️⃣ Retrieve the total quantity of books sold by each author.
        SELECT
            b.author,
	          SUM(o.quantity) AS total_books_sold
        FROM orders o
        JOIN books b ON b.book_id = o.book_id
        GROUP BY b.author;

### 7️⃣ List the cities where customers who spent over $30 are located.
        SELECT DISTINCT
                    c.city,
	                  total_amount
        FROM orders o
        JOIN customers c ON o.customer_id = c.customer_id
        WHERE o.total_amount > 30;

### 8️⃣ Find the customer who spent the most on orders.
        SELECT
             c.customer_id,
	           c.name,
	           SUM(o.total_amount) AS total_spent
             FROM Orders o
        JOIN Customers c ON o.customer_id = c.customer_id
        GROUP BY c.customer_id, c.name
        ORDER BY total_spent DESC
        LIMIT 1;
        
### 9️⃣ Calculate the stock remaining after fulfilling all orders.
        SELECT
           b.book_id,
	         b.stock,
	         COALESCE(SUM(o.quantity), 0) AS order_quantity,
	         b.stock - COALESCE(SUM(o.quantity), 0) AS remaining_qty
       FROM Books b
       LEFT JOIN Orders o ON b.book_id = o.book_id
       GROUP BY b.book_id
       ORDER BY remaining_qty;
       
## ________________________________________

## 📈 Reports Generated
  • 💰 Total Revenue Generated
  • 🧍‍♂️ Top Customers & Active Buyers
  • 📊 Customer Insights
  • 📕 Book Performance Report
  • 📦 Inventory & Stock Management
  • 📉 Remaining Stock Analysis

## ________________________________________

## 🧠 Conclusion
This project enhances understanding of:
  • Data Organization
  • Business Reporting
  • Decision-Making in Retail Environment

## ________________________________________

## 🧰 Tech Stack
  Component     Description
  Database      PostgreSQL (pgAdmin4)
  Data Source   CSV Files (Books, Customers, Orders)
  Language      SQL

## ________________________________________

## 👨‍💻 Author
Harsh Kumar Verma
📍 Agra, India
🎓 B.Tech
🧩 Skilled in SQL, Excel
