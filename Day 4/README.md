Day 4 – Sales ETL Pipeline

Overview

This project focuses on building an ETL pipeline using the Kaggle Sales Dataset.

The work is implemented in Google Colab using Python, Pandas and NumPy.

Dataset

The project uses:

sales.csv

customers.csv

products.csv

ETL Process

Step 1 – Extract

The source sales data is loaded and kept separately from the transformed data.

Step 2 – Transform

The sales data is transformed by:

Standardizing column names to snake_case

Converting OrderDate to datetime

Converting Quantity to numeric

Preparing sales measures such as unit_price, sales_amount and total_price

Removing the redundant salesamount column

Step 3 – Data Quality

The transformed sales data is checked for:

Missing values

Duplicate order_id values

Key cardinality

Data types

Current results:

Check

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

Step 4 – Missing Unit Price Decision

UnitPrice is missing for all 1,000 sales records in the source sales data.

Instead of removing the sales records, the pipeline keeps them because the product data can be used later to provide the corresponding price.

Therefore:

sales.product_id
       ↓
products.product_id
       ↓
product price
       ↓
unit_price
       ↓
quantity × unit_price
       ↓
sales_amount

This decision prevents unnecessary loss of valid transaction records.

Step 5 – Date Dimension

A dim_date table is created for time-based analysis.

It includes:

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

Current result:

Item

Value

Rows

609

Columns

12

Primary Key

date_id

Date Range

2023-01-01 to 2025-06-28

Status

SUCCESS

Data Warehousing Design

The project follows a Star Schema approach:

               dim_date
                   |
                   |
dim_customer — fact_sales — dim_product

Fact Table

fact_sales

Main fields:

order_id

customer_key

product_key

date_key

quantity

unit_price

sales_amount

total_price

Dimension Tables

dim_customer

Stores customer-related information and uses customer_key.

dim_product

Stores product-related information and uses product_key.

dim_date

Stores calendar and time-related information and uses date_id.

Transformation Decisions

Valid sales records are retained even when UnitPrice is missing.

Missing price values are intended to be enriched from the product dataset.

sales_amount is calculated using quantity and unit price after enrichment.

Source columns are standardized to consistent names.

Critical transaction fields are validated before records are rejected.

A separate date dimension is used for time-based analysis.

Surrogate keys are used for warehouse dimensions.

Technologies

Python

Pandas

NumPy

Google Colab

Jupyter Notebook

Kaggle Dataset

Project Files

Day 4/

├── Morning/

│   ├── Data Set/

│   │   ├── customers.csv

│   │   ├── products.csv

│   │   └── sales.csv

│   ├── Day_4_Multiple_ETL_Pipeline.ipynb

│   ├── ETL_PROGRESS_REPORT_.pdf

│   └── The project currently includes ETL.txt

└── README.md

Main Notebook

Morning/Day_4_Multiple_ETL_Pipeline.ipynb

The notebook contains the ETL implementation, data-quality checks, transformation logic and date-dimension modelling completed for Day 4.
