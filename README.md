## GAHDSE252F-001

# Sri Lankan A/L Examination Results Analysis

A Python-based data processing and cleaning pipeline for analyzing Sri Lankan G.C.E. Advanced Level (A/L) examination results dataset containing over 337,000 candidate records.

---

## 📌 Features

- **Automated Data Cleaning:** Standardizes mixed data types, missing values, and missing candidate indicators (`Absent`, `-`, empty spaces).
- **Data Health Check:** Provides column-level metrics including missing count, missing percentage, unique value counts, and data types.
- **Rank & Z-Score Processing:** Safely casts exam metrics (`Zscore`, `district_rank`, `island_rank`) into numeric formats for accurate statistical analysis.

---

## 📊 Dataset Structure

The dataset contains **337,553 rows** and **21 columns**:

| Column Name | Description | Data Type |
| :--- | :--- | :--- |
| `index` | Unique record identifier | `int64` |
| `stream` | Academic Stream (Bio, Maths, Commerce, Arts, Tech, etc.) | `object` |
| `Zscore` | Calculated Z-Score value | `float64` |
| `district_rank` | Candidate's District Rank | `float64` |
| `island_rank` | Candidate's Island Rank | `float64` |
| `al_year` | G.C.E. A/L Examination Year | `int64` |
| `sub1`, `sub2`, `sub3` | Subjects taken by the candidate | `object` |
| `sub1_r`, `sub2_r`, `sub3_r` | Subject Grade/Result obtained | `object` |
| `cgt_r` | Common General Test Result | `object` |
| `ge_r` | General English Result | `object` |
| `syllabus` | Syllabus type (New / Old) | `object` |
| `birth_day`, `birth_month` | Candidate demographic info | `object` |

---

## Project Overview: Academic Data Analysis
This project performs an exploratory data analysis (EDA) on a 2020 academic dataset, focusing on student performance (Zscore). It covers the full ETL (Extract, Transform, Load) pipeline, including essential data cleaning steps like handling missing values and type conversions. Key insights are derived through visualizations:

- Zscore Distribution: Understanding the overall spread of academic performance.
- Gender-based Zscore Comparison: Analyzing performance differences between genders using violin plots.
- Correlation Heatmap: Identifying relationships between numerical features.
This analysis provides a foundational understanding of the academic data and its key performance indicators.

---

## 🛠️ Requirements & Installation

Ensure you have **Python 3.8+** installed along with the following packages:

```bash
pip install pandas numpy
