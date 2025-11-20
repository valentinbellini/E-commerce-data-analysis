# E-commerce Sales Analysis Portfolio Project

## Project Overview

This project simulates a real-world data analysis pipeline, starting from raw data acquisition and cleaning through  SQL querying and final preparation for Business Intelligence (BI) visualization.

**Goal:** Identify key sales drivers, product performance (by revenue and volume), and customer behavior patterns from a UK-based e-commerce transactional dataset. (https://www.kaggle.com/datasets/carrie1/ecommerce-data?resource=download)

**Key Tools:** Python (Pandas), SQL (SQLite), Matplotlib/Seaborn, Git/GitHub.

---

## Project Structure and Workflow

The project is segmented into two dedicated Jupyter Notebooks to ensure a clear separation of concerns:

| Notebook | Primary Function | Core Technology | Output |
| :--- | :--- | :--- | :--- |
| **`DataCleaningAndTransformation.ipynb`** | Data Quality Assurance, Feature Engineering, Outlier Handling (IQR). | **Python (Pandas, NumPy, Seaborn)** | Clean DataFrame saved to SQLite. |
| **`SQL-Queries.ipynb`** | Executing business-driven aggregate queries and analytical reporting. | **SQL (via SQLite connection)** | Key result tables (e.g., Top Revenue). |

---

## Phases and Methodology

### Phase 1 & 2: Data Cleaning and Transformation

* **Initial EDA:** Identified significant data quality issues (missing `CustomerID`, negative `Quantity`, incorrect data types).
* **Data Cleaning:**
    * Dropped rows with missing `CustomerID` to focus on identifiable customers.
    * Converted `InvoiceDate` to `datetime` and `CustomerID` to `int`.
    * Filtered out rows with non-positive `Quantity` or `UnitPrice` (returns/errors).
* **Outlier Handling:** Used the **Interquartile Range (IQR)** method to detect and cap extreme outliers in the `UnitPrice` column, ensuring analysis integrity.
* **Feature Engineering:** Created the core metric `TotalSale` (`Quantity` * `UnitPrice`) and extracted time features (`Year`, `Month`, `Day`, `Hour`).

### Phase 3: SQL Analysis and Persistence

* **Persistence:** The cleaned data was loaded from Pandas into a local **SQLite** database table named `Sales`.
* **Key Query (Top Revenue):** A SQL query was executed to find the top 10 products based on total revenue, combining aggregates for a comprehensive view:

```sql
SELECT
    Description,
    SUM(TotalVenta) AS TotalRecaudado,
    SUM(Quantity) AS TotalCantidadVendida,
    AVG(UnitPrice) AS PrecioPromedio
FROM
    Ventas
GROUP BY
    Description
ORDER BY
    TotalRecaudado DESC
LIMIT 10;
```