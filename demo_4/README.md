# 📊 Adventure Works Sales Analysis – Power BI

## 📌 Project Overview

This project is my fourth Power BI practice lab.

The objective of this lab was to connect to a **SQL Server database (Adventure Works)**, explore the Sales schema tables, clean and prepare the data in Power Query, build relationships manually, create DAX measures, and build a multi-page interactive dashboard to analyze sales orders, territories, and product categories.

---

## 🗂 Data Source

| Property | Details |
|---|---|
| **Source** | SQL Server Database |
| **Database** | Adventure Works |
| **Schema** | Sales |

### Tables Loaded from SQL Server

| Table | Rows | Key Columns |
|---|---|---|
| **Orders** | 999+ | OrderID, OrderDate, ShipDate, DueDate, StatusID, Territory, Group |
| **OrderDetail** | 999+ | DetailID, OrderID, ProductID, OrderQty, UnitPrice, LineTotal |
| **Product** | 504 | ProductID, Product, ProductSubcategoryID |
| **ProductCategory** | 4 | ProductCategoryID, Category |
| **ProductSubcategory** | 37 | ProductSubcategoryID, ProductCategoryID, Subcategory |
| **Territory** | 10 | TerritoryID, Territory, CountryRegionCode, Group |
| **ShipMethod** | 5 | ShipMethodID, ShipMethod |
| **Status** | 6 | StatusID, Status *(custom table — see below)* |
| **ufnGetSalesOrderStatusText** | — | SQL function used to resolve status names |

---

## ⚙️ Data Preparation

Data preparation was performed using **Power Query in Power BI**.

### General Steps Applied to All Tables

- Connected to SQL Server and selected the required Sales schema tables.
- Removed unused columns that belonged to other tables and were not needed for this analysis.
- Renamed columns to cleaner, shorter names (e.g. `SalesOrderID` → `OrderID`, `SalesOrderDetailID` → `DetailID`, `Name` → `Product` / `Category` / `Subcategory` / `Territory`).

### Per-Table Details

**Orders**
- Kept: OrderID, OrderDate, DueDate, ShipDate, StatusID, Territory, Group, and financial columns.
- Removed all other columns not needed for analysis.
- Used the SQL Server function `ufnGetSalesOrderStatusText` to add a Status name column based on StatusID.

**OrderDetail**
- Kept: OrderID, DetailID, ProductID, OrderQty, UnitPrice, UnitPriceDiscount, LineTotal.
- Removed all other columns.

**Product / ProductCategory / ProductSubcategory / Territory / ShipMethod**
- Removed extra columns and kept only the ID and name columns needed for relationships and labels.

### Custom Status Table

The **Orders** table stores order status as a number (1 to 6), but the names come from a SQL Server function (`ufnGetSalesOrderStatusText`), not a lookup table.

To solve this, a custom **Status** table was created manually in Power Query:

| StatusID | Status |
|---|---|
| 1 | In process |
| 2 | Approved |
| 3 | Backordered |
| 4 | Rejected |
| 5 | Shipped |
| 6 | Cancelled |

---

## 🔗 Data Model & Relationships

The relationships between tables were managed manually in Power BI.

- **Auto-detect relationships** was disabled to have full control over the model.
- Relationships were created manually between the following tables:

| From Table | Key | To Table | Key |
|---|---|---|---|
| Orders | StatusID | Status | StatusID |
| Orders | TerritoryID | Territory | TerritoryID |
| Orders | ShipMethodID | ShipMethod | ShipMethodID |
| OrderDetail | OrderID | Orders | OrderID |
| OrderDetail | ProductID | Product | ProductID |
| Product | ProductSubcategoryID | ProductSubcategory | ProductSubcategoryID |
| ProductSubcategory | ProductCategoryID | ProductCategory | ProductCategoryID |

### Data Model Diagram

![Data Model & Relationships](![alt text](image.png))

---

## 📐 DAX Measures

A dedicated **Dax Measures** table was created to keep all measures organized and separated from the data tables.

Measures were organized into two display folders:

### Amount Folder
| Measure | Description |
|---|---|
| # SubTotal Amount | Sum of SubTotal from Orders |
| # Total Due | Sum of TotalDue from Orders |
| # Total Freight | Sum of Freight from Orders |
| # Total Tax | Sum of TaxAmt from Orders |

### Count Folder
| Measure | Description |
|---|---|
| # Orders | Count of distinct OrderID |
| # Order Details | Count of rows in OrderDetail |

---

## 📊 Dashboard

The report contains **3 pages**:

---

### Page 1 – Sales (Main Dashboard)

Copied the base layout from Demo #003 (Lesson #12) and updated it for Adventure Works data.

**KPI Cards:**

| KPI | Value |
|---|---|
| Orders | 31K |
| ORDER Details | 40K |
| Total Amount | $109.85M |
| Total Tax | $10.19M |
| Total Freight | $3.18M |
| Total Due | $123.22M |

**Charts:**

- **Order by Year** — Bar/line chart showing order count trend from 2011 to 2014.
- **Order by Status** — Pie chart showing distribution by status (Shipped, Approved, In Process, etc.).
- **Order by Territory** — Horizontal bar chart ranking territories: Australia, Southwest, Northwest, Canada, United Kingdom, France, Germany, Southeast, Central, Northeast.
- **Order by Product Category** — Treemap showing Road Bikes, Mountain Bikes, Touring Bikes.

---

### Page 2 – Orders (Details Page)

A detailed drill-through page showing individual orders with filters.

**Slicers / Filters:**
- SubTotal range slider
- OrderDate range slider
- Status filter buttons (Approved, Backordered, Cancelled, In process, Rejected, Shipped)

**Table Columns:**
OrderID, OrderDate, ShipDate, DueDate, Status, Group, Territory, SubTotal Amount, Total Tax, Total Freight, Total Due, Count of DetailID

---

### Page 3 – Order Details (Details Page)

A drill-through page showing product-level order details.

**Slicer:** Territory

**Table Columns:**
DetailID, OrderID, Category, Subcategory, Product, Sum of OrderQty, Sum of UnitPrice, Sum of LineTotal

---

## 🛠 Skills Practiced

- Connecting Power BI to a SQL Server database
- Selecting and loading tables from a specific schema
- Removing unused columns from SQL Server tables
- Renaming columns and tables for clarity
- Loading a SQL Server function (`ufnGetSalesOrderStatusText`) into Power Query
- Creating a custom lookup table manually (Status table)
- Disabling auto-detect relationships
- Building relationships manually in the model view
- Creating DAX measures
- Creating a dedicated DAX Measures table
- Organizing measures into display folders (Amount, Count)
- Copying and reusing a dashboard layout from a previous demo
- Adding drill-through pages in Power BI
- Using drill-through with single and multiple filters
- Adding Slicer visuals for interactive filtering
- Applying filters at Visual, Page, and All Pages levels
- Changing date column format to a specific display format

---

## 🛠 Tools Used

- Power BI Desktop
- Power Query (M Language)
- DAX
- SQL Server (Adventure Works Database)

---

## 📷 Dashboard Preview

**Sales Page**
![Sales Dashboard](![alt text](image-1.png))

**Orders Page**
![Orders Page](![alt text](image-2.png))

**Order Details Page**
![Order Details Page](![alt text](image-3.png))

---

## 📚 Key Learnings

Through this project I learned:

- How to connect Power BI to a SQL Server database and explore schema tables
- How to remove columns that come from SQL Server but are not needed in the report
- How to load and use a SQL Server function in Power Query
- Why auto-relationships can cause issues and how to disable them
- How to manually build a data model with correct relationships
- How to create a Status lookup table when SQL Server stores only numeric IDs
- How to create a separate measures table and organize measures with folders
- How to copy a dashboard from a previous report and adapt it for new data
- How to add drill-through pages and configure them with single and multiple filters
- How to use slicers for interactive multi-value filtering
- How to change the date column display format in Power BI

---

## 🎯 Key Insights

- The highest order activity was in **2013**
- The majority of orders have a status of **Shipped**
- **Australia** and **Southwest** lead in order count by territory
- **Road Bikes** and **Mountain Bikes** dominate product category sales

---

## 👤 Author

**Salah Eddine**
Power BI / Data Analytics Practice Projects
