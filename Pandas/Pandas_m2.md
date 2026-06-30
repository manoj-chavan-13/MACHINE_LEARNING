# Module 2 — Series (Complete)

---

# Learning Objectives

After completing this module, you will be able to:

* Understand what a Series is.
* Create Series in multiple ways.
* Understand Index and Values.
* Access and modify elements.
* Perform vectorized operations.
* Handle missing values.
* Filter data.
* Apply transformations.
* Perform statistical analysis.
* Convert data types.
* Use Series in ML preprocessing.

---

# 2.1 What is a Series?

A **Series** is a **one-dimensional labeled array**.

It consists of:

* Values
* Index (labels)

Visualization:

```text
Index      Value

0          Rahul

1          Amit

2          Neha

3          Priya
```

Think of it as **one column of an Excel sheet**.

---

## Real Life Example

Excel

| Name  |
| ----- |
| Rahul |
| Amit  |
| Neha  |

In Pandas

```python
import pandas as pd

names = pd.Series(["Rahul", "Amit", "Neha"])
```

That single column is a Series.

---

# Internal Structure

Every Series has three important parts.

```text
Series

│

├── Values

├── Index

└── dtype
```

Example

```python
s = pd.Series([10,20,30])
```

Internally

```text
Index

0

1

2


Values

10

20

30


dtype

int64
```

---

# Why is Series Important?

Suppose a hospital has:

| Name | Age | Weight | Blood Group |
| ---- | --: | -----: | ----------- |
| A    |  20 |     60 | A+          |
| B    |  25 |     70 | O+          |
| C    |  30 |     65 | B+          |

Sometimes we only need one feature.

Example:

Age

```text
20

25

30
```

That is a Series.

---

# 2.2 Creating a Series

There are several ways.

---

## Method 1 — From a List ⭐⭐⭐⭐⭐

Most common.

```python
import pandas as pd

s = pd.Series([10,20,30,40])

print(s)
```

Output

```text
0    10
1    20
2    30
3    40
dtype: int64
```

Pandas automatically creates the index.

---

## Method 2 — Custom Index ⭐⭐⭐⭐⭐

```python
salary = pd.Series(
    [50000,60000,70000],
    index=["EMP101","EMP102","EMP103"]
)

print(salary)
```

Output

```text
EMP101    50000
EMP102    60000
EMP103    70000
dtype: int64
```

Instead of numeric indexes, we now use employee IDs.

---

## Why Use Custom Index?

Suppose you want Rahul's salary.

With default index:

```python
salary[0]
```

Not very meaningful.

With custom index:

```python
salary["EMP101"]
```

Much clearer.

---

## Method 3 — From Dictionary ⭐⭐⭐⭐⭐

```python
salary = {
    "Rahul":50000,
    "Amit":60000,
    "Neha":70000
}

s = pd.Series(salary)

print(s)
```

Output

```text
Rahul    50000
Amit     60000
Neha     70000
dtype: int64
```

Notice:

Dictionary Keys → Index

Dictionary Values → Series Values

---

## Method 4 — From NumPy Array ⭐⭐⭐⭐

```python
import numpy as np

arr = np.array([10,20,30])

s = pd.Series(arr)
```

Since Pandas is built on NumPy, this is common in ML workflows.

---

## Method 5 — Scalar Value ⭐⭐⭐

```python
s = pd.Series(
    100,
    index=["A","B","C"]
)

print(s)
```

Output

```text
A    100
B    100
C    100
```

The scalar value is repeated for each index.

Useful for initializing columns.

---

# 2.3 Series Attributes

Attributes describe a Series.

---

## index

Returns row labels.

```python
s.index
```

Output

```text
Index(['A','B','C'])
```

---

## values

Returns the underlying NumPy array.

```python
s.values
```

Output

```text
array([100,100,100])
```

---

## dtype

Returns data type.

```python
s.dtype
```

Output

```text
int64
```

---

## shape

```python
s.shape
```

Output

```text
(3,)
```

Meaning:

3 elements

1 dimension

---

## size

```python
s.size
```

Output

```text
3
```

---

## ndim

```python
s.ndim
```

Output

```text
1
```

Series is always one-dimensional.

---

## name

A Series can have a name.

```python
age = pd.Series(
    [20,21,22],
    name="Age"
)

print(age)
```

Output

```text
0    20
1    21
2    22
Name: Age
dtype: int64
```

Useful when converting a Series to a DataFrame.

---

# 2.4 Accessing Data

---

## By Position

```python
s = pd.Series([10,20,30])

print(s[0])
```

Output

```text
10
```

---

## By Label

```python
s = pd.Series(
    [10,20,30],
    index=["A","B","C"]
)

print(s["A"])
```

Output

```text
10
```

---

# Slicing

Just like Python lists.

```python
s = pd.Series([10,20,30,40,50])

print(s[1:4])
```

Output

```text
1    20
2    30
3    40
```

---

Using labels

```python
s = pd.Series(
    [10,20,30,40],
    index=["A","B","C","D"]
)

print(s["A":"C"])
```

Output

```text
A    10
B    20
C    30
```

⚠️ Label slicing includes the ending label.

---

# 2.5 Vectorized Operations ⭐⭐⭐⭐⭐

One of the biggest advantages of Pandas.

Suppose

```python
age = pd.Series([20,25,30])
```

Increase everyone's age by 1.

Instead of

```python
new = []

for i in age:
    new.append(i + 1)
```

Use

```python
age + 1
```

Output

```text
21
26
31
```

No loop.

Cleaner.

Faster.

---

## Mathematical Operators

```python
s + 5
```

Adds 5 to every value.

---

```python
s - 5
```

Subtracts 5.

---

```python
s * 2
```

Multiplies every value.

---

```python
s / 2
```

Divides every value.

---

```python
s ** 2
```

Squares every value.

---

# 2.6 Automatic Index Alignment ⭐⭐⭐⭐⭐

This is a unique feature of Pandas.

```python
math = pd.Series(
    [90,80,70],
    index=["Rahul","Amit","Neha"]
)

science = pd.Series(
    [85,95,88],
    index=["Rahul","Neha","Amit"]
)
```

Now

```python
math + science
```

Output

```text
Rahul    175
Amit     168
Neha     165
```

Notice:

Pandas matches labels, not positions.

This prevents many data-matching errors.

---

# 2.7 Boolean Indexing ⭐⭐⭐⭐⭐

Filtering data.

```python
salary = pd.Series(
    [25000,40000,80000,100000]
)
```

Find salaries above ₹50,000.

```python
salary[salary > 50000]
```

Output

```text
80000

100000
```

---

Multiple conditions

```python
salary[
    (salary > 30000) &
    (salary < 90000)
]
```

Output

```text
40000

80000
```

---

# 2.8 Missing Values ⭐⭐⭐⭐⭐

Machine Learning datasets almost always contain missing data.

```python
import numpy as np

s = pd.Series(
    [10,20,np.nan,40]
)
```

Output

```text
10

20

NaN

40
```

---

Detect

```python
s.isna()
```

Count

```python
s.isna().sum()
```

Remove

```python
s.dropna()
```

Fill

```python
s.fillna(0)
```

Mean

```python
s.fillna(s.mean())
```

Median

```python
s.fillna(s.median())
```

---

# 2.9 Statistics ⭐⭐⭐⭐⭐

Most commonly used methods:

```python
s.sum()
```

```python
s.mean()
```

```python
s.median()
```

```python
s.mode()
```

```python
s.min()
```

```python
s.max()
```

```python
s.std()
```

```python
s.var()
```

```python
s.describe()
```

---

# 2.10 Sorting ⭐⭐⭐⭐

Sort values.

```python
s.sort_values()
```

Descending

```python
s.sort_values(
    ascending=False
)
```

Sort by index

```python
s.sort_index()
```

---

# 2.11 Useful Functions ⭐⭐⭐⭐⭐

Unique values

```python
s.unique()
```

Count unique

```python
s.nunique()
```

Frequency

```python
s.value_counts()
```

Ranking

```python
s.rank()
```

Check membership

```python
s.isin([20,30])
```

Range checking

```python
s.between(20,40)
```

---

# 2.12 Data Transformation ⭐⭐⭐⭐⭐

## map()

Maps values.

```python
grade = pd.Series(["A","B","A"])

grade.map({
    "A":4,
    "B":3
})
```

Output

```text
4

3

4
```

---

## apply()

Applies a function to each element.

```python
age.apply(lambda x: x + 5)
```

---

Difference

| `map()`                       | `apply()`          |
| ----------------------------- | ------------------ |
| Mapping values                | Apply any function |
| Faster for dictionary lookups | More flexible      |

---

## replace()

```python
grade.replace(
    "A",
    "Excellent"
)
```

---

## astype()

Change datatype.

```python
age.astype(float)
```

---

# 2.13 Memory Usage

For large datasets, memory matters.

```python
s.memory_usage(deep=True)
```

Useful for optimizing large ML datasets.

---

# ML Example

Suppose you have:

```python
age = pd.Series([20, None, 30, 25])
```

Clean it:

```python
age = age.fillna(age.median())
age = age.astype(int)
```

Now the feature is ready for use in a Machine Learning model.

---

# Common Mistakes

❌ Using loops instead of vectorized operations.

❌ Ignoring missing values before training.

❌ Confusing `map()` and `apply()`.

❌ Forgetting that Pandas aligns Series by index.

---

# Interview Questions

### 1. What is a Series?

A one-dimensional labeled array in Pandas.

---

### 2. Difference between `map()` and `apply()`?

`map()` is primarily for replacing or mapping values (often with a dictionary or another Series), while `apply()` applies a custom function to each element.

---

### 3. Why is vectorization faster than loops?

Because Pandas leverages optimized operations (largely through NumPy) instead of executing Python-level loops element by element.

---

### 4. What is automatic index alignment?

When operating on multiple Series, Pandas matches values using their **index labels**, not just their positions.

---

### 5. Difference between `unique()` and `nunique()`?

* `unique()` returns the distinct values.
* `nunique()` returns the number of distinct values.

---

# Practice Exercises

### Exercise 1

Create a Series from:

* List
* Dictionary
* NumPy array

---

### Exercise 2

Create a custom index.

---

### Exercise 3

Filter values greater than 50.

---

### Exercise 4

Replace missing values using:

* Mean
* Median

---

### Exercise 5

Convert the Series from `int` to `float`.

---

### Exercise 6

Count the frequency of values using `value_counts()`.

