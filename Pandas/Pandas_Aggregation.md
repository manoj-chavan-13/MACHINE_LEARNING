
# 📘 Pandas for Machine Learning

# Module 12 — GroupBy & Aggregation (Complete)

---

# 🎯 Learning Objectives

After completing this module, you will be able to:

* Understand Split-Apply-Combine
* Use `groupby()`
* Perform aggregations
* Group by multiple columns
* Use `agg()`
* Use `transform()`
* Use `filter()`
* Perform custom aggregations
* Create group-based ML features

---

# What is GroupBy?

Suppose we have employee data.

| Name  | Department | Salary |
| ----- | ---------- | ------ |
| Rahul | IT         | 50000  |
| Amit  | HR         | 65000  |
| Neha  | IT         | 55000  |
| Priya | Sales      | 70000  |
| John  | HR         | 60000  |

Question:

> What is the **average salary of each department?**

Instead of calculating manually,

Pandas does it using

```python
groupby()
```

---

# Split → Apply → Combine

Every `groupby()` operation follows this process.

```text
Original Data
      │
      ▼
Split into Groups
      │
      ▼
Apply Function
(mean, sum, count...)
      │
      ▼
Combine Results
```

Example

```text
IT
50000
55000

↓

Mean

↓

52500
```

---

# Sample Dataset

We'll use this DataFrame throughout the module.

```python
import pandas as pd

df = pd.DataFrame({

    "Employee":[
        "Rahul",
        "Amit",
        "Neha",
        "Priya",
        "John",
        "Riya"
    ],

    "Department":[
        "IT",
        "HR",
        "IT",
        "Sales",
        "HR",
        "Sales"
    ],

    "Age":[
        22,
        25,
        21,
        24,
        26,
        23
    ],

    "Salary":[
        50000,
        65000,
        55000,
        70000,
        60000,
        75000
    ]
})

print(df)
```

Output

| Employee | Department | Age | Salary |
| -------- | ---------- | --: | -----: |
| Rahul    | IT         |  22 |  50000 |
| Amit     | HR         |  25 |  65000 |
| Neha     | IT         |  21 |  55000 |
| Priya    | Sales      |  24 |  70000 |
| John     | HR         |  26 |  60000 |
| Riya     | Sales      |  23 |  75000 |

---

# 12.1 groupby() ⭐⭐⭐⭐⭐

Basic syntax

```python
df.groupby("Department")
```

This creates a **GroupBy object**.

Nothing is calculated yet.

You must apply an aggregation.

---

# Mean Salary by Department

```python
df.groupby("Department")["Salary"].mean()
```

Output

| Department | Mean Salary |
| ---------- | ----------: |
| HR         |       62500 |
| IT         |       52500 |
| Sales      |       72500 |

---

## Understanding Internally

```text
IT

50000
55000

↓

Mean

↓

52500

--------------------

HR

65000
60000

↓

Mean

↓

62500
```

---

# Sum

```python
df.groupby("Department")["Salary"].sum()
```

Output

| Department | Salary |
| ---------- | -----: |
| HR         | 125000 |
| IT         | 105000 |
| Sales      | 145000 |

---

# Count

```python
df.groupby("Department")["Salary"].count()
```

Output

| Department | Employees |
| ---------- | --------: |
| HR         |         2 |
| IT         |         2 |
| Sales      |         2 |

---

# Min

```python
df.groupby("Department")["Salary"].min()
```

---

# Max

```python
df.groupby("Department")["Salary"].max()
```

---

# Median

```python
df.groupby("Department")["Salary"].median()
```

---

# Standard Deviation

```python
df.groupby("Department")["Salary"].std()
```

Useful for measuring salary variation within each department.

---

# 12.2 Multiple Aggregations ⭐⭐⭐⭐⭐

Instead of one statistic,

calculate many.

```python
df.groupby("Department")["Salary"].agg(

    ["mean","min","max","sum","count"]

)
```

Output

| Department |  Mean |   Min |   Max |    Sum | Count |
| ---------- | ----: | ----: | ----: | -----: | ----: |
| HR         | 62500 | 60000 | 65000 | 125000 |     2 |
| IT         | 52500 | 50000 | 55000 | 105000 |     2 |
| Sales      | 72500 | 70000 | 75000 | 145000 |     2 |

This is one of the most common interview questions.

---

# 12.3 agg() ⭐⭐⭐⭐⭐

`agg()` allows multiple operations.

Example

```python
df.groupby("Department").agg({

    "Salary":["mean","max"],

    "Age":["mean","min"]

})
```

Output

| Department | Salary Mean | Salary Max | Age Mean | Age Min |
| ---------- | ----------: | ---------: | -------: | ------: |
| HR         |       62500 |      65000 |     25.5 |      25 |
| IT         |       52500 |      55000 |     21.5 |      21 |

---

# Named Aggregation ⭐⭐⭐⭐

Cleaner column names.

```python
df.groupby("Department").agg(

    AverageSalary=("Salary","mean"),

    MaximumSalary=("Salary","max"),

    AverageAge=("Age","mean")

)
```

Output

| Department | AverageSalary | MaximumSalary | AverageAge |
| ---------- | ------------: | ------------: | ---------: |
| HR         |         62500 |         65000 |       25.5 |

Recommended for production code.

---

# 12.4 Grouping Multiple Columns ⭐⭐⭐⭐⭐

Suppose

Dataset

| Department | Gender | Salary |
| ---------- | ------ | -----: |
| IT         | Male   |  50000 |
| IT         | Female |  55000 |

Group

```python
df.groupby(

    ["Department","Gender"]

)["Salary"].mean()
```

Output

```text
Department   Gender

HR           Male

IT           Female

IT           Male

Sales        Female
```

Useful for multi-dimensional analysis.

---

# 12.5 size() vs count() ⭐⭐⭐⭐⭐

Very common interview question.

Suppose

| Department | Salary |
| ---------- | ------ |
| IT         | 50000  |
| IT         | NaN    |

```python
df.groupby("Department").size()
```

Output

```text
2
```

Counts every row.

---

```python
df.groupby("Department")["Salary"].count()
```

Output

```text
1
```

Counts only non-missing values.

---

Difference

| size()          | count()                |
| --------------- | ---------------------- |
| Counts all rows | Ignores missing values |

---

# 12.6 transform() ⭐⭐⭐⭐⭐

One of the most important functions for ML.

Unlike `agg()`,

it returns the **same number of rows** as the original DataFrame.

Example

Calculate department average salary.

```python
df["DeptAvgSalary"] = (

    df.groupby("Department")["Salary"]

      .transform("mean")

)
```

Output

| Employee | Department | Salary | DeptAvgSalary |
| -------- | ---------- | -----: | ------------: |
| Rahul    | IT         |  50000 |         52500 |
| Neha     | IT         |  55000 |         52500 |

Notice

Every row receives its department's average.

---

## Why is transform() Important?

Because it creates **new ML features**.

Example

Difference from department average

```python
df["SalaryDiff"] = (

    df["Salary"]

    -

    df.groupby("Department")["Salary"]

      .transform("mean")

)
```

Output

| Salary | DeptAvgSalary | SalaryDiff |
| -----: | ------------: | ---------: |
|  50000 |         52500 |      -2500 |
|  55000 |         52500 |       2500 |

This is a powerful feature engineering technique.

---

# transform() vs agg()

| agg()        | transform()         |
| ------------ | ------------------- |
| Reduced rows | Same number of rows |
| Summary      | Feature creation    |

---

# 12.7 filter() ⭐⭐⭐⭐

Keep only groups satisfying a condition.

Example

Departments with average salary > 60,000

```python
df.groupby("Department").filter(

    lambda x:

    x["Salary"].mean()>60000

)
```

Output

Only

```text
HR

Sales
```

employees remain.

---

# 12.8 apply() on Groups ⭐⭐⭐⭐

Apply a custom function to each group.

```python
def salary_range(group):

    return group["Salary"].max() - group["Salary"].min()

df.groupby("Department").apply(salary_range)
```

Output

| Department | Range |
| ---------- | ----: |
| HR         |  5000 |
| IT         |  5000 |
| Sales      |  5000 |

Use `apply()` when built-in aggregations are insufficient.

---

# 12.9 get_group()

Retrieve a specific group.

```python
grouped = df.groupby("Department")
```

Retrieve IT employees

```python
grouped.get_group("IT")
```

Output

Only IT rows.

---

# 12.10 groups

Display group indices.

```python
grouped.groups
```

Output

```text
{

'HR':[1,4],

'IT':[0,2],

'Sales':[3,5]

}
```

Useful for understanding how Pandas partitioned the data.

---

# Practical ML Examples

---

## Average Salary by Department

```python
df.groupby("Department")["Salary"].mean()
```

---

## Department Size

```python
df.groupby("Department").size()
```

---

## Average Age

```python
df.groupby("Department")["Age"].mean()
```

---

## Create Department Mean Salary Feature

```python
df["DeptMeanSalary"] = (

    df.groupby("Department")["Salary"]

      .transform("mean")

)
```

---

## Salary Difference

```python
df["SalaryGap"] = (

df["Salary"]

-

df["DeptMeanSalary"]

)
```

Very common in recommendation systems, HR analytics, banking, and retail.

---

# Best Practices ⭐⭐⭐⭐⭐

✔ Use `agg()` for summaries.

✔ Use `transform()` for feature engineering.

✔ Prefer named aggregations for readability.

✔ Group only by meaningful categorical columns.

✔ Check missing values before grouping.

---

# Common Mistakes

### ❌ Expecting `groupby()` to calculate immediately

```python
df.groupby("Department")
```

returns a GroupBy object.

You still need an aggregation like:

```python
.mean()
```

---

### ❌ Confusing `agg()` and `transform()`

Remember

```text
agg()

↓

Summary Table

transform()

↓

Same Number of Rows
```

---

### ❌ Using `count()` when missing values exist

If you need total rows,

use

```python
.size()
```

---

### ❌ Forgetting `reset_index()`

Many groupby operations return the grouping columns as the index.

Convert back to a regular DataFrame if needed.

```python
result = (
    df.groupby("Department")["Salary"]
      .mean()
      .reset_index()
)
```

---

# Interview Questions

### 1. What is the Split-Apply-Combine strategy?

* **Split** the data into groups.
* **Apply** a function to each group.
* **Combine** the results into a final output.

---

### 2. Difference between `agg()` and `transform()`?

* `agg()` returns aggregated summaries.
* `transform()` returns values aligned with the original rows, making it suitable for feature engineering.

---

### 3. Difference between `size()` and `count()`?

* `size()` counts all rows.
* `count()` counts only non-null values.

---

### 4. Why is `transform()` useful in ML?

It enables creation of group-based features (such as department averages) while preserving the original DataFrame structure.

---

### 5. Why use named aggregations?

They produce clear, descriptive column names instead of MultiIndex column headers.

---

# Practice Exercises

Using the sample DataFrame:

1. Calculate the average salary for each department.
2. Calculate the maximum salary for each department.
3. Find the total salary paid by each department.
4. Compute the employee count for each department.
5. Use `agg()` to calculate mean, min, and max salary.
6. Use named aggregations for average salary and average age.
7. Create a `DeptAvgSalary` column using `transform()`.
8. Create a `SalaryDifference` feature.
9. Filter departments with an average salary greater than ₹60,000.
10. Retrieve only the IT department using `get_group()`.

---

