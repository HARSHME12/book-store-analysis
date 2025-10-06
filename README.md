# 📚 Book Store Analysis

## 🧩 Project Overview
# This project illustrates SQL database design and analysis for an Online Bookstore.
# It covers:
# • Database creation
# • Table configuration
# • Data import from CSV files
# • Execution of simple & complex SQL queries for Business Intelligence (BI)

# ________________________________________

## 🗄️ SQL Queries of BookstoreAnalysis

### 🔹 Create Database
# CREATE DATABASE BookstoreAnalysis;
# \c BookstoreAnalysis;

# ________________________________________

### 🔹 Create Tables
# DROP TABLE IF EXISTS Books;
# CREATE TABLE Books (
#     Book_ID SERIAL PRIMARY KEY,
#     Title VARCHAR(100),
#     Author VARCHAR(100),
#     Genre VARCHAR(50),
#     Published_Year INT,
#     Price NUMERIC(10, 2),
#     Stock INT  
# );

# DROP TABLE IF EXISTS Customers;
# CREATE TABLE Customers (
#     Customer_ID SERIAL PRIMARY KEY,
#     Name VARCHAR(100),
#     Email VARCHAR(100),
#     Phone VARCHAR(15),
#     City VARCHAR(50),
#     Country VARCHAR(150)
# );

# DROP TABLE IF EXISTS Orders;
# CREATE TABLE Orders (
#     Order_ID INT PRIMARY KEY,
#     Customer_ID INT REFERENCES Customers(Customer_ID),
#     Book_ID INT REFERENCES Books(Book_ID),
#     Order_Date DATE,
#     Quantity INT,
#     Total_Amount NUMERIC(10, 2)
# );

# ________________________________________

### 🔹 Import Data from CSV
# COPY Books(book_id, title, author, genre, published_year, price, stock)
# FROM 'C:\\sqlproject\\Books.csv'
# CSV HEADER;

# COPY Customers(customer_id, name, email, phone, city, country)
# FROM 'C:\\sqlproject\\Customers.csv'
# CSV HEADER;

# COPY Orders(order_id, customer_id, book_id, order_date, quantity, total_amount)
# FROM 'C:\\sqlproject\\Orders.csv'
# CSV HEADER;

# ________________________________________

## 📊 Basic SQL Queries
# 1️⃣ Retrieve all books in the Fiction genre
# 2️⃣ Books published after 1950
# 3️⃣ Customers from Canada
# 4️⃣ Orders placed in November 2023
# 5️⃣ Total stock of all books
# 6️⃣ Most expensive book
# 7️⃣ Customers who ordered more than 1 quantity
# 8️⃣ Orders with total amount > $20
# 9️⃣ Distinct genres
# 🔟 Book with lowest stock
# 1️⃣1️⃣ Total revenue generated

# ________________________________________

## ⚙️ Advanced SQL Queries
# 1️⃣ Total number of books sold by each genre
# 2️⃣ Average price of Fantasy books
# 3️⃣ Customers with more than 2 orders
# 4️⃣ Most frequently ordered book
# 5️⃣ Top 3 most expensive Fantasy books
# 6️⃣ Total quantity sold by each author
# 7️⃣ Cities where customers spent over $30
# 8️⃣ Customer who spent the most
# 9️⃣ Stock remaining after fulfilling all orders

# ________________________________________

## 📈 Reports Generated
# • 💰 Total Revenue Generated
# • 🧍‍♂️ Top Customers & Active Buyers
# • 📊 Customer Insights
# • 📕 Book Performance Report
# • 📦 Inventory & Stock Management
# • 📉 Remaining Stock Analysis

# ________________________________________

## 🧠 Conclusion
# This project enhances understanding of:
# • Data Organization
# • Business Reporting
# • Decision-Making in Retail Environment

# ________________________________________

## 🧰 Tech Stack
# Component     Description
# Database      PostgreSQL (pgAdmin4)
# Data Source   CSV Files (Books, Customers, Orders)
# Language      SQL

# ________________________________________

## 👨‍💻 Author
# Harsh Kumar Verma
# 📍 Agra, India
# 🎓 B.Tech
# 🧩 Skilled in SQL, Excel
