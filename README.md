# 🚀 Databricks Lakehouse Project – E-Commerce Analytics

## 📌 Project Overview

This project demonstrates an end-to-end Lakehouse data pipeline built using **Databricks** and **Delta Lake** following the **Medallion Architecture (Bronze, Silver, Gold)**.

The objective was to process raw retail transaction data and transform it into business-ready analytical datasets using Spark transformations and Delta Lake features.

---

## 🏗 Architecture

The pipeline follows the Medallion Architecture pattern:

Raw CSV → Bronze Layer → Silver Layer → Gold Layer

### 📊 Architecture Diagram

![Architecture Diagram](architecture.png)

---

## 📂 Dataset

**Online Retail Dataset (Kaggle)**

The dataset contains transactional retail data including:

- InvoiceNo  
- StockCode  
- Description  
- Quantity  
- InvoiceDate  
- UnitPrice  
- CustomerID  
- Country  

### 📊 Dataset Statistics

- Total Raw Records (Bronze): **541,909**
- Cleaned Records (Silver): **397,924**
- Daily Aggregation Days (Gold): **305**

---

## 🥉 Bronze Layer – Raw Ingestion

- Ingested raw CSV data into a **managed Delta table**
- Preserved original schema
- Stored unprocessed transactional data
- Enabled ACID compliance through Delta transaction logs

---

## 🥈 Silver Layer – Data Cleaning & Transformation

Applied real-world data engineering cleaning logic:

- Removed cancelled invoices (InvoiceNo starting with 'C')
- Filtered negative quantities (returns)
- Removed null CustomerID records
- Added derived column:
  

total_amount = Quantity * UnitPrice

- Reduced dataset size by ~26% after cleaning invalid transactions

---

## 🥇 Gold Layer – Business Aggregations

Created analytics-ready datasets:

### 1️⃣ gold_daily_sales
- Aggregated total revenue per day
- Generated 305 days of sales insights

### 2️⃣ gold_top_products
- Calculated revenue by product
- Identified Top 10 revenue-generating products

---

## ⚡ Advanced Delta Lake Features Implemented

### 🔹 OPTIMIZE
Compacted small files to improve query performance and reduce file fragmentation.

### 🔹 ZORDER
Applied Z-Ordering on `CustomerID` to enhance data skipping and filtering performance.

### 🔹 Time Travel
Queried historical table versions using:

```sql
SELECT * FROM silver_ecommerce VERSION AS OF 0;

Demonstrated Delta's version control and ACID capabilities.

🔹 MERGE (Upsert Logic)

Simulated incremental data ingestion using:MERGE INTO silver_ecommerce AS target
USING source
ON target.InvoiceNo = source.InvoiceNo
WHEN MATCHED THEN UPDATE SET *
WHEN NOT MATCHED THEN INSERT *

Handled both insert and update scenarios efficiently.

🛠 Technologies Used

Databricks (Free Edition)

Apache Spark (PySpark)

Spark SQL

Delta Lake

GitHub (Version Control)
