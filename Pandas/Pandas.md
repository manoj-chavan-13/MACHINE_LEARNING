# 📘 Pandas Handbook for Machine Learning

## Chapter 1 – Introduction to Pandas

---

# 1.1 What is Pandas?

Pandas is an **open-source Python library** designed for **data manipulation**, **data cleaning**, **data analysis**, and **data preprocessing**.

It provides fast, flexible, and easy-to-use data structures that make working with structured (tabular) data much simpler.

In Machine Learning, Pandas is usually the **first library** you use after loading a dataset.

Example workflow:

```
Dataset
   │
   ▼
Read using Pandas
   │
   ▼
Inspect Data
   │
   ▼
Clean Data
   │
   ▼
Feature Engineering
   │
   ▼
Prepare Dataset
   │
   ▼
Train ML Model
```

Almost every Machine Learning project begins with Pandas.

---

# 1.2 Why was Pandas Created?

Before Pandas existed, Python programmers mainly used:

* Lists
* Dictionaries
* NumPy Arrays

These worked well for numerical computations but were inconvenient for real-world datasets.

Imagine an employee table:

| ID  | Name  | Age | Salary |
| --- | ----- | --- | ------ |
| 101 | Rahul | 25  | 50000  |
| 102 | Amit  | 28  | 60000  |
| 103 | Priya | 23  | 45000  |

Using lists:

```python
id = [101,102,103]
name = ["Rahul","Amit","Priya"]
age = [25,28,23]
salary = [50000,60000,45000]
```

Problems:

* Data scattered across multiple lists
* Difficult to filter
* Difficult to sort
* Difficult to update
* Easy to make mistakes

Example:

Increase Rahul's salary.

Without Pandas:

```python
salary[0] += 5000
```

Now imagine 100,000 employees.

It becomes difficult to maintain.

Pandas solves this problem.

---

# 1.3 Why is Pandas Important in Machine Learning?

Machine Learning algorithms **cannot** work with messy data.

Real-world datasets contain:

* Missing values
* Duplicate records
* Incorrect datatypes
* Empty strings
* Outliers
* Mixed formats

Example:

| Name  | Age | Salary |
| ----- | --- | ------ |
| Rahul | 25  | 50000  |
| Amit  | NaN | 60000  |
| Priya | 23  | 45000  |
| Rahul | 25  | 50000  |

Problems:

* Missing age
* Duplicate row

Before training a model, these issues must be handled.

Pandas provides tools like:

```python
dropna()

fillna()

drop_duplicates()

replace()

astype()
```

Without Pandas, data preprocessing would be much more difficult.

---

# 1.4 What Can Pandas Do?

Pandas supports nearly every common data manipulation task.

### Read data

```python
pd.read_csv()

pd.read_excel()

pd.read_json()

pd.read_sql()
```

---

### View data

```python
head()

tail()

sample()

info()

describe()
```

---

### Clean data

```python
fillna()

dropna()

replace()

drop_duplicates()
```

---

### Filter data

```python
loc

iloc

Boolean Indexing
```

---

### Modify data

```python
rename()

drop()

insert()

assign()
```

---

### Analyze data

```python
groupby()

pivot_table()

value_counts()

agg()
```

---

### Export data

```python
to_csv()

to_excel()

to_json()
```

---

# 1.5 Real-Life Example

Suppose a company stores employee information.

```
Employee Data

ID

Name

Department

Age

Salary
```

Management wants answers like:

* Average salary?
* Highest salary?
* Employees older than 30?
* Employees in IT?
* Employees earning above ₹80,000?

Pandas can answer these questions in a few lines of code.

---

# 1.6 Where is Pandas Used?

Pandas is used in:

### Data Analytics

* Dashboards
* Reports
* KPIs

---

### Machine Learning

* Data Cleaning
* Feature Engineering
* Dataset Preparation

---

### Finance

* Stock Analysis
* Risk Analysis
* Portfolio Analysis

---

### Healthcare

* Patient Records
* Disease Prediction

---

### Banking

* Fraud Detection
* Customer Analytics

---

### E-commerce

* Product Analysis
* Recommendation Systems

---

### Marketing

* Customer Segmentation
* Campaign Analysis

---

# 1.7 Advantages of Pandas

### Easy Syntax

Instead of writing long loops:

```python
for i in range(len(data)):
    ...
```

Pandas often requires only one line.

---

### Fast

Pandas is built on top of **NumPy**.

NumPy performs many operations using optimized C code, making Pandas much faster than ordinary Python loops for most tabular data tasks.

---

### Handles Missing Data

Example:

```python
df.fillna(df["Age"].mean())
```

---

### Supports Large Datasets

Thousands

Millions

Even tens of millions of rows (subject to your computer's available memory).

---

### Rich Functionality

Over **1,000 methods and attributes** are available for working with data.

---

# 1.8 Limitations of Pandas

No library is perfect.

### Memory Usage

Pandas stores data in RAM.

If your dataset is larger than available memory, you'll need alternatives such as distributed processing or out-of-core tools.

---

### Not Ideal for Distributed Computing

For extremely large datasets, frameworks like **Apache Spark** or **Dask** are often used.

---

### Slower than Pure NumPy for Some Numerical Tasks

Pandas prioritizes convenience and labeled data structures.

For heavy numerical computation on homogeneous arrays, NumPy can be faster.

---

# 1.9 Installing Pandas

Using pip

```bash
pip install pandas
```

Using Anaconda

```bash
conda install pandas
```

Check installation

```python
import pandas as pd

print(pd.__version__)
```

Example output

```
2.3.0
```

(The exact version depends on what you have installed.)

---

# 1.10 Importing Pandas

Standard import

```python
import pandas as pd
```

### Why use `pd`?

Instead of writing:

```python
pandas.read_csv()
```

we write:

```python
pd.read_csv()
```

`pd` is simply the widely accepted alias used throughout the Python community.

---

# 1.11 Understanding Tabular Data

Pandas organizes information into **rows** and **columns**.

Example:

| Roll | Name    | Age | Marks |
| ---- | ------- | --- | ----- |
| 1    | Alice   | 20  | 89    |
| 2    | Bob     | 21  | 78    |
| 3    | Charlie | 22  | 91    |

### Row

Represents one observation (or record).

Row 1

```
1 Alice 20 89
```

---

### Column

Represents one feature (or variable).

Example:

```
Age

20

21

22
```

---

# 1.12 Basic Terminology

| Term      | Meaning                                              |
| --------- | ---------------------------------------------------- |
| Dataset   | Complete collection of data                          |
| Row       | One record or observation                            |
| Column    | One feature or variable                              |
| Index     | Labels identifying rows                              |
| Series    | One-dimensional labeled array                        |
| DataFrame | Two-dimensional labeled table                        |
| Cell      | Single value at the intersection of a row and column |

---

# 1.13 Why Learn Pandas Before Machine Learning?

Consider a dataset with:

```
500,000 rows

35 columns
```

Before training a model, you need to:

* Remove duplicates
* Handle missing values
* Correct incorrect data types
* Convert categorical values to numeric
* Create new features
* Remove unnecessary columns
* Explore the data

Almost all of these tasks are performed with Pandas.

A common saying in Data Science is:

> **"Most of a Data Scientist's time is spent cleaning and preparing data, not training models."**

Pandas is the primary tool for that preparation.

---
.
