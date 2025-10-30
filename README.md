# 🧠 DSA 2040A – ETL Pipeline Project  
---

## 1️⃣ Project Overview  

This project demonstrates the **ETL (Extract, Transform, Load)** process using a synthetic student exam dataset.  
The main goal was to design a working data pipeline that takes raw exam records and processes them into clean, validated, and enriched data ready for storage or analysis.  

The project is split into two main Jupyter notebooks:
- `etl_extract.ipynb` – Generates and validates raw data  
- `etl_transform.ipynb` – Cleans, enriches, and standardizes the data  

By the end of the workflow, two clean CSV files (`transformed_full.csv` and `transformed_incremental.csv`) are produced — representing ready-to-analyze student performance data.

---

## 2️⃣ Data Source  

**Dataset:** Synthetic student exam records generated using Python’s [`Faker`](https://faker.readthedocs.io/en/master/) library.  

**Description:**  
The dataset simulates real-world student exam data, containing details such as:
- Student demographics (ID, gender, age, region)  
- Academic info (subject, grade level, school)  
- Performance metrics (exam score, date, and pass/fail outcome)  

**Files generated:**  
| File | Description |
|------|--------------|
| `raw_data.csv` | Main dataset containing 8,000 records |
| `incremental_data.csv` | Additional 1,000 records simulating new incoming data |
| `validated_full.csv` | Cleaned + validated merged dataset |
| `validated_incremental.csv` | Cleaned incremental data for testing transformations |

📁 **Location:** `/data` folder inside the main project directory.  

---

## 3️⃣ ET Phases  

### **🔹 Extract Phase**
Notebook: `etl_extract.ipynb`  
**Goal:** Generate, validate, and prepare raw exam data for transformation.  

**Key Steps:**
1. Generated random but realistic data using the `Faker` library.  
2. Created separate `raw` and `incremental` datasets.  
3. Validated data structure — checked column data types, missing values, and duplicates.  
4. Saved validated data into the `/data` folder.  

**Outputs:**
- `validated_full.csv`  
- `validated_incremental.csv`

---

### **🔹 Transform Phase**
Notebook: `etl_transform.ipynb`  
**Goal:** Clean, standardize, and enrich the validated data for analytics.  

**Key Transformations:**
| Step | Transformation | Description |
|------|----------------|-------------|
| 1 | Handle Missing Values | Filled missing regions/schools with "Unknown". |
| 2 | Standardize Text | Converted text fields (gender, subject, region) to Title Case. |
| 3 | Data Type Conversion | Changed `exam_date` to a proper datetime format. |
| 4 | Derived Column | Added `score_status` = Pass/Fail based on score threshold (≥ 50). |
| 5 | Categorization | Binned `exam_score` into Low, Average, Good, Excellent. |
| 6 | Drop Irrelevant Columns | Removed `name` for privacy and minimalism. |

**Outputs:**
- `/transformed/transformed_full.csv`  
- `/transformed/transformed_incremental.csv`

---

## 4️⃣ Tools Used  

| Tool / Library | Purpose |
|----------------|----------|
| **Python 3.13** | Programming language used for data pipeline |
| **Pandas** | Data manipulation and cleaning |
| **NumPy** | Numerical operations |
| **Faker** | Synthetic data generation |
| **Jupyter Notebook** | Interactive environment for coding and documentation |


---

## 5️⃣ Steps to Run the Project  

### **Prerequisites**
Ensure you have the following installed:
- Python 3.10 or later  
- Jupyter Notebook or VS Code with Jupyter extension  
- Required libraries:
  ```bash
  pip install pandas numpy faker matplotlib

<<<<<<< HEAD
## Load & Verification

### Format Used:
SQLite database
![alt text](image.png)

### Description:
The transformed datasets were successfully loaded into a SQLite database stored in the `loaded/` folder as `full_data.db`.  
Two tables were created:
- `full_data`
- `incremental_data`

### Verification:
```python
# Sample verification snippet
pd.read_sql('SELECT * FROM full_data LIMIT 5', conn)

### Issue:
sqlite3.OperationalError: table already exists
###Fix: 
Used if_exists='replace' in to_sql() to overwrite.
=======
---
## 6️⃣ 🧾 Sample Outputs

Below are selected outputs showing the ETL process — from raw extraction to final validation.

---
#### 🧩 1. Extracted Data (Raw Dataset)

After running the **Extract** phase, the `raw_data.csv` file contained over 8,000 rows of student records:

| student_id | name         | gender | age | subject | exam_score | exam_date  | region  | grade_level | school       |
| ---------- | ------------ | ------ | --- | ------- | ---------- | ---------- | ------- | ----------- | ------------ |
| 1001       | Alice Kim    | Female | 19  | Math    | 78.5       | 2025-03-10 | Nairobi | Year 1      | Hillcrest HS |
| 1002       | Brian Otieno | Male   | 20  | Science | 88.0       | 2025-03-11 | Kisumu  | Year 2      | St. Mary’s   |

---

#### 🧮 2. Transform Phase (Validated Data)

After applying data cleaning and validation rules, the **Transform** phase produced the following:

| student_id | name         | gender | age | subject | exam_score | exam_date  | region  | grade_level | school       |
| ---------- | ------------ | ------ | --- | ------- | ---------- | ---------- | ------- | ----------- | ------------ |
| 1001       | Alice Kim    | Female | 19  | Math    | 78.50      | 2025-03-10 | Nairobi | Year 1      | Hillcrest HS |
| 1002       | Brian Otieno | Male   | 20  | Science | 88.00      | 2025-03-11 | Kisumu  | Year 2      | St. Mary’s   |

✅ *All missing values filled, and `exam_date` converted to `datetime`.*

---

 **Transformations applied:**
- Missing categorical values filled with `"Unknown"`.
- Converted `exam_date` to `datetime`.
- Ensured all `exam_score` values between `0–100`.
- Verified numeric data types (`int64`, `float64`).

---

### 📊 3. Final Validation Summary

Example output from validation checks:

```python
print(full.shape, inc.shape)
print(full.dtypes)

 **OUTPUT**
(9000, 10) (9000, 10)
student_id         int64
name              object
gender            object
age                int64
subject           object
exam_score       float64
exam_date         datetime64[ns]
region            object
grade_level       object
school            object
dtype: object

✅ 4. Final Deliverables

Validated datasets ready for loading:

data/validated_full.csv

data/validated_incremental.csv

Each dataset is fully cleaned, validated, and ready for warehouse integration.

## Load & Verification

### Format Used
I used **SQLite** as the load format because it’s lightweight, easy to use, and stores data locally in a `.db` file. This made it convenient to verify my load and preview results directly from Python without needing an external server.

---

### Verification Code Snippet
```python
import sqlite3
import pandas as pd

# Connect to the SQLite database
conn = sqlite3.connect('loaded/full_data.db')

# Verify data in one of the tables
preview = pd.read_sql('SELECT * FROM full_data LIMIT 5', conn)
print(preview.head())

# Count total rows for verification
count = pd.read_sql('SELECT COUNT(*) AS total_records FROM full_data', conn)
print(count)
conn.close()

Issues Faced & How I Solved Them
Issue 1:
Database not saving properly
Cause:
The loaded/ directory didn’t exist
Solution:
Created the folder manually using os.makedirs('loaded', exist_ok=True)
Issue 2:
Git kept pushing all files
Cause:
No .gitignore configured
Solution
Added unwanted folders like /data, /transformed, and /loaded/*.db to .gitignore
Issue 3:
Verification error
Cause:
Table name mismatch
Solution:
Ensured consistent naming between the CSV file and SQL table (full_data)
