
#  Basic Sales Summary using SQLite and Python

##  Objective
The goal of this task is to extract basic sales information such as total quantity sold and total revenue from a small SQLite database using Python. The project demonstrates how to connect SQLite with Python, run SQL queries, and visualize the results using a bar chart.

---

##  Tools Used
- **Python** (Jupyter Notebook via Anaconda)
- **SQLite** (accessed via built-in `sqlite3` library)
- **Pandas** (for SQL data to DataFrame conversion)
- **Matplotlib** (for visualization)

---

##  Dataset Description
A small manually created SQLite database named `sales_data.db` containing a single table: `sales`.

### Table: `sales`
| Column Name | Data Type | Description              |
|-------------|-----------|--------------------------|
| id          | INTEGER   | Primary Key (auto ID)    |
| date        | TEXT      | Date of the sale         |
| product     | TEXT      | Product name             |
| quantity    | INTEGER   | Quantity of product sold |
| price       | REAL      | Price per unit           |

---

##  SQL Query Used

```
SELECT product, 
       SUM(quantity) AS total_qty, 
       SUM(quantity * price) AS revenue 
FROM sales 
GROUP BY product;
```

---

##  Full Python Script

```
import sqlite3
import pandas as pd
import matplotlib.pyplot as plt

# STEP 1: Create or connect to database
conn = sqlite3.connect("sales_data.db")
cursor = conn.cursor()

# STEP 2: Create sales table
cursor.execute("""
CREATE TABLE IF NOT EXISTS sales (
    id INTEGER PRIMARY KEY,
    date TEXT,
    product TEXT,
    quantity INTEGER,
    price REAL
)
""")

# STEP 3: Insert sample data
cursor.executemany("""
INSERT INTO sales (date, product, quantity, price) VALUES (?, ?, ?, ?)
""", [
    ('2025-06-01', 'Apple', 10, 2.5),
    ('2025-06-01', 'Banana', 5, 1.0),
    ('2025-06-02', 'Apple', 15, 2.5),
    ('2025-06-02', 'Orange', 8, 3.0),
    ('2025-06-03', 'Banana', 10, 1.0)
])

# STEP 4: Query total quantity and revenue
query = """
SELECT product, 
       SUM(quantity) AS total_qty, 
       SUM(quantity * price) AS revenue 
FROM sales 
GROUP BY product
"""

df = pd.read_sql_query(query, conn)
print(df)

# STEP 5: Plot revenue per product
df.plot(kind='bar', x='product', y='revenue', legend=False)
plt.title("Revenue by Product")
plt.xlabel("Product")
plt.ylabel("Revenue")
plt.tight_layout()
plt.savefig("sales_chart.png")
plt.show()

# Close the connection
conn.close()
```

---

##  Output Summary

### Table Output:

| Product | Total Qty | Revenue |
|---------|-----------|---------|
| Apple   | 25        | 62.5    |
| Banana  | 15        | 15.0    |
| Orange  | 8         | 24.0    |

### Bar Chart:
A bar chart was generated using Matplotlib showing **Revenue by Product**.

 The image is saved as:  
`sales_chart.png`

---

##  Project Files

| File Name                | Description                                      |
|--------------------------|--------------------------------------------------|
| `sales_summary.ipynb`    | Jupyter Notebook with full code                |
| `sales_data.db`          | SQLite database file containing sales table     |
| `sales_chart.png`        | Bar chart image generated from query results    |
| `README.md`              | This project explanation file                   |

---

##  Outcome 
- Used `sqlite3` to create a database and insert data using Python
- Executed SQL queries from within Python using Pandas
- Displayed results in both text and visual form
- Learned how to generate a basic bar chart with Matplotlib

---
