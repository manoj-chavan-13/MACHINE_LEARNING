
# 📘 Pandas for Machine Learning

# Module 1 — Introduction to Pandas

---

# Learning Objectives

After completing this module, you will be able to:

* Understand what Pandas is.
* Explain why Pandas is used in Machine Learning.
* Understand the difference between Pandas and NumPy.
* Understand the difference between Pandas and Excel.
* Understand Series and DataFrame.
* Understand rows, columns, index, and data types.
* Install and import Pandas.
* Write your first Pandas program.

---

# 1.1 What is Pandas?

**Pandas** is an open-source Python library designed for working with **structured or tabular data**.

It provides powerful data structures and functions to:

* Read datasets
* Clean data
* Transform data
* Analyze data
* Prepare data for Machine Learning

In almost every ML project, **Pandas is the first library you use**.

---

## Real Machine Learning Workflow

A typical ML project looks like this:

```text
Dataset (CSV / Excel / SQL)
          │
          ▼
Read Dataset using Pandas
          │
          ▼
Inspect Dataset
          │
          ▼
Clean Dataset
          │
          ▼
Feature Engineering
          │
          ▼
Split Features & Target
          │
          ▼
Train Machine Learning Model
```

Notice that almost everything before model training is handled using **Pandas**.

---

# 1.2 Why Do We Need Pandas?

Suppose we have student data.

| Roll | Name  | Age | Marks |
| ---- | ----- | --- | ----: |
| 101  | Rahul | 20  |    85 |
| 102  | Amit  | 21  |    92 |
| 103  | Neha  | 19  |    88 |

Without Pandas, you might store it as:

```python
roll = [101,102,103]
name = ["Rahul","Amit","Neha"]
age = [20,21,19]
marks = [85,92,88]
```

Now imagine you want:

* Students older than 20
* Average marks
* Highest marks
* Students sorted by marks

You would need loops and manual logic.

With Pandas:

```python
import pandas as pd

df[df["Age"] > 20]

df["Marks"].mean()

df.sort_values("Marks")
```

The code is shorter, easier to read, and scales to much larger datasets.

---

# 1.3 Why is Pandas Important for Machine Learning?

Machine Learning models expect **clean, structured data**.

Real-world data often contains:

| Problem        | Example                     |
| -------------- | --------------------------- |
| Missing values | Age = NaN                   |
| Duplicate rows | Same customer appears twice |
| Wrong datatype | Age stored as text          |
| Extra spaces   | `" Rahul "`                 |
| Mixed formats  | Dates stored differently    |
| Invalid values | Salary = -5000              |

Pandas provides tools to solve these problems before training a model.

Example:

```python
df = df.drop_duplicates()

df["Age"] = df["Age"].fillna(df["Age"].median())

df["Name"] = df["Name"].str.strip()
```

---

# 1.4 Why Not Use Excel?

Many beginners ask:

> If Excel can store tables, why learn Pandas?

### Excel

✅ Easy for small datasets

❌ Slow for millions of rows

❌ Difficult to automate

❌ Not suitable for ML pipelines

---

### Pandas

✅ Handles large datasets (limited by available memory)

✅ Easy to automate

✅ Integrates with Python

✅ Works directly with ML libraries like `scikit-learn`

---

Example:

Imagine cleaning **500 CSV files**.

Excel:

* Open file
* Clean file
* Save file
* Repeat 500 times

Pandas:

```python
for file in files:
    df = pd.read_csv(file)
    # Cleaning steps
```

One script can process every file consistently.

---

# 1.5 Why Not Use NumPy Alone?

This is an important interview question.

### NumPy

Designed for **fast numerical computation**.

Example:

```python
import numpy as np

arr = np.array([10,20,30])
```

It is excellent for matrix operations.

---

### Pandas

Built **on top of NumPy**.

It adds:

* Column names
* Row labels
* Missing-value handling
* Reading CSV/Excel
* Filtering by labels
* Grouping and aggregation

---

Example

NumPy:

```python
array([
    [1,20],
    [2,25]
])
```

Question:

Which column is Age?

Not obvious.

---

Pandas:

| ID | Age |
| -- | --- |
| 1  | 20  |
| 2  | 25  |

Column names make data much easier to understand.

---

## Comparison

| Feature        | NumPy   | Pandas                |
| -------------- | ------- | --------------------- |
| Arrays         | ✅       | Uses NumPy internally |
| Column Names   | ❌       | ✅                     |
| Missing Values | Limited | Excellent support     |
| CSV Reading    | ❌       | ✅                     |
| Excel Reading  | ❌       | ✅                     |
| SQL Support    | ❌       | ✅                     |
| GroupBy        | ❌       | ✅                     |
| Data Cleaning  | Limited | Excellent             |

**Conclusion:** NumPy is the computation engine; Pandas is the data manipulation layer.

---

# 1.6 Installing Pandas

Using pip:

```bash
pip install pandas
```

Using Anaconda:

```bash
conda install pandas
```

Check the installed version:

```python
import pandas as pd

print(pd.__version__)
```

Example output:

```text
2.3.0
```

Your version may differ.

---

# 1.7 Importing Pandas

The standard import is:

```python
import pandas as pd
```

Why `pd`?

It's simply the community convention.

Instead of writing:

```python
pandas.read_csv("data.csv")
```

we write:

```python
pd.read_csv("data.csv")
```

Shorter and universally recognized.

---

# 1.8 Core Data Structures in Pandas

Pandas has **two primary data structures**.

---

## 1. Series

A **Series** is a one-dimensional labeled array.

Example:

```python
age = pd.Series([20,22,19])
```

Visualization:

```text
Index    Value

0         20

1         22

2         19
```

Think of a Series as **one column** of data.

---

## 2. DataFrame

A **DataFrame** is a two-dimensional table.

Example:

```python
students = pd.DataFrame({
    "Name": ["Rahul", "Amit"],
    "Age": [20,21]
})
```

Visualization:

| Index | Name  | Age |
| ----: | ----- | --: |
|     0 | Rahul |  20 |
|     1 | Amit  |  21 |

Think of a DataFrame as an **Excel worksheet**.

---

## Relationship

```text
DataFrame

 ┌───────────────┐
 │ Name │ Age    │
 │ Rahul│ 20     │
 │ Amit │ 21     │
 └───────────────┘

Each column is a Series.
```

A DataFrame is essentially a collection of aligned Series sharing the same index.

---

# 1.9 Rows, Columns, and Index

Consider:

| Index | Name  | Age | Marks |
| ----: | ----- | --: | ----: |
|     0 | Rahul |  20 |    85 |
|     1 | Amit  |  21 |    92 |
|     2 | Neha  |  19 |    88 |

### Row

A row represents **one observation**.

Example:

```text
Rahul

20

85
```

This is one student's record.

---

### Column

A column represents **one feature (variable)**.

Example:

```text
Age

20

21

19
```

---

### Index

The index identifies rows.

Default index:

```text
0

1

2
```

You can also use custom indexes:

```text
EMP101

EMP102

EMP103
```

The index makes selection and alignment more meaningful.

---

# 1.10 Data Types (`dtype`)

Each Series has a data type.

Common types:

| Data       | Pandas dtype         |
| ---------- | -------------------- |
| 10         | `int64`              |
| 10.5       | `float64`            |
| True       | `bool`               |
| "Rahul"    | `object` or `string` |
| 2026-06-30 | `datetime64[ns]`     |

Example:

```python
age = pd.Series([20,21,22])

print(age.dtype)
```

Output:

```text
int64
```

Knowing data types is important because many ML preprocessing steps depend on them.

---

# 1.11 First Pandas Program

```python
import pandas as pd

students = pd.DataFrame({
    "Name": ["Rahul", "Amit", "Neha"],
    "Age": [20,21,19],
    "Marks": [85,92,88]
})

print(students)
```

Output:

```text
    Name  Age  Marks
0  Rahul   20     85
1   Amit   21     92
2   Neha   19     88
```

This creates your first DataFrame.

---

# 1.12 Real ML Example

Suppose you receive a CSV file:

```text
house_prices.csv
```

The first step is:

```python
import pandas as pd

df = pd.read_csv("house_prices.csv")
```

Then:

```python
print(df.head())
print(df.info())
print(df.describe())
```

Before building any ML model, you first understand the dataset using Pandas.

---

# Common Mistakes

### ❌ Forgetting the alias

```python
read_csv("data.csv")
```

Correct:

```python
pd.read_csv("data.csv")
```

---

### ❌ Treating a Series as a DataFrame

A Series has one dimension; a DataFrame has two.

---

### ❌ Thinking Pandas replaces NumPy

Pandas depends heavily on NumPy. They complement each other rather than replace one another.

---

# Interview Questions

### 1. What is Pandas?

A Python library for data manipulation, analysis, and preprocessing of structured data.

---

### 2. Why is Pandas used in Machine Learning?

Because it simplifies loading, cleaning, transforming, and preparing data before model training.

---

### 3. Difference between Series and DataFrame?

| Series          | DataFrame                          |
| --------------- | ---------------------------------- |
| One-dimensional | Two-dimensional                    |
| Single column   | Multiple columns                   |
| One dtype       | Each column can have its own dtype |

---

### 4. Difference between NumPy and Pandas?

NumPy focuses on numerical arrays and computation; Pandas builds on NumPy to provide labeled tabular data structures and powerful data manipulation tools.

---

### 5. Why is Pandas preferred over Excel for ML?

Because it is programmable, automatable, handles much larger datasets, and integrates directly with the Python ML ecosystem.

---

# Practice Exercises

### Exercise 1

Install Pandas and print its version.

---

### Exercise 2

Import Pandas using the standard alias.

---

### Exercise 3

Create a Series containing:

```text
10
20
30
40
50
```

Print it.

---

### Exercise 4

Create the following DataFrame:

| Name  | Age | Marks |
| ----- | --: | ----: |
| Rahul |  20 |    85 |
| Amit  |  21 |    92 |
| Neha  |  19 |    88 |

Print the DataFrame.

---

### Exercise 5

In your own words, explain:

* What is a Series?
* What is a DataFrame?
* Why is Pandas important in Machine Learning?

---

