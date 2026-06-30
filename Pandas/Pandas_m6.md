# Module 6 — Indexing (Complete)

---

# 🎯 Learning Objectives

After this module, you will be able to:

* Select rows and columns correctly.
* Understand `loc`, `iloc`, `at`, and `iat`.
* Filter data using conditions.
* Combine multiple conditions.
* Select subsets of data.
* Understand label-based vs position-based indexing.
* Avoid common indexing mistakes.

---

# Sample Dataset

We'll use this DataFrame throughout the module.

```python
import pandas as pd

df = pd.DataFrame({
    "Name": ["Rahul","Amit","Neha","Priya","John"],
    "Age": [22,25,21,24,26],
    "Salary": [50000,65000,55000,70000,60000],
    "Department": ["IT","HR","IT","Sales","HR"]
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
|     4 | John  |  26 |  60000 | HR         |

---

# Why Indexing Matters

Suppose your dataset has

```text
1,000,000 rows
```

You don't always need every row or every column.

Maybe you only need:

* IT employees
* Salary column
* First 100 rows
* Age > 25

Indexing allows you to retrieve exactly the data you need.

---

# Types of Indexing

Pandas provides four primary indexers.

| Indexer | Purpose                                   |
| ------- | ----------------------------------------- |
| `loc`   | Label-based selection                     |
| `iloc`  | Position-based selection                  |
| `at`    | Fast access to one value (label-based)    |
| `iat`   | Fast access to one value (position-based) |

---

# 6.1 Selecting Columns

## Single Column

```python
df["Age"]
```

Output

```text
0    22
1    25
2    21
3    24
4    26
```

Notice:

Return type:

```text
Series
```

---

## Multiple Columns

```python
df[["Name","Salary"]]
```

Output

| Name  | Salary |
| ----- | -----: |
| Rahul |  50000 |
| Amit  |  65000 |
| Neha  |  55000 |
| Priya |  70000 |
| John  |  60000 |

Return type:

```text
DataFrame
```

---

### Interview Question ⭐⭐⭐⭐⭐

Difference between

```python
df["Age"]
```

and

```python
df[["Age"]]
```

Answer

```text
Single bracket

↓

Series

Double bracket

↓

DataFrame
```

---

# 6.2 loc (Label-Based Indexing) ⭐⭐⭐⭐⭐

## What is loc?

`loc` selects rows and columns using **labels**.

Syntax

```python
df.loc[row_label, column_label]
```

Think of it as:

```text
DataFrame

↓

Row Label

↓

Column Label
```

---

## Select One Row

```python
df.loc[2]
```

Output

```text
Name          Neha
Age             21
Salary       55000
Department      IT
```

Because

```text
Index = 2
```

---

## Select Multiple Rows

```python
df.loc[[1,3]]
```

Output

| Name  | Age |
| ----- | --: |
| Amit  |  25 |
| Priya |  24 |

---

## Select Range of Rows

```python
df.loc[1:3]
```

Output

| Name  |
| ----- |
| Amit  |
| Neha  |
| Priya |

⚠️ Important

With **`loc`**, the **ending label is included**.

This differs from normal Python slicing.

---

## Select One Cell

```python
df.loc[1, "Salary"]
```

Output

```text
65000
```

Visualization

```text
Row 1

↓

Salary

↓

65000
```

---

## Select Multiple Columns

```python
df.loc[:, ["Name","Salary"]]
```

Meaning

```text
:

↓

All rows

Name

Salary

↓

Columns
```

Output

| Name  | Salary |
| ----- | -----: |
| Rahul |  50000 |
| Amit  |  65000 |
| Neha  |  55000 |
| Priya |  70000 |
| John  |  60000 |

---

## Select Rows and Columns Together

```python
df.loc[1:3, ["Name","Salary"]]
```

Output

| Name  | Salary |
| ----- | -----: |
| Amit  |  65000 |
| Neha  |  55000 |
| Priya |  70000 |

---

# 6.3 iloc (Position-Based Indexing) ⭐⭐⭐⭐⭐

`iloc` selects by **integer position**.

Syntax

```python
df.iloc[row_position, column_position]
```

---

## First Row

```python
df.iloc[0]
```

Output

```text
Rahul
22
50000
IT
```

---

## Multiple Rows

```python
df.iloc[[0,2,4]]
```

---

## Row Range

```python
df.iloc[1:4]
```

Output

Rows

```text
1

2

3
```

Notice

Just like Python,

the ending position is **excluded**.

---

## Select One Cell

```python
df.iloc[2,1]
```

Let's count positions.

Columns

```text
0 Name

1 Age

2 Salary

3 Department
```

Row

```text
2
```

Age

```text
21
```

Output

```text
21
```

---

## Select Multiple Columns

```python
df.iloc[:, [0,2]]
```

Output

| Name  | Salary |
| ----- | -----: |
| Rahul |  50000 |
| Amit  |  65000 |
| Neha  |  55000 |
| Priya |  70000 |
| John  |  60000 |

---

# loc vs iloc ⭐⭐⭐⭐⭐

This is one of the most common interview questions.

| loc                   | iloc                   |
| --------------------- | ---------------------- |
| Label-based           | Position-based         |
| Includes end label    | Excludes end position  |
| Uses row/column names | Uses integer positions |

Example

```python
df.loc[1:3]
```

Returns

```text
1

2

3
```

But

```python
df.iloc[1:3]
```

Returns

```text
1

2
```

This difference is extremely important.

---

# 6.4 at ⭐⭐⭐⭐

Used for accessing **one value** quickly.

```python
df.at[1, "Salary"]
```

Output

```text
65000
```

Why use `at`?

* Faster than `loc` when retrieving or updating a single value.

---

# 6.5 iat ⭐⭐⭐⭐

Same idea,

but uses positions.

```python
df.iat[1,2]
```

Output

```text
65000
```

---

Difference

| at    | iat      |
| ----- | -------- |
| Label | Position |

---

# 6.6 Boolean Indexing ⭐⭐⭐⭐⭐

One of the most important ML concepts.

Suppose

```python
df["Age"] > 23
```

Output

```text
0 False

1 True

2 False

3 True

4 True
```

Now

```python
df[df["Age"] > 23]
```

Output

| Name  | Age |
| ----- | --: |
| Amit  |  25 |
| Priya |  24 |
| John  |  26 |

---

Internally

Step 1

```text
False

True

False

True

True
```

Step 2

Keep only

```text
True
```

rows.

---

# Multiple Conditions ⭐⭐⭐⭐⭐

Employees

Age > 23

AND

Salary > 60000

```python
df[
    (df["Age"] > 23) &
    (df["Salary"] > 60000)
]
```

Output

| Name  | Salary |
| ----- | -----: |
| Amit  |  65000 |
| Priya |  70000 |

---

## OR Condition

```python
df[
    (df["Department"]=="IT") |
    (df["Salary"]>65000)
]
```

Output

IT employees plus anyone earning above 65000.

---

## NOT Condition

```python
df[
    ~(df["Department"]=="HR")
]
```

Returns everyone except HR employees.

---

# isin() ⭐⭐⭐⭐

Instead of writing

```python
Department == "IT"

OR

Department == "HR"
```

Use

```python
df[
    df["Department"].isin(
        ["IT","HR"]
    )
]
```

Cleaner and easier to read.

---

# between() ⭐⭐⭐⭐

Find salaries between

50000

and

65000

```python
df[
    df["Salary"].between(
        50000,
        65000
    )
]
```

Equivalent to

```python
(df["Salary"]>=50000) &
(df["Salary"]<=65000)
```

---

# Query Method ⭐⭐⭐

Another way to filter.

```python
df.query("Age > 23")
```

Multiple conditions

```python
df.query(
    "Age > 23 and Salary > 60000"
)
```

Readable for simple conditions, though understanding boolean indexing is more fundamental.

---

# Practical ML Examples

### Example 1

Find all customers older than 30.

```python
df[df["Age"] > 30]
```

---

### Example 2

Find employees from IT.

```python
df[
    df["Department"]=="IT"
]
```

---

### Example 3

Find customers earning above ₹50,000.

```python
df[
    df["Salary"]>50000
]
```

---

### Example 4

Select only

Age

Salary

```python
df[
    ["Age","Salary"]
]
```

---

### Example 5

First 100 rows

```python
df.iloc[:100]
```

Very common for testing ML pipelines.

---

# Common Mistakes

### ❌ Using `and`

Wrong

```python
df[
    df["Age"]>23 and
    df["Salary"]>50000
]
```

Correct

```python
df[
    (df["Age"]>23) &
    (df["Salary"]>50000)
]
```

`and` works with single Boolean values, not entire Series.

---

### ❌ Forgetting parentheses

Wrong

```python
df[
    df["Age"]>23 &
    df["Salary"]>50000
]
```

Correct

```python
df[
    (df["Age"]>23) &
    (df["Salary"]>50000)
]
```

---

### ❌ Confusing `loc` and `iloc`

Remember

```text
loc

↓

Labels

iloc

↓

Integer Positions
```

---

### ❌ Forgetting

```python
df[["Age","Salary"]]
```

instead of

```python
df["Age","Salary"]
```

---

# Interview Questions

### 1. Difference between `loc` and `iloc`?

* `loc` uses labels.
* `iloc` uses integer positions.

---

### 2. Difference between `at` and `loc`?

* `at` is optimized for accessing or updating a single value.
* `loc` is more general and can select multiple rows and columns.

---

### 3. Why are parentheses required in boolean indexing?

Because comparison operators and bitwise operators (`&`, `|`) have different precedence, and parentheses ensure each condition is evaluated correctly.

---

### 4. Difference between `&` and `and`?

* `&` performs element-wise logical AND on Series.
* `and` works only with single Boolean values.

---

### 5. Difference between `isin()` and multiple OR conditions?

`isin()` is more concise and readable when checking membership in a list of values.

---

# Practice Exercises

Using the sample DataFrame:

1. Select only the `Age` column.
2. Select `Name` and `Salary`.
3. Retrieve the third row using `loc`.
4. Retrieve the third row using `iloc`.
5. Get the salary of Rahul using `loc`.
6. Get the salary of Rahul using `at`.
7. Display employees with salary greater than 55,000.
8. Display IT employees.
9. Display employees aged between 22 and 25.
10. Display employees whose department is IT or HR using `isin()`.

---

