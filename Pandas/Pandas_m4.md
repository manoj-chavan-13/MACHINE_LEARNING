# Module 4 — Reading Datasets

---

# 🎯 Learning Objectives

After this module, you will be able to:

* Read CSV, Excel, JSON, Parquet, and SQL data
* Understand the most important parameters
* Load only the required columns
* Handle missing values during import
* Parse dates correctly
* Improve loading performance
* Avoid common dataset loading mistakes

---

# 4.1 Why Reading Data is Important

Before you can clean, analyze, or train a Machine Learning model, you must first load the dataset.

Typical ML workflow:

```text
CSV File
    ↓
Pandas
    ↓
DataFrame
    ↓
Cleaning
    ↓
Machine Learning
```

---

# 4.2 CSV Files

CSV stands for **Comma Separated Values**.

Example:

```csv
Name,Age,Salary
Rahul,22,50000
Amit,25,60000
Neha,21,55000
```

This is the most common file format in Machine Learning.

---

# read_csv()

## Syntax

```python
pd.read_csv(filepath, parameters...)
```

Example

```python
import pandas as pd

df = pd.read_csv("students.csv")
```

Now

```python
print(df)
```

Output

| Name  | Age | Salary |
| ----- | --: | -----: |
| Rahul |  22 |  50000 |
| Amit  |  25 |  60000 |
| Neha  |  21 |  55000 |

---

# How read_csv() Works Internally

```
students.csv

↓

Read Text

↓

Split by Delimiter

↓

Identify Header

↓

Infer Data Types

↓

Create DataFrame
```

Pandas automatically:

* Reads the file
* Detects columns
* Creates indexes
* Infers data types

---

# Important Parameters

---

# filepath

The location of the file.

```python
pd.read_csv("students.csv")
```

Absolute path

```python
pd.read_csv("D:/Datasets/students.csv")
```

Linux/macOS

```python
pd.read_csv("/home/manoj/students.csv")
```

---

# sep

Delimiter.

Default

```text
,
```

Suppose data uses

```text
;
```

instead.

```
Name;Age;Salary
Rahul;22;50000
```

Read it as

```python
pd.read_csv(
    "students.csv",
    sep=";"
)
```

Other separators:

```python
sep="\t"      # Tab

sep="|"       # Pipe

sep=":"       # Colon
```

---

# header

Specifies which row contains column names.

Normal CSV

```csv
Name,Age
Rahul,22
```

Default

```python
header=0
```

If there is **no header**:

```csv
Rahul,22
Amit,25
```

Read using

```python
pd.read_csv(
    "students.csv",
    header=None
)
```

Output

| 0     | 1  |
| ----- | -- |
| Rahul | 22 |
| Amit  | 25 |

---

# names

Assign custom column names.

```python
pd.read_csv(
    "students.csv",
    header=None,
    names=["Name","Age"]
)
```

Output

| Name  | Age |
| ----- | --: |
| Rahul |  22 |
| Amit  |  25 |

Useful when datasets don't have headers.

---

# usecols ⭐⭐⭐⭐⭐

One of the most useful parameters.

Suppose dataset has

```text
50 Columns
```

You only need

```text
Age

Salary

Purchased
```

Instead of reading everything,

```python
pd.read_csv(
    "customers.csv",
    usecols=["Age","Salary","Purchased"]
)
```

Benefits:

* Faster loading
* Lower memory usage
* Cleaner DataFrame

---

# nrows

Read only the first few rows.

```python
pd.read_csv(
    "customers.csv",
    nrows=100
)
```

Useful for quickly inspecting very large datasets.

---

# skiprows

Skip rows at the beginning.

```python
pd.read_csv(
    "students.csv",
    skiprows=2
)
```

Useful when files contain metadata before the actual table.

---

# index_col

Specify which column should become the index.

Example

| ID  | Name  | Age |
| --- | ----- | --: |
| 101 | Rahul |  22 |
| 102 | Amit  |  25 |

Instead of

```
0

1
```

Use

```python
pd.read_csv(
    "students.csv",
    index_col="ID"
)
```

Output

| ID  | Name  | Age |
| --- | ----- | --: |
| 101 | Rahul |  22 |
| 102 | Amit  |  25 |

Now the index uses employee/student IDs.

---

# dtype ⭐⭐⭐⭐⭐

Specify data types manually.

Example

```python
pd.read_csv(
    "students.csv",
    dtype={
        "Age":"int32",
        "Salary":"float64"
    }
)
```

Why?

Sometimes Pandas guesses incorrectly.

Specifying dtypes:

* Prevents incorrect inference
* Can reduce memory usage
* Ensures consistency

---

# parse_dates ⭐⭐⭐⭐⭐

Suppose

```csv
JoiningDate
2025-01-10
2025-03-15
```

Read as dates

```python
pd.read_csv(
    "employees.csv",
    parse_dates=["JoiningDate"]
)
```

Without this,

Pandas may treat them as plain text.

---

# na_values ⭐⭐⭐⭐

Sometimes missing values appear as

```
?

NA

Unknown

missing
```

Instead of `NaN`.

Tell Pandas to treat them as missing.

```python
pd.read_csv(
    "students.csv",
    na_values=["?","Unknown"]
)
```

Now those values become proper missing values (`NaN`).

---

# encoding

Different files use different character encodings.

Default

```python
encoding="utf-8"
```

Sometimes

```python
encoding="latin1"
```

or

```python
encoding="cp1252"
```

If you encounter strange characters or `UnicodeDecodeError`, checking the file's encoding often solves the issue.

---

# low_memory

For large CSV files,

```python
pd.read_csv(
    "big.csv",
    low_memory=False
)
```

This can improve dtype inference consistency, although it may use more memory during reading.

---

# Reading Excel Files

Excel format

```
students.xlsx
```

Use

```python
pd.read_excel(
    "students.xlsx"
)
```

Read a specific sheet

```python
pd.read_excel(
    "students.xlsx",
    sheet_name="Sales"
)
```

Read multiple sheets

```python
pd.read_excel(
    "students.xlsx",
    sheet_name=None
)
```

Returns a dictionary of DataFrames.

---

# Reading JSON

Example

```json
[
 {"Name":"Rahul","Age":22},
 {"Name":"Amit","Age":25}
]
```

Read

```python
pd.read_json(
    "students.json"
)
```

Useful for APIs and web applications.

---

# Reading Parquet ⭐⭐⭐⭐⭐

Parquet is a columnar storage format.

```python
pd.read_parquet(
    "customers.parquet"
)
```

Why use Parquet?

* Faster reads
* Better compression
* Lower storage requirements
* Widely used in Big Data ecosystems

Many production ML pipelines store datasets as Parquet instead of CSV.

---

# Reading SQL

Suppose data is stored in a database.

```python
import sqlite3

connection = sqlite3.connect("company.db")
```

Read

```python
pd.read_sql(
    "SELECT * FROM employees",
    connection
)
```

Very common in industry.

---

# Reading Large Datasets Efficiently

Suppose

```
5 GB CSV
```

Don't read everything if unnecessary.

Instead

```python
pd.read_csv(
    "customers.csv",
    usecols=[
        "Age",
        "Salary",
        "Purchased"
    ]
)
```

or

```python
pd.read_csv(
    "customers.csv",
    nrows=5000
)
```

Always read only what you need.

---

# Common Errors

---

## FileNotFoundError

```python
pd.read_csv(
    "student.csv"
)
```

Error

```
FileNotFoundError
```

Reason

Wrong filename or incorrect path.

---

## Wrong Separator

CSV

```
Name;Age
```

But

```python
pd.read_csv(
    "students.csv"
)
```

Output

```
Name;Age
```

Everything appears in one column.

Solution

```python
sep=";"
```

---

## Wrong Encoding

Sometimes

```
UnicodeDecodeError
```

Try

```python
encoding="latin1"
```

or the appropriate encoding for your file.

---

## Wrong Data Types

Age

```
20

21

ABC
```

Pandas may infer the column as text because of mixed values.

Inspect with

```python
df.dtypes
```

and clean or convert the data afterward.

---

# ML Example

Suppose

```
house_prices.csv
```

Contains

```
Id

Area

Bedrooms

Price

Date
```

Load

```python
df = pd.read_csv(

"house_prices.csv",

parse_dates=["Date"],

usecols=[

"Area",

"Bedrooms",

"Price",

"Date"

]

)
```

Now

* Only useful columns are loaded.
* Dates are parsed correctly.
* Less memory is used.

The DataFrame is ready for inspection.

---

# Best Practices ⭐⭐⭐⭐⭐

✔ Always inspect the first few rows after reading:

```python
df.head()
```

✔ Check data types:

```python
df.dtypes
```

✔ Check missing values:

```python
df.isna().sum()
```

✔ Load only required columns.

✔ Parse dates during import.

✔ Set the correct index if one exists.

---

# Interview Questions

### 1. What is the difference between `read_csv()` and `read_excel()`?

* `read_csv()` reads comma-separated text files.
* `read_excel()` reads Excel workbooks (`.xlsx`, `.xls`).

---

### 2. Why use `usecols`?

To load only the required columns, improving speed and reducing memory usage.

---

### 3. What is `parse_dates` used for?

To convert specified columns directly into datetime objects while reading the dataset.

---

### 4. Why use `dtype`?

To control column data types, avoid incorrect inference, and sometimes reduce memory usage.

---

### 5. Why is Parquet preferred over CSV in many production ML pipelines?

Because it is typically faster to read, more storage-efficient, and preserves data types better.

---

# Practice Exercises

### Exercise 1

Read a CSV file named `employees.csv`.

---

### Exercise 2

Read only these columns:

* Name
* Salary

---

### Exercise 3

Read only the first 20 rows.

---

### Exercise 4

Set `EmployeeID` as the index.

---

### Exercise 5

Read a file where missing values are represented by `"Unknown"`.

---

### Exercise 6

Read a dataset and convert the `JoiningDate` column into datetime during import.

---

