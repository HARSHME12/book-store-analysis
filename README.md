# 📚 Book Store Analysis - Full Python Implementation
# Author: Harsh Kumar Verma
# Location: Agra, India
# Tech Stack: Python, PostgreSQL, CSV

import psycopg2
import pandas as pd

# ----------------------------
# 🗄️ Database Connection
# ----------------------------
def create_connection(db_name="postgres", user="postgres", password="password", host="localhost", port="5432"):
    try:
        conn = psycopg2.connect(
            dbname=db_name,
            user=user,
            password=password,
            host=host,
            port=port
        )
        conn.autocommit = True
        print(f"Connected to database: {db_name}")
        return conn
    except Exception as e:
        print(f"Error: {e}")
        return None

# ----------------------------
# 🔹 Create Database
# ----------------------------
def create_database():
    conn = create_connection()
    cur = conn.cursor()
    cur.execute("DROP DATABASE IF EXISTS BookstoreAnalysis;")
    cur.execute("CREATE DATABASE BookstoreAnalysis;")
    cur.close()
    conn.close()
    print("Database BookstoreAnalysis created successfully!")

# ----------------------------
# 🔹 Create Tables
# ----------------------------
def create_tables():
    conn = create_connection("BookstoreAnalysis")
    cur = conn.cursor()
    
    # Books Table
    cur.execute("DROP TABLE IF EXISTS Books;")
    cur.execute("""
        CREATE TABLE Books (
            Book_ID SERIAL PRIMARY KEY,
            Title VARCHAR(100),
            Author VARCHAR(100),
            Genre VARCHAR(50),
            Published_Year INT,
            Price NUMERIC(10,2),
            Stock INT
        );
    """)
    
    # Customers Table
    cur.execute("DROP TABLE IF EXISTS Customers;")
    cur.execute("""
        CREATE TABLE Customers (
            Customer_ID SERIAL PRIMARY KEY,
            Name VARCHAR(100),
            Email VARCHAR(100),
            Phone VARCHAR(15),
            City VARCHAR(50),
            Country VARCHAR(150)
        );
    """)
    
    # Orders Table
    cur.execute("DROP TABLE IF EXISTS Orders;")
    cur.execute("""
        CREATE TABLE Orders (
            Order_ID INT PRIMARY KEY,
            Customer_ID INT REFERENCES Customers(Customer_ID),
            Book_ID INT REFERENCES Books(Book_ID),
            Order_Date DATE,
            Quantity INT,
            Total_Amount NUMERIC(10,2)
        );
    """)
    
    conn.commit()
    cur.close()
    conn.close()
    print("Tables created successfully!")

# ----------------------------
# 🔹 Import Data from CSV
# ----------------------------
def import_csv_to_db():
    conn = create_connection("BookstoreAnalysis")
    
    books = pd.read_csv('C:/sqlproject/Books.csv')
    customers = pd.read_csv('C:/sqlproject/Customers.csv')
    orders = pd.read_csv('C:/sqlproject/Orders.csv')
    
    books.to_sql('Books', conn, if_exists='replace', index=False, method='multi')
    customers.to_sql('Customers', conn, if_exists='replace', index=False, method='multi')
    orders.to_sql('Orders', conn, if_exists='replace', index=False, method='multi')
    
    print("CSV data imported successfully!")
    conn.close()

# ----------------------------
# 📊 Basic SQL Queries
# ----------------------------
def basic_queries():
    conn = create_connection("BookstoreAnalysis")
    cur = conn.cursor()
    
    queries = {
        "Fiction Books": "SELECT * FROM Books WHERE genre='Fiction';",
        "Books After 1950": "SELECT * FROM Books WHERE published_year>1950;",
        "Customers from Canada": "SELECT * FROM Customers WHERE country='Canada';",
        "Orders in Nov 2023": "SELECT * FROM Orders WHERE order_date BETWEEN '2023-11-01' AND '2023-12-01';",
        "Total Stock": "SELECT SUM(stock) AS total_stock FROM Books;",
        "Most Expensive Book": "SELECT * FROM Books ORDER BY price DESC LIMIT 1;",
        "Customers Ordered >1 Quantity": "SELECT * FROM Orders WHERE quantity>1;",
        "Orders > $20": "SELECT * FROM Orders WHERE total_amount>20;",
        "Distinct Genres": "SELECT DISTINCT genre FROM Books;",
        "Books with Lowest Stock": "SELECT * FROM Books ORDER BY stock LIMIT 5;",
        "Total Revenue": "SELECT SUM(total_amount) AS total_revenue FROM Orders;"
    }
    
    for name, query in queries.items():
        cur.execute(query)
        result = cur.fetchall()
        print(f"\n{name}:\n", result)
    
    cur.close()
    conn.close()

# ----------------------------
# ⚙️ Advanced SQL Queries
# ----------------------------
def advanced_queries():
    conn = create_connection("BookstoreAnalysis")
    cur = conn.cursor()
    
    queries = {
        "Books Sold by Genre": """
            SELECT b.genre, SUM(o.quantity) AS total_books_sold
            FROM Orders o
            JOIN Books b ON o.book_id=b.book_id
            GROUP BY b.genre;
        """,
        "Average Price of Fantasy": "SELECT AVG(price) AS avg_price FROM Books WHERE genre='Fantasy';",
        "Customers with >2 Orders": """
            SELECT customer_id, COUNT(order_id) AS order_count
            FROM Orders
            GROUP BY customer_id
            HAVING COUNT(order_id)>2
            ORDER BY order_count;
        """,
        "Most Frequently Ordered Book": """
            SELECT book_id, COUNT(order_id) AS order_count
            FROM Orders
            GROUP BY book_id
            ORDER BY order_count DESC
            LIMIT 1;
        """,
        "Top 3 Expensive Fantasy Books": """
            SELECT * FROM Books WHERE genre='Fantasy' ORDER BY price DESC LIMIT 3;
        """,
        "Total Quantity Sold by Author": """
            SELECT b.author, SUM(o.quantity) AS total_books_sold
            FROM Orders o
            JOIN Books b ON b.book_id=o.book_id
            GROUP BY b.author;
        """,
        "Cities with Spending >$30": """
            SELECT DISTINCT c.city, o.total_amount
            FROM Orders o
            JOIN Customers c ON o.customer_id=c.customer_id
            WHERE o.total_amount>30;
        """,
        "Customer Who Spent Most": """
            SELECT c.customer_id, c.name, SUM(o.total_amount) AS total_spent
            FROM Orders o
            JOIN Customers c ON o.customer_id=c.customer_id
            GROUP BY c.customer_id, c.name
            ORDER BY total_spent DESC
            LIMIT 1;
        """,
        "Remaining Stock After Orders": """
            SELECT b.book_id, b.stock, COALESCE(SUM(o.quantity),0) AS order_quantity,
            b.stock - COALESCE(SUM(o.quantity),0) AS remaining_qty
            FROM Books b
            LEFT JOIN Orders o ON b.book_id=o.book_id
            GROUP BY b.book_id
            ORDER BY remaining_qty;
        """
    }
    
    for name, query in queries.items():
        cur.execute(query)
        result = cur.fetchall()
        print(f"\n{name}:\n", result)
    
    cur.close()
    conn.close()

# ----------------------------
# 📈 Reports Generation
# ----------------------------
def generate_reports():
    print("\n💰 Total Revenue, Top Customers, Book Performance, Inventory & Stock Analysis")
    basic_queries()
    advanced_queries()
    print("\nReports generated successfully!")

# ----------------------------
# Main Execution
# ----------------------------
if __name__ == "__main__":
    # Step 1: Create Database
    create_database()
    
    # Step 2: Create Tables
    create_tables()
    
    # Step 3: Import Data from CSV
    # import_csv_to_db()  # Uncomment if using pandas CSV import
    
    # Step 4: Run Basic Queries
    basic_queries()
    
    # Step 5: Run Advanced Queries
    advanced_queries()
    
    # Step 6: Generate Reports
    generate_reports()
    
    print("\n📌 Book Store Analysis Completed Successfully!")
