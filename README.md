# 🛒 SQL Task 4 – E-commerce (Online Retail) Data Analysis  

## 📘 Project Overview  
This project focuses on analyzing an **E-commerce (Online Retail)** dataset using **SQL** and **Python (Google Colab)**.  
We use SQL queries to explore sales trends, customer behavior, and top-performing products.  
The goal is to demonstrate SQL-based analytics using **SQLite** without needing any external installation.

---

## 🎯 Objectives  
- Load a real-world dataset and store it in an SQLite database  
- Run SQL queries directly from Google Colab  
- Analyze total revenue, customer activity, and sales patterns  
- Generate insights and export results  

---

## 🧰 Tools & Technologies  
- **Google Colab** – online notebook to run Python + SQL  
- **SQLite3** – lightweight database engine  
- **Pandas** – to read, clean, and transfer data to SQL  
- **Kaggle Dataset** – “Online Retail”  

---

## 📊 Dataset Details  
| Column | Description |
|:--|:--|
| **InvoiceNo** | Transaction number |
| **StockCode** | Product code |
| **Description** | Product name |
| **Quantity** | Units sold |
| **InvoiceDate** | Date of purchase |
| **UnitPrice** | Price per unit |
| **CustomerID** | Unique customer |
| **Country** | Customer’s country |

---

## 🚀 Step-by-Step Implementation  

### 1️⃣ Upload Dataset in Google Colab
```python
from google.colab import files
uploaded = files.upload()     # Choose "Online Retail.csv"

###2️⃣ Import Required Libraries
import pandas as pd
import sqlite3

###3️⃣ Load Dataset
df = pd.read_csv('Online Retail.csv', encoding='ISO-8859-1')
df.head()

###4️⃣ Create SQLite Database
conn = sqlite3.connect('ecommerce.db')
df.to_sql('ecommerce', conn, index=False, if_exists='replace')
print("✅ Database created and table loaded successfully!")

###5️⃣ Verify Data
pd.read_sql_query("SELECT * FROM ecommerce LIMIT 5;", conn)

### SQL Queries (Run Each Using pd.read_sql_query(query, conn))
🔹 Query 1 – Display first 10 records
SELECT * FROM ecommerce LIMIT 10;

🔹 Query 2 – Total revenue per customer
SELECT CustomerID,
       SUM(Quantity * UnitPrice) AS TotalRevenue
FROM ecommerce
WHERE CustomerID IS NOT NULL
GROUP BY CustomerID
ORDER BY TotalRevenue DESC;

🔹 Query 3 – Top 5 countries by revenue
SELECT Country,
       SUM(Quantity * UnitPrice) AS TotalRevenue
FROM ecommerce
GROUP BY Country
ORDER BY TotalRevenue DESC
LIMIT 5;

🔹 Query 4 – Average order value per customer
SELECT CustomerID,
       AVG(Quantity * UnitPrice) AS AvgOrderValue
FROM ecommerce
WHERE CustomerID IS NOT NULL
GROUP BY CustomerID
ORDER BY AvgOrderValue DESC
LIMIT 10;

🔹 Query 5 – Monthly sales trend
SELECT strftime('%Y-%m', InvoiceDate) AS Month,
       SUM(Quantity * UnitPrice) AS MonthlyRevenue
FROM ecommerce
GROUP BY Month
ORDER BY Month;

🔹 Query 6 – Highest-selling products
SELECT Description,
       SUM(Quantity) AS TotalQuantitySold
FROM ecommerce
GROUP BY Description
ORDER BY TotalQuantitySold DESC
LIMIT 10;

🔹 Query 7 – Top customers by number of purchases
SELECT CustomerID,
       COUNT(DISTINCT InvoiceNo) AS TotalPurchases
FROM ecommerce
WHERE CustomerID IS NOT NULL
GROUP BY CustomerID
ORDER BY TotalPurchases DESC
LIMIT 10;

🔹 Query 8 – Most frequently purchased products per country
SELECT Country,
       Description,
       SUM(Quantity) AS TotalQuantity
FROM ecommerce
GROUP BY Country, Description
ORDER BY Country, TotalQuantity DESC;

🔹 Query 9 – Identify cancelled orders (if InvoiceNo starts with “C”)
SELECT * 
FROM ecommerce
WHERE InvoiceNo LIKE 'C%';

🔹 Query 10 – Total quantity and revenue per product category
SELECT Description,
       SUM(Quantity) AS TotalQuantity,
       SUM(Quantity * UnitPrice) AS TotalRevenue
FROM ecommerce
GROUP BY Description
ORDER BY TotalRevenue DESC
LIMIT 15;

### Exporting Results to CSV
result1 = pd.read_sql_query("SELECT * FROM ecommerce LIMIT 10;", conn)
result2 = pd.read_sql_query("""SELECT CustomerID, SUM(Quantity * UnitPrice) AS TotalRevenue
                               FROM ecommerce WHERE CustomerID IS NOT NULL
                               GROUP BY CustomerID ORDER BY TotalRevenue DESC;""", conn)
result3 = pd.read_sql_query("""SELECT Country, SUM(Quantity * UnitPrice) AS TotalRevenue
                               FROM ecommerce GROUP BY Country ORDER BY TotalRevenue DESC LIMIT 5;""", conn)

result1.to_csv('query1_output.csv', index=False)
result2.to_csv('query2_total_revenue.csv', index=False)
result3.to_csv('query3_top_countries.csv', index=False)
print("✅ Query results saved to CSV!")

### Conclusion

The Online Retail dataset clearly shows how SQL and Python together can uncover business insights.
Using Google Colab and SQLite, we can perform data extraction, aggregation, and visualization seamlessly — no need to install MySQL locally.
This approach is ideal for students, analysts, and beginners working with limited resources.

👨‍💻 Author

Name: Karnakanti Varsha
Task: SQL Task 4 – E-commerce Analysis
Course: Data Analytics
Platform: Google Colab
