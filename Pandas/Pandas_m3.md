 Module 3 — DataFrame (Complete)

---

# 🎯 Learning Objectives

After this module, you will be able to:

* Understand what a DataFrame is
* Create DataFrames in different ways
* Understand rows, columns and index
* Access and modify data
* Add/Delete/Rename columns
* Understand DataFrame attributes
* Copy DataFrames correctly
* Export DataFrames
* Use DataFrames in ML projects

---

# 3.1 What is a DataFrame?

A **DataFrame** is a **2-dimensional labeled data structure**.

Think of it as an Excel spreadsheet.

Example

| Roll | Name  | Age | Marks |
| ---- | ----- | --- | ----- |
| 101  | Rahul | 20  | 85    |
| 102  | Amit  | 22  | 90    |
| 103  | Neha  | 21  | 88    |

This entire table is one DataFrame.

---

## Definition

A DataFrame consists of:

* Rows
* Columns
* Index
* Values
* Data Types

Visualization

```text
               DataFrame

        Columns
        ↓

      Name   Age   Marks

0     Rahul   20     85

1      Amit   22     90

2      Neha   21     88

↑
Index
```

---

# Why is DataFrame Important?

Every ML dataset looks like this.

Example

| Age | Salary | Experience | Purchased |
| --- | ------ | ---------- | --------- |
| 22  | 35000  | 1          | 0         |
| 25  | 50000  | 3          | 1         |
| 28  | 70000  | 5          | 1         |

Each row = One observation

Each column = One feature

The whole dataset = One DataFrame

---

# 3.2 Relationship Between Series and DataFrame

Remember:

A **Series** is one column.

A **DataFrame** is multiple Series combined together.

Visualization

```text
Series

Age

20

22

21

+

Series

Marks

85

90

88

↓

DataFrame

Name   Age   Marks

Rahul  20     85

Amit   22     90

Neha   21     88
```

This is why understanding Series first is important.

---

# 3.3 Creating DataFrames

There are several ways.

---

## Method 1 — Dictionary ⭐⭐⭐⭐⭐

Most common.

```python
import pandas as pd

data = {

    "Name":["Rahul","Amit","Neha"],

    "Age":[20,22,21],

    "Marks":[85,90,88]

}

df = pd.DataFrame(data)

print(df)
```

Output

```text
    Name  Age  Marks

0 Rahul   20     85

1 Amit    22     90

2 Neha    21     88
```

Each dictionary key becomes a column.

---

## Method 2 — List of Lists

```python
data = [

["Rahul",20,85],

["Amit",22,90],

["Neha",21,88]

]

df = pd.DataFrame(

data,

columns=["Name","Age","Marks"]

)
```

Useful when data already exists as nested lists.

---

## Method 3 — List of Dictionaries

```python
data = [

{"Name":"Rahul","Age":20},

{"Name":"Amit","Age":22},

{"Name":"Neha","Age":21}

]

df = pd.DataFrame(data)
```

Common when working with JSON or API responses.

---

## Method 4 — NumPy Array

```python
import numpy as np

arr = np.array([

[20,85],

[22,90],

[21,88]

])

df = pd.DataFrame(

arr,

columns=["Age","Marks"]

)
```

Frequently used in ML.

---

## Method 5 — Multiple Series

```python
name = pd.Series(["Rahul","Amit"])

age = pd.Series([20,22])

df = pd.DataFrame({

"Name":name,

"Age":age

})
```

---

# 3.4 DataFrame Attributes

These provide information about the dataset.

---

## shape ⭐⭐⭐⭐⭐

Most frequently used.

```python
df.shape
```

Output

```text
(3,3)
```

Meaning

```text
3 Rows

3 Columns
```

---

## columns

```python
df.columns
```

Output

```text
Index(['Name','Age','Marks'])
```

Useful for checking column names.

---

## index

```python
df.index
```

Output

```text
RangeIndex(start=0, stop=3, step=1)
```

---

## dtypes ⭐⭐⭐⭐⭐

Shows datatype of every column.

```python
df.dtypes
```

Output

```text
Name      object

Age        int64

Marks      int64
```

Very important before ML preprocessing.

---

## size

```python
df.size
```

Output

```text
9
```

Because

```text
3 × 3 = 9
```

Total elements.

---

## ndim

```python
df.ndim
```

Output

```text
2
```

Because DataFrame is two-dimensional.

---

## values

```python
df.values
```

Returns

```text
NumPy Array
```

Useful when integrating with numerical libraries, though many ML libraries also accept DataFrames directly.

---

# 3.5 Selecting Columns ⭐⭐⭐⭐⭐

Single column

```python
df["Age"]
```

Output

```text
20

22

21
```

Notice

The result is a **Series**.

---

Multiple columns

```python
df[["Name","Marks"]]
```

Output

```text
Name

Marks
```

Result is a **DataFrame**.

---

Interview Question ⭐⭐⭐⭐⭐

Difference?

```python
df["Age"]
```

↓

Series

```python
df[["Age"]]
```

↓

DataFrame

---

# 3.6 Adding Columns

Suppose

```text
Name Age

Rahul 20

Amit 22
```

Add Salary.

```python
df["Salary"] = [

50000,

60000,

70000

]
```

Output

| Name  | Age | Salary |
| ----- | --- | ------ |
| Rahul | 20  | 50000  |
| Amit  | 22  | 60000  |
| Neha  | 21  | 70000  |

---

# Derived Columns

Create new features.

```python
df["Bonus"] = df["Salary"] * 0.10
```

Output

| Salary | Bonus |
| ------ | ----- |
| 50000  | 5000  |
| 60000  | 6000  |

This is called **Feature Engineering**.

Very common in ML.

---

# insert()

Insert at specific location.

```python
df.insert(

1,

"Gender",

["M","M","F"]

)
```

Position

```text
0 Name

1 Gender

2 Age

3 Marks
```

---

# 3.7 Renaming Columns

```python
df.rename(

columns={

"Marks":"Score"

}

)
```

Output

```text
Score
```

instead of

```text
Marks
```

⚠️ By default, `rename()` returns a new DataFrame. Use `inplace=True` or assign the result back if you want to keep the change.

---

# 3.8 Deleting Columns

```python
df.drop(

"Salary",

axis=1

)
```

Why

```python
axis=1
```

?

Because

```text
axis=0

↓

Rows

axis=1

↓

Columns
```

---

Delete multiple

```python
df.drop(

["Salary","Bonus"],

axis=1

)
```

---

# Delete Rows

```python
df.drop(

0,

axis=0

)
```

Removes first row.

---

# 3.9 Transpose

Rows become columns.

```python
df.T
```

Before

```text
Rahul

20

85
```

After

```text
Name Rahul

Age 20

Marks 85
```

Useful in certain analyses.

---

# 3.10 Copying DataFrames ⭐⭐⭐⭐⭐

Very important.

Suppose

```python
df2 = df
```

Question

Did Pandas create another DataFrame?

No.

Both variables refer to the **same object**.

Visualization

```text
df

↓

Memory

↑

df2
```

Modify

```python
df2["Age"] = 100
```

Now

```python
df
```

also changes.

---

Correct way

```python
df2 = df.copy()
```

Now

```text
df

↓

Object 1


df2

↓

Object 2
```

Independent copies.

---

Interview Question ⭐⭐⭐⭐⭐

Difference

```python
df2 = df
```

vs

```python
df2 = df.copy()
```

Answer

Assignment shares the same object; `copy()` creates a new independent DataFrame.

---

# 3.11 Exporting Data

CSV

```python
df.to_csv(

"students.csv",

index=False

)
```

Excel

```python
df.to_excel(

"students.xlsx",

index=False

)
```

JSON

```python
df.to_json(

"students.json"
)
```

After preprocessing, exporting cleaned data is common.

---

# 3.12 ML Example

Dataset

| Age | Salary | Purchased |
| --- | ------ | --------- |
| 22  | 35000  | 0         |
| 25  | 50000  | 1         |
| 28  | 70000  | 1         |

Load

```python
df = pd.read_csv(

"customers.csv"
)
```

Create feature

```python
df["Income_per_Age"] = (

df["Salary"] /

df["Age"]

)
```

Drop unnecessary column

```python
df = df.drop(

"CustomerID",

axis=1

)
```

The resulting DataFrame is now closer to being ready for ML.

---

# Common Mistakes

### ❌ Forgetting double brackets

Wrong

```python
df["Age","Salary"]
```

Correct

```python
df[["Age","Salary"]]
```

---

### ❌ Using

```python
df2 = df
```

instead of

```python
df.copy()
```

when an independent copy is required.

---

### ❌ Forgetting

```python
axis=1
```

when dropping columns.

---

### ❌ Assuming `rename()` modifies the DataFrame automatically

It returns a new DataFrame unless you assign the result or use `inplace=True`.

---

# Interview Questions

### 1. What is a DataFrame?

A two-dimensional labeled tabular data structure.

---

### 2. Difference between Series and DataFrame?

| Series          | DataFrame        |
| --------------- | ---------------- |
| One column      | Multiple columns |
| One-dimensional | Two-dimensional  |

---

### 3. Difference between

```python
df["Age"]
```

and

```python
df[["Age"]]
```

* First returns a **Series**.
* Second returns a **DataFrame**.

---

### 4. Difference between

```python
df2 = df
```

and

```python
df2 = df.copy()
```

* Assignment shares the same object.
* `copy()` creates an independent object.

---

### 5. Why is `shape` important?

It tells you how many rows and columns your dataset has.

---

# Practice Exercises

### Exercise 1

Create a DataFrame with:

| Name  | Age | Salary |
| ----- | --: | -----: |
| Rahul |  20 |  50000 |
| Amit  |  22 |  60000 |
| Neha  |  21 |  70000 |

---

### Exercise 2

Print:

* `shape`
* `columns`
* `dtypes`
* `size`
* `ndim`

---

### Exercise 3

Select:

* Only the `Age` column.
* `Age` and `Salary` together.

---

### Exercise 4

Add a new column:

```text
Bonus

10% of Salary
```

---

### Exercise 5

Rename:

```text
Salary

↓

Income
```

---

### Exercise 6

Delete the `Bonus` column.

---

### Exercise 7

Create a copy of the DataFrame, modify the copy, and verify that the original remains unchanged.

---

