 

# Module 5 — Data Inspection (EDA Level 1)

---

# 🎯 Learning Objectives

After completing this module, you will be able to:

* Understand the structure of any dataset.
* Inspect rows and columns.
* Check data types.
* Identify missing values.
* Understand numerical distributions.
* Estimate memory usage.
* Detect potential problems before preprocessing.

---

# Suppose We Have This Dataset

We'll use the same DataFrame throughout this module.

```python
import pandas as pd

df = pd.DataFrame({
    "Name":["Rahul","Amit","Neha","Priya","John"],
    "Age":[22,25,21,24,None],
    "Salary":[50000,65000,55000,70000,60000],
    "Department":["IT","HR","IT","Sales","HR"]
})

print(df)
```

Output

| Index | Name  | Age | Salary | Department |
| ----: | ----- | --: | -----: | ---------- |
|     0 | Rahul |  22 |  50000 | IT         |
|     1 | Amit  |  25 |  65000 | HR         |
|     2 | Neha  |  21 |  55000 | IT         |
|     3 | Priya |  24 |  70000 | Sales      |
|     4 | John  | NaN |  60000 | HR         |

---

# Why Data Inspection?

Suppose someone gives you a CSV file.

Would you immediately train an ML model?

❌ No.

First ask:

* How many rows?
* How many columns?
* Missing values?
* Wrong data types?
* Numerical or categorical?
* Duplicate rows?
* Memory usage?

Pandas inspection methods answer these questions.

---

# 5.1 head() ⭐⭐⭐⭐⭐

## Purpose

Displays the first **n rows**.

### Syntax

```python
df.head(n)
```

Default

```python
df.head()
```

Output

| Name  | Age | Salary | Department |
| ----- | --: | -----: | ---------- |
| Rahul |  22 |  50000 | IT         |
| Amit  |  25 |  65000 | HR         |
| Neha  |  21 |  55000 | IT         |
| Priya |  24 |  70000 | Sales      |
| John  | NaN |  60000 | HR         |

---

Display only first two rows

```python
df.head(2)
```

Output

| Name  | Age |
| ----- | --: |
| Rahul |  22 |
| Amit  |  25 |

---

## Why Use head()?

Imagine

```text
10,000,000 rows
```

Printing the whole dataset

```python
print(df)
```

is not practical.

Instead

```python
df.head()
```

lets you quickly verify:

* Column names
* Data types (visually)
* Obvious missing values
* Overall structure

---

# 5.2 tail() ⭐⭐⭐⭐

Shows the last rows.

```python
df.tail()
```

Default

Last five rows.

---

Last two rows

```python
df.tail(2)
```

Output

| Name  | Age |
| ----- | --: |
| Priya |  24 |
| John  | NaN |

---

### Why Use tail()?

Useful for checking:

* Data loaded completely
* Appended records
* Final observations
* Time-series ending values

---

# 5.3 sample() ⭐⭐⭐⭐

Instead of first rows,

choose random rows.

```python
df.sample()
```

Returns one random row.

---

Three random rows

```python
df.sample(3)
```

Possible output

| Name  | Age |
| ----- | --: |
| John  | NaN |
| Rahul |  22 |
| Priya |  24 |

Every execution may return different rows.

---

## Why Use sample()?

Suppose the dataset is sorted by date.

Using only

```python
head()
```

shows only the oldest records.

Random sampling gives a broader view.

---

# 5.4 shape ⭐⭐⭐⭐⭐

One of the most frequently used attributes.

```python
df.shape
```

Output

```text
(5,4)
```

Meaning

```text
5 Rows

4 Columns
```

Visualization

```text
Rows × Columns
```

---

Why Important?

Suppose

```text
800000 × 45
```

Immediately tells you

* Dataset size
* Complexity
* Approximate memory requirements

---

# 5.5 columns ⭐⭐⭐⭐⭐

Displays column names.

```python
df.columns
```

Output

```text
Index([
'Name',
'Age',
'Salary',
'Department'
])
```

Useful for

* Checking spelling
* Selecting columns
* Renaming columns

---

Convert to list

```python
list(df.columns)
```

Output

```text
['Name','Age','Salary','Department']
```

---

# 5.6 index ⭐⭐⭐⭐

Displays row labels.

```python
df.index
```

Output

```text
RangeIndex(
start=0,
stop=5,
step=1
)
```

---

Custom index

Suppose

Employee IDs

```text
EMP101

EMP102

EMP103
```

Then

```python
df.index
```

returns

```text
Index([
'EMP101',
'EMP102',
'EMP103'
])
```

---

# 5.7 dtypes ⭐⭐⭐⭐⭐

Shows datatype of each column.

```python
df.dtypes
```

Output

```text
Name           object

Age           float64

Salary         int64

Department     object
```

---

Why did Age become float?

Because

```text
NaN
```

exists.

Traditional `NaN` requires a floating-point representation in this case.

---

ML Importance

Before modeling,

always check

```python
df.dtypes
```

Many algorithms require numerical features.

---

# 5.8 info() ⭐⭐⭐⭐⭐

One of the most important methods.

```python
df.info()
```

Example output

```text
<class 'pandas.core.frame.DataFrame'>

RangeIndex: 5 entries

Data columns:

Name

Age

Salary

Department

dtypes:

object

float64

int64

Memory usage:
```

---

What info() tells you

✅ Number of rows

✅ Number of columns

✅ Column names

✅ Non-null values

✅ Data types

✅ Memory usage

---

Example

Suppose

Age

```text
22

25

NaN

24
```

info()

shows

```text
Non-Null Count

3
```

Immediately tells you

One value is missing.

---

Why is info() Important?

If I receive a new dataset,

the first command I usually run is

```python
df.info()
```

because it summarizes the dataset quickly.

---

# 5.9 describe() ⭐⭐⭐⭐⭐

Provides descriptive statistics.

```python
df.describe()
```

Output (numeric columns)

| Statistic | Age | Salary |
| --------- | --: | -----: |
| count     |   4 |      5 |
| mean      |  23 |  60000 |
| std       | ... |    ... |
| min       |  21 |  50000 |
| 25%       | ... |    ... |
| 50%       | ... |    ... |
| 75%       | ... |    ... |
| max       |  25 |  70000 |

---

Meaning of each row

| Field | Meaning            |
| ----- | ------------------ |
| count | Non-missing values |
| mean  | Average            |
| std   | Standard deviation |
| min   | Minimum            |
| 25%   | First quartile     |
| 50%   | Median             |
| 75%   | Third quartile     |
| max   | Maximum            |

---

Why Use describe()?

Quickly detect

* Outliers
* Large variation
* Missing values
* Data spread

---

Categorical summary

```python
df.describe(include="object")
```

Output includes

* count
* unique
* top (most frequent)
* frequency

Very useful for text columns.

---

All columns

```python
df.describe(include="all")
```

---

# 5.10 memory_usage() ⭐⭐⭐⭐

Shows memory consumed by each column.

```python
df.memory_usage()
```

Output

```text
Index

Name

Age

Salary

Department
```

Include actual string memory

```python
df.memory_usage(deep=True)
```

---

Why Important?

Large datasets

```text
10 million rows
```

may consume several GB of RAM.

Understanding memory helps optimize data types.

---

# 5.11 size ⭐⭐⭐

```python
df.size
```

Output

```text
20
```

Because

```text
5 Rows

×

4 Columns

=

20 Values
```

---

# 5.12 ndim ⭐⭐⭐

```python
df.ndim
```

Output

```text
2
```

Because

DataFrame

↓

Two-dimensional

---

# Common Inspection Workflow ⭐⭐⭐⭐⭐

Whenever you receive a dataset:

```python
df.head()

df.shape

df.columns

df.dtypes

df.info()

df.describe()

df.memory_usage(deep=True)
```

This sequence answers most initial questions.

---

# Real ML Example

Suppose

```python
df = pd.read_csv("house_prices.csv")
```

First inspection

```python
df.head()

df.info()

df.describe()

df.dtypes

df.shape
```

From these results, you can identify:

* Missing values
* Wrong data types
* Number of samples
* Number of features
* Numerical ranges
* Potential outliers

Only after that should you begin preprocessing.

---

# Common Mistakes

### ❌ Printing the whole DataFrame

```python
print(df)
```

for millions of rows.

Use

```python
df.head()
```

instead.

---

### ❌ Ignoring `info()`

Many beginners skip it and later discover incorrect data types.

---

### ❌ Assuming `describe()` includes text columns

By default, it summarizes only numeric columns.

Use

```python
include="object"
```

or

```python
include="all"
```

when appropriate.

---

### ❌ Confusing `shape` and `size`

| `shape`           | `size`                   |
| ----------------- | ------------------------ |
| `(rows, columns)` | Total number of elements |

Example

```text
5 × 4
```

Shape

```text
(5,4)
```

Size

```text
20
```

---

# Interview Questions

### 1. Difference between `head()` and `tail()`?

* `head()` returns the first rows.
* `tail()` returns the last rows.

---

### 2. Difference between `shape` and `size`?

* `shape` → `(rows, columns)`
* `size` → total number of values

---

### 3. Why is `info()` one of the first methods you use?

Because it provides:

* Data types
* Missing values (non-null counts)
* Number of rows
* Memory usage

---

### 4. Why use `sample()`?

To inspect random records and reduce bias from viewing only the first rows.

---

### 5. Why does `describe()` sometimes ignore text columns?

Because, by default, it summarizes only numeric data. Use `include="object"` or `include="all"` to include categorical columns.

---

# Practice Exercises

Create the following DataFrame:

| Name  | Age | Salary | Department |
| ----- | --: | -----: | ---------- |
| Rahul |  22 |  50000 | IT         |
| Amit  |  25 |  65000 | HR         |
| Neha  |  21 |  55000 | IT         |
| Priya |  24 |  70000 | Sales      |
| John  | NaN |  60000 | HR         |

Perform the following:

1. Display the first three rows.
2. Display the last two rows.
3. Display two random rows.
4. Print the shape.
5. Print all column names.
6. Print the index.
7. Print data types.
8. Run `info()`.
9. Run `describe()`.
10. Display memory usage.

---

