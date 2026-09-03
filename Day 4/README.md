📊 Day 4 – Sales Data ETL Pipeline

📌 Overview

This folder contains the Day 4 ETL and Data Warehousing work completed using the Kaggle Sales Dataset.

The project demonstrates how raw sales data can be transformed into structured, analytics-ready data through:

Extract → Transform → Data Quality → Integration → Data Warehousing → Analytics → Visualization → Load → Validation

The main implementation is provided in the Google Colab/Jupyter notebook:

Day_4_Multiple_ETL_Pipeline.ipynb

📂 Project Contents

Morning/
│
├── Data Set/
│   ├── customers.csv
│   ├── products.csv
│   └── sales.csv
│
├── Day_4_Multiple_ETL_Pipeline.ipynb
├── ETL_PROGRESS_REPORT_.pdf
└── The project currently includes ETL.txt

🎯 Project Objectives

The main objectives of this ETL pipeline are to:

Extract sales, customer, and product data.

Profile and identify data-quality issues.

Clean and standardize the source data.

Handle missing and invalid values appropriately.

Integrate related datasets using business keys.

Build a dimensional data-warehouse structure.

Create analytical sales measures.

Validate the transformed data.

Prepare the data for business analysis and visualization.

🔄 ETL Pipeline

                 RAW DATA
                    │
                    ▼
              1. EXTRACT
                    │
                    ▼
         2. DATA PROFILING
                    │
                    ▼
            3. TRANSFORM
                    │
                    ▼
           4. DATA QUALITY
                    │
                    ▼
          5. DATA INTEGRATION
                    │
                    ▼
       6. DATA WAREHOUSE MODEL
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
     DIMENSIONS           FACT TABLE
          │                   │
          └─────────┬─────────┘
                    ▼
             7. ANALYTICS
                    │
                    ▼
           8. VISUALIZATION
                    │
                    ▼
               9. LOAD
                    │
                    ▼
              10. VALIDATE

1️⃣ Extract

The ETL process begins with the source CSV datasets:

sales.csv

customers.csv

products.csv

The raw data is kept separate from transformed data so that the original source is not directly modified.

2️⃣ Data Profiling & Quality

The sales data is inspected before transformation.

The pipeline checks:

Missing values

Duplicate records

Duplicate order IDs

Key cardinality

Data types

Critical identifiers

Numeric fields

Date fields

Current sales-data findings

Metric

Result

Sales records

1,000

Unique Order IDs

1,000

Unique Customer IDs

638

Unique Product IDs

614

Duplicate Order IDs

0

3️⃣ Transformation

3.1 Column Standardization

The source column names are standardized to snake_case.

Examples:

Source Column

Transformed Column

OrderID

order_id

OrderDate

order_date

CustomerID

customer_id

ProductID

product_id

Quantity

quantity

UnitPrice

unit_price

This creates consistent naming across the ETL pipeline.

3.2 Data Type Transformation

The pipeline converts:

order_date → datetime

quantity → numeric

unit_price → numeric

sales_amount → numeric

total_price → numeric

Invalid dates or invalid numeric values are handled using controlled conversion rather than silently producing incorrect data.

3.3 Missing Unit Price Decision

An important issue was identified in the source sales data:

UnitPrice is missing for all 1,000 sales records.

Instead of deleting all sales transactions, the pipeline keeps the valid transaction records.

The transformation decision is:

sales.csv
    │
    ├── order_id       ✓
    ├── order_date     ✓
    ├── customer_id    ✓
    ├── product_id     ✓
    ├── quantity       ✓
    └── unit_price     ✗
                         │
                         ▼
                 Enrich from products

Therefore:

unit_price is temporarily missing during the initial sales transformation.

sales_amount is calculated after price enrichment.

total_price is calculated after price enrichment.

Valid sales transactions are preserved.

3.4 Redundant Field Handling

A redundant salesamount field was identified and removed to avoid conflicting representations of the sales measure.

4️⃣ Data Quality Results

After the corrected sales transformation:

Check

Result

Extracted rows

1,000

Transformed rows

1,000

Rejected rows

0

Missing unit_price

1,000

Missing sales_amount

1,000

Missing total_price

1,000

The financial missing values are considered an intermediate ETL condition, because the product dataset is intended to provide the required price during integration.

5️⃣ Data Integration

The ETL pipeline integrates the related datasets using their business keys.

Main relationships

sales ───────► customers
   │
   └──────────► products
   │
   └──────────► date dimension

Product integration

The product data is used to enrich the missing sales price.

Conceptually:

sales.product_id
       │
       ▼
products.product_id
       │
       ▼
products.price
       │
       ▼
sales.unit_price
       │
       ▼
quantity × unit_price
       │
       ▼
sales_amount

Before joining, key uniqueness and unmatched records are checked to avoid unexpected row multiplication.

6️⃣ Data Warehousing

The project follows a Star Schema design.

                    ┌─────────────────┐
                    │   dim_date      │
                    └────────┬────────┘
                             │
                             │
┌─────────────────┐     ┌────▼───────┐     ┌─────────────────┐
│  dim_customer   │────►│ fact_sales │◄────│   dim_product   │
└─────────────────┘     └────────────┘     └─────────────────┘

6.1 Fact Table – fact_sales

The fact table represents sales transactions and contains measurable business information.

Main fields include:

order_id

customer_key

product_key

date_key

quantity

unit_price

sales_amount

total_price

The fact table uses foreign keys to connect sales transactions to the dimensions.

6.2 Customer Dimension – dim_customer

The customer dimension stores customer-related descriptive information.

It uses:

customer_id → source/business key

customer_key → warehouse surrogate key

The surrogate key is used for warehouse relationships.

6.3 Product Dimension – dim_product

The product dimension stores product-related descriptive information.

It uses:

product_id → source/business key

product_key → warehouse surrogate key

The product dimension is also important for resolving the missing unit price in the original sales data.

6.4 Date Dimension – dim_date

The date dimension provides reusable calendar attributes for time-based analysis.

The current date dimension contains:

Attribute

Value

Rows

609

Columns

12

Primary Key

date_id

Date Start

2023-01-01

Date End

2025-06-28

Main date attributes include:

date_id

full_date

day_of_week

day_of_month

day_of_year

week_of_year

month

month_num

quarter

year

is_weekend

is_holiday

season

7️⃣ Analytics

The final analytics stage prepares the data for business reporting.

The notebook prepares analysis such as:

Total Sales

Total Quantity

Total Orders

Unique Customers

Unique Products

Average Order Value

Sales by Product

Sales by Customer

Monthly Sales

Yearly Sales

Top Products

Top Customers

Time-related analytical fields include:

sales_year

sales_month

sales_quarter

order_total

8️⃣ Visualization

The pipeline supports visual analysis through charts such as:

📈 Monthly Sales Trend

Shows changes in sales over time.

🏆 Top 10 Products

Identifies products contributing the highest sales.

👥 Top 10 Customers

Identifies customers with the highest sales contribution.

📊 Sales Distribution

Shows the distribution of transaction sales amounts.

📦 Quantity Distribution

Shows the distribution of quantities sold.

9️⃣ Load

The processed outputs are designed to be stored in:

/content/etl_output/

Expected output datasets include:

cleaned_sales.csv
cleaned_customers.csv
cleaned_products.csv
dim_customer.csv
dim_product.csv
dim_date.csv
fact_sales.csv
final_sales_analytics.csv

Validation/report files can also be generated as part of the ETL process.

🔟 Validation

The final validation stage checks:

Sales row counts

Fact-table row counts

Customer foreign keys

Product foreign keys

Date foreign keys

Missing sales amounts

Overall ETL consistency

The objective is to make sure transformations and integrations do not create unexpected data loss or duplication.

🧠 Key Transformation & Warehouse Decisions

Transformation Decisions

Standardize source fields to snake_case.

Convert dates to datetime.

Convert quantity and measure fields to numeric types.

Preserve valid sales transactions when UnitPrice is missing.

Enrich missing price values from the product dataset.

Calculate sales_amount only after obtaining a valid price.

Remove redundant/conflicting sales fields.

Validate business keys before joins.

Data-Warehouse Decisions

Use a Star Schema.

Use fact_sales as the central fact table.

Use dim_customer for customer attributes.

Use dim_product for product attributes.

Use dim_date for time-based analysis.

Use surrogate keys for warehouse dimensions.

Keep business/source IDs for traceability.

Avoid unnecessary descriptive duplication in the fact table.

🛠️ Technologies Used

🐍 Python

🐼 Pandas

🔢 NumPy

📊 Matplotlib

☁️ Google Colab

📓 Jupyter Notebook

📦 Kaggle Dataset

🗂️ Git & GitHub

📁 Repository Structure

Day 4/
│
├── Morning/
│   ├── Data Set/
│   │   ├── customers.csv
│   │   ├── products.csv
│   │   └── sales.csv
│   │
│   ├── Day_4_Multiple_ETL_Pipeline.ipynb
│   ├── ETL_PROGRESS_REPORT_.pdf
│   └── The project currently includes ETL.txt
│
└── README.md

🚀 How to Run

Open Day_4_Multiple_ETL_Pipeline.ipynb in Google Colab.

Upload or provide the required CSV datasets.

Run the notebook cells in sequential order.

Review the data-quality reports.

Execute the transformation and integration stages.

Review the Star Schema outputs.

Run analytics and visualizations.

Review the final validation results.

Check the generated ETL outputs.

📌 Current Project Status

Completed / Implemented

✅ Sales data transformation

✅ Column standardization

✅ Date conversion

✅ Numeric conversion

✅ Data-quality profiling

✅ Duplicate Order ID validation

✅ Cardinality analysis

✅ Date dimension creation

✅ Star Schema design

✅ Product price enrichment logic

✅ Customer integration logic

✅ Date integration logic

✅ Fact-table construction logic

✅ Analytics logic

✅ Visualization logic

✅ Load/export logic

✅ Validation logic

Notes

The pipeline was designed to handle the missing UnitPrice issue without discarding the 1,000 valid sales transactions. The price is intended to be obtained through the related product data during integration.

📚 Documentation

Detailed ETL decisions and project progress are documented in:

ETL_PROGRESS_REPORT_.pdf

The main implementation and executable workflow are documented in:

Day_4_Multiple_ETL_Pipeline.ipynb

👨‍💻 Project

Sales Data ETL & Data Warehousing – Day 4

This project is developed as a practical Data Engineering / Data Warehousing exercise demonstrating ETL pipeline design, data-quality handling, dimensional modelling, analytics preparation, and GitHub-based project organization.
