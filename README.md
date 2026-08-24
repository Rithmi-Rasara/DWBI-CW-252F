# GAHDSE252F-001
# Sri Lankan A/L Examination Results Analysis

A Python-based data processing and cleaning pipeline for analyzing Sri Lankan G.C.E. Advanced Level (A/L) examination results dataset containing over 337,000 candidate records.

---

##  Features

- **Automated Data Cleaning:** Standardizes mixed data types, missing values, and missing candidate indicators (`Absent`, `-`, empty spaces).
- **Data Health Check:** Provides column-level metrics including missing count, missing percentage, unique value counts, and data types.
- **Rank & Z-Score Processing:** Safely casts exam metrics (`Zscore`, `district_rank`, `island_rank`) into numeric formats for accurate statistical analysis.
- **Stream-wise Aggregation:** Groups candidates by academic stream and summarizes average Z-Score performance.
- **Data Visualization:** Generates bar charts, distribution plots, and comparison visuals to communicate insights clearly.

---

##  Dataset Structure

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

This project performs an exploratory data analysis (EDA) on a 2020 academic dataset, focusing on student performance (Zscore). It covers the full ETL (Extract, Transform, Load) pipeline, including essential data cleaning steps like handling missing values and type conversions.

Key insights are derived through visualizations:

- **Zscore Distribution:** Understanding the overall spread of academic performance.
- **Gender-based Zscore Comparison:** Analyzing performance differences between genders using violin plots.
- **Correlation Heatmap:** Identifying relationships between numerical features.
- **Stream-wise Average Zscore:** Comparing average performance across academic streams (Bio, Maths, Commerce, Arts, Tech).

This analysis provides a foundational understanding of the academic data and its key performance indicators.

---

##  Requirements & Installation

Ensure you have **Python 3.8+** installed along with the following packages:

```bash
pip install pandas numpy matplotlib seaborn
```

Or install everything at once using a requirements file:

```bash
pip install -r requirements.txt
```

**requirements.txt**
```
pandas>=1.5.0
numpy>=1.23.0
matplotlib>=3.6.0
seaborn>=0.12.0
```

---

##  Project Structure

```
├── 2020_al_data_kaggle_upload_new_old_syllabi.csv   # Raw input dataset
├── al_analysis.py                                    # Main ETL + analysis script
├── final_analytics.csv                                # Processed output (stream-wise avg Zscore)
└── README.md                                          # Project documentation
```

---

##  Usage

1. Place the dataset CSV file in your working directory (or update the file path in the script).
2. Run the script:

```bash
python al_analysis.py
```

3. The script will:
   - Load and preview the dataset
   - Clean missing values and standardize data types
   - Convert `Zscore`, `district_rank`, and `island_rank` to numeric
   - Group data by `stream` and compute average `Zscore`
   - Export the summarized results to `final_analytics.csv`
   - Display a bar chart of average Zscore by stream

---

##  ETL Pipeline Overview

**1. Extract**
Reads the raw CSV file into a pandas DataFrame and previews its structure.

**2. Transform**
- Removes rows with missing values.
- Converts `Zscore` to numeric, coercing invalid entries to `NaN`.
- Prepares clean, analysis-ready data grouped by academic stream.

**3. Load / Analytics**
- Aggregates average `Zscore` per stream.
- Exports the summary table to `final_analytics.csv`.

**4. Visualization**
- Bar chart: Average Zscore by Stream
- Violin plot: Gender-based Zscore comparison
- Heatmap: Correlation between numerical features

---

##  Output

- **`final_analytics.csv`** — Average Zscore grouped by academic stream
- **Inline visualizations** — Bar chart, violin plot, and correlation heatmap (displayed via `plt.show()`; add `plt.savefig()` calls if you want them saved as image files)

---

## 📄 License

This project is for academic/educational purposes as part of coursework (GAHDSE252F-001).
