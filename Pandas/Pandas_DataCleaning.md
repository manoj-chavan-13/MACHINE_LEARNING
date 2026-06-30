# Module 7 — Data Cleaning (Complete)

---

# 🎯 Learning Objectives

After completing this module, you will be able to:

* Detect missing values
* Handle missing values correctly
* Remove duplicate records
* Replace incorrect values
* Convert data types
* Clean data for ML models
* Understand best practices for preprocessing

---

# Sample Dataset

We'll use this dataset throughout the module.

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "Name": ["Rahul","Amit","Neha","Rahul","John"],
    "Age": [22,np.nan,21,22,"25"],
    "Salary": [50000,65000,np.nan,50000,60000],
    "Department": ["IT","HR","IT","IT","Unknown"]
})

print(df)
```

Output

| Index | Name  | Age  | Salary | Department |
| ----- | ----- | ---- | ------ | ---------- |
| 0     | Rahul | 22   | 50000  | IT         |
| 1     | Amit  | NaN  | 65000  | HR         |
| 2     | Neha  | 21   | NaN    | IT         |
| 3     | Rahul | 22   | 50000  | IT         |
| 4     | John  | "25" | 60000  | Unknown    |

Immediately we can see problems:

* Missing Age
* Missing Salary
* Duplicate row
* Wrong datatype (`"25"` is a string)
* Invalid category (`Unknown`)

This is typical of real datasets.

---

# Why Data Cleaning?

Machine Learning algorithms expect **clean and consistent** data.

Example:

```text
Age

22

NaN

25
```

Many algorithms cannot train with missing values.

---

# Types of Data Problems

| Problem           | Example             |
| ----------------- | ------------------- |
| Missing values    | NaN                 |
| Duplicate rows    | Same customer twice |
| Wrong datatype    | Age stored as text  |
| Invalid values    | Unknown             |
| Inconsistent text | " IT ", "it", "IT"  |
| Wrong dates       | 31/02/2025          |

Pandas provides tools to solve all of these.

---

# Part 1 — Missing Values ⭐⭐⭐⭐⭐

---

# What is a Missing Value?

Missing means

> Information is unavailable.

Example

| Name  | Age |
| ----- | --- |
| Rahul | 22  |
| Amit  | NaN |
| Neha  | 21  |

Amit's age is missing.

---

# Why Missing Values Occur?

* User skipped a form field.
* Sensor failure.
* Database corruption.
* Data entry mistake.
* File import problems.

---

# How Pandas Represents Missing Data

There are three common representations.

## 1. NaN

```python
import numpy as np

np.nan
```

Most common for numeric data.

---

## 2. None

Python's null object.

```python
None
```

Often converted internally by Pandas.

---

## 3. pd.NA

Modern nullable missing value.

```python
pd.NA
```

Used with nullable Pandas dtypes.

---

# Detect Missing Values

## isna()

```python
df.isna()
```

Output

| Name  | Age   | Salary |
| ----- | ----- | ------ |
| False | False | False  |
| False | True  | False  |
| False | False | True   |

True = Missing value.

---

## isnull()

```python
df.isnull()
```

Exactly the same as

```python
df.isna()
```

Interview Question ⭐⭐⭐⭐⭐

Difference?

Answer:

None.

`isnull()` is simply an alias for `isna()`.

---

# Count Missing Values

One of the most frequently used commands.

```python
df.isna().sum()
```

Example Output

```text
Name          0
Age           1
Salary        1
Department    0
```

This tells you exactly how many missing values exist in each column.

---

# Percentage of Missing Values

A common EDA step.

```python
(df.isna().sum() / len(df)) * 100
```

Example Output

```text
Age       20%
Salary    20%
```

This helps decide whether to fill or drop missing values.

---

# Removing Missing Values

## dropna()

```python
df.dropna()
```

Removes every row containing at least one missing value.

Before

| Name  | Age | Salary |
| ----- | --- | ------ |
| Rahul | 22  | 50000  |
| Amit  | NaN | 65000  |
| Neha  | 21  | NaN    |

After

| Name  | Age | Salary |
| ----- | --- | ------ |
| Rahul | 22  | 50000  |

---

## Drop Only if Entire Row is Missing

```python
df.dropna(how="all")
```

---

## Drop Columns

```python
df.dropna(axis=1)
```

Removes columns containing missing values.

---

# Filling Missing Values

Instead of deleting,

replace them.

---

## Fill with Constant

```python
df.fillna(0)
```

Output

| Age |
| --- |
| 22  |
| 0   |
| 21  |

Useful only when 0 makes sense.

---

## Fill Numerical Data with Mean

```python
df["Age"] = df["Age"].fillna(df["Age"].mean())
```

Suppose

```text
20

25

30
```

Mean

```text
25
```

The missing value becomes 25.

---

## Fill with Median ⭐⭐⭐⭐⭐

```python
df["Age"] = df["Age"].fillna(df["Age"].median())
```

Preferred when outliers exist.

Example

```text
20

25

1000
```

Mean = 348.3 ❌

Median = 25 ✅

---

## Fill with Mode

Useful for categorical columns.

```python
df["Department"] = df["Department"].fillna(
    df["Department"].mode()[0]
)
```

---

## Forward Fill

```python
df.ffill()
```

Copies previous value.

Example

```text
20

NaN

30
```

Becomes

```text
20

20

30
```

Mostly useful in time-series.

---

## Backward Fill

```python
df.bfill()
```

Copies next value.

---

# When Should You Use Each?

| Method        | Best Use Case                     |
| ------------- | --------------------------------- |
| Mean          | Normally distributed numeric data |
| Median        | Numeric data with outliers        |
| Mode          | Categorical data                  |
| Forward Fill  | Time-series data                  |
| Backward Fill | Sequential observations           |
| Drop          | Very few missing rows             |

---

# Part 2 — Duplicate Data ⭐⭐⭐⭐⭐

Suppose

| Name  | Age |
| ----- | --- |
| Rahul | 22  |
| Rahul | 22  |

Duplicate rows distort statistics and ML training.

---

# Detect Duplicates

```python
df.duplicated()
```

Output

```text
False

False

False

True

False
```

True indicates a duplicate row.

---

# Remove Duplicates

```python
df.drop_duplicates()
```

The first occurrence is kept by default.

---

# Remove Based on Specific Columns

```python
df.drop_duplicates(
    subset=["Name"]
)
```

Useful when uniqueness depends on one or more columns.

---

# Part 3 — Replace Values ⭐⭐⭐⭐⭐

Suppose

```text
Unknown
```

should become

```text
NaN
```

```python
import numpy as np

df.replace(
    "Unknown",
    np.nan
)
```

Very common.

---

Replace multiple values

```python
df.replace({
    "M":"Male",
    "F":"Female"
})
```

---

# Part 4 — Type Conversion ⭐⭐⭐⭐⭐

One of the most important ML preprocessing steps.

---

# astype()

Suppose

Age

```text
"25"
```

Stored as a string.

Convert

```python
df["Age"] = df["Age"].astype(int)
```

Now

```text
25
```

is an integer.

---

Convert to float

```python
df["Salary"] = df["Salary"].astype(float)
```

---

Convert to string

```python
df["Department"] = df["Department"].astype(str)
```

---

# to_numeric()

Safer than `astype()` when values may be invalid.

Suppose

```text
22

25

ABC
```

```python
pd.to_numeric(
    df["Age"],
    errors="coerce"
)
```

Output

```text
22

25

NaN
```

Invalid values become missing values instead of raising an error.

---

# to_datetime()

Dates stored as text.

```text
2025-01-10
```

Convert

```python
df["JoiningDate"] = pd.to_datetime(
    df["JoiningDate"]
)
```

Now Pandas understands them as dates.

---

# Complete Cleaning Pipeline ⭐⭐⭐⭐⭐

A realistic preprocessing sequence:

```python
import numpy as np

# Replace invalid values
df.replace("Unknown", np.nan, inplace=True)

# Remove duplicate rows
df.drop_duplicates(inplace=True)

# Convert data types
df["Age"] = pd.to_numeric(
    df["Age"],
    errors="coerce"
)

# Fill missing numerical values
df["Age"] = df["Age"].fillna(
    df["Age"].median()
)

df["Salary"] = df["Salary"].fillna(
    df["Salary"].median()
)

# Fill missing categorical values
df["Department"] = df["Department"].fillna(
    df["Department"].mode()[0]
)
```

This is very close to what you would do in a real ML project.

---

# Best Practices ⭐⭐⭐⭐⭐

✔ Always inspect missing values first.

```python
df.isna().sum()
```

---

✔ Use **median** when numeric data has outliers.

---

✔ Use **mode** for categorical columns.

---

✔ Convert data types before modeling.

---

✔ Remove duplicates before feature engineering.

---

✔ Replace invalid placeholder values ("Unknown", "N/A", "?") with `NaN` first.

---

# Common Mistakes

❌ Filling every missing value with `0`.

Not every feature treats 0 as meaningful.

---

❌ Dropping half the dataset because of missing values.

Consider imputation before removing rows.

---

❌ Ignoring string numbers.

```text
"25"
```

is **not** the same as

```text
25
```

---

❌ Forgetting to convert dates.

Dates stored as strings limit your ability to extract useful features.

---

# Interview Questions

### 1. Difference between `isna()` and `isnull()`?

No difference.

---

### 2. When should you use mean vs median?

* Mean → approximately symmetric data.
* Median → skewed data or data with outliers.

---

### 3. Difference between `astype()` and `to_numeric()`?

* `astype()` performs a direct conversion and raises an error on invalid values.
* `to_numeric(errors="coerce")` converts invalid values to `NaN`, making it safer for messy datasets.

---

### 4. Why remove duplicates before training?

Duplicate records can bias statistics and cause the model to overemphasize repeated observations.

---

### 5. Why replace `"Unknown"` with `NaN`?

Because Pandas recognizes `NaN` as a missing value, allowing consistent handling with functions like `isna()` and `fillna()`.

---

# Practice Exercises

Using the sample dataset:

1. Count missing values.
2. Calculate the percentage of missing values.
3. Fill `Age` with the median.
4. Fill `Salary` with the mean.
5. Replace `"Unknown"` with `NaN`.
6. Remove duplicate rows.
7. Convert `Age` to numeric using `to_numeric()`.
8. Convert a text date column to datetime.
9. Build a complete cleaning pipeline for the dataset.

---

