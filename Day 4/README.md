# Day 4 - Sales ETL Pipeline

This project contains the Day 4 work for a Sales Data ETL Pipeline developed using Google Colab, Python, Pandas and NumPy.

## Overview

The project focuses on extracting, cleaning, transforming, integrating and preparing sales data for data warehousing and business analysis.

## Dataset

The project uses the following datasets:

- `sales.csv`
- `customers.csv`
- `products.csv`

## ETL Process

### 1. Extract
Raw sales, customer and product datasets are loaded for processing.

### 2. Data Quality
The data is checked for:

- Missing values
- Duplicate records
- Duplicate Order IDs
- Data types
- Key cardinality
- Critical missing fields

### 3. Transform
The sales data is transformed by:

- Standardizing column names
- Converting dates to datetime
- Converting numeric fields
- Removing redundant columns
- Preparing sales-related measures

### 4. Missing Unit Price Handling

The source sales data contains missing `UnitPrice` values.

Instead of removing valid sales records, the pipeline keeps the records and plans to enrich the missing price information from the product dataset.


sales.product_id

       ↓
products.product_id

       ↓
Product Price

       ↓
Unit Price

       ↓
Quantity × Unit Price

       ↓
Sales Amount
