# 📘 Pandas for Machine Learning

# Module 10 — Feature Engineering (Complete)

---

# 🎯 Learning Objectives

After completing this module, you will be able to:

* Understand feature engineering
* Create new features
* Transform existing features
* Use `map()`
* Use `apply()`
* Use `assign()`
* Use `insert()`
* Use `where()`
* Use `mask()`
* Use `clip()`
* Use `cut()`
* Use `qcut()`
* Build better ML features

---

# What is Feature Engineering?

Feature Engineering means

> **Creating new useful features from existing data.**

Suppose

| Age | Salary |
| --- | ------ |
| 25  | 50000  |

Instead of using only these columns,

create

```text
Salary_per_Age
```

```python
df["Salary_per_Age"] = df["Salary"] / df["Age"]
```

Now the model has more information.

---

# Why Feature Engineering?

Raw data

| JoiningDate |
| ----------- |
| 2023-05-12  |

Model doesn't understand

```text
2023-05-12
```

Create

* Year
* Month
* Day
* Weekday

Now the model learns better.

---

# Sample Dataset

```python
import pandas as pd

df = pd.DataFrame({

    "Name":["Rahul","Amit","Neha","Priya"],

    "Age":[22,25,21,24],

    "Salary":[50000,65000,55000,70000],

    "Department":["IT","HR","IT","Sales"]

})

print(df)
```

Output

| Name  | Age | Salary | Department |
| ----- | --- | ------ | ---------- |
| Rahul | 22  | 50000  | IT         |
| Amit  | 25  | 65000  | HR         |
| Neha  | 21  | 55000  | IT         |
| Priya | 24  | 70000  | Sales      |

---

# 10.1 Creating New Columns ⭐⭐⭐⭐⭐

The simplest form of feature engineering.

```python
df["Bonus"] = df["Salary"] * 0.10
```

Output

| Salary | Bonus |
| ------ | ----- |
| 50000  | 5000  |
| 65000  | 6500  |

---

Create another feature

```python
df["Salary_per_Age"] = df["Salary"] / df["Age"]
```

Very common in ML.

---

# 10.2 assign() ⭐⭐⭐⭐

Creates new columns without modifying the original DataFrame.

```python
new_df = df.assign(

    Bonus=df["Salary"]*0.10

)
```

Original DataFrame

↓

Unchanged

New DataFrame

↓

Contains Bonus.

---

Multiple columns

```python
new_df = df.assign(

    Bonus=df["Salary"]*0.10,

    Tax=df["Salary"]*0.05

)
```

---

Why use `assign()`?

Useful in method chaining:

```python
clean_df = (
    df
      .assign(Bonus=df["Salary"]*0.10)
      .assign(NetSalary=lambda x: x["Salary"]-x["Bonus"])
)
```

---

# 10.3 insert() ⭐⭐⭐⭐

Insert a column at a specific position.

```python
df.insert(

1,

"Gender",

["M","M","F","F"]

)
```

Output

| Name  | Gender | Age |
| ----- | ------ | --- |
| Rahul | M      | 22  |

Position

```text
0 Name

1 Gender

2 Age
```

---

# 10.4 map() ⭐⭐⭐⭐⭐

One of the most important functions.

Maps one value to another.

Suppose

| Department |
| ---------- |
| IT         |
| HR         |
| Sales      |

Convert

```python
department_code = {

    "IT":1,

    "HR":2,

    "Sales":3

}

df["DepartmentCode"] = (
    df["Department"]
        .map(department_code)
)
```

Output

| Department | DepartmentCode |
| ---------- | -------------- |
| IT         | 1              |
| HR         | 2              |
| Sales      | 3              |

---

Why use `map()`?

Commonly used for:

* Label encoding
* Category mapping
* Binary conversion
* Value replacement

---

Example

```python
gender = {

    "Male":1,

    "Female":0

}

df["Gender"] = df["Gender"].map(gender)
```

---

# What if a Value is Missing?

Suppose

```text
Marketing
```

is not in the dictionary.

Output

```text
NaN
```

Always check the mapping dictionary.

---

# 10.5 apply() ⭐⭐⭐⭐⭐

The most flexible transformation method.

Applies a function to every element.

Example

Increase Age

```python
df["Age"].apply(

    lambda x: x+1

)
```

Output

```text
23

26

22

25
```

---

Another example

Salary

```python
df["Salary"].apply(

    lambda x: x*1.1

)
```

Adds a 10% increase.

---

Using your own function

```python
def classify(age):

    if age<25:

        return "Young"

    return "Adult"

df["Category"] = (
    df["Age"]
        .apply(classify)
)
```

Output

| Age | Category |
| --- | -------- |
| 22  | Young    |
| 25  | Adult    |

---

# map() vs apply() ⭐⭐⭐⭐⭐

| map()          | apply()                                     |
| -------------- | ------------------------------------------- |
| Mapping values | Any function                                |
| Dictionary     | Lambda                                      |
| Faster         | More flexible                               |
| One Series     | One Series (or rows/columns for DataFrames) |

Interview Question ⭐⭐⭐⭐⭐

Know this difference.

---

# 10.6 where() ⭐⭐⭐⭐

Keep values satisfying a condition.

Otherwise,

replace them.

Suppose

```python
df["Salary"].where(

    df["Salary"]>55000,

    0

)
```

Output

```text
0

65000

0

70000
```

Think

```text
Condition True

↓

Keep

Condition False

↓

Replace
```

---

# 10.7 mask() ⭐⭐⭐⭐

Opposite of `where()`.

```python
df["Salary"].mask(

    df["Salary"]>55000,

    0

)
```

Output

```text
50000

0

55000

0
```

Difference

| where         | mask         |
| ------------- | ------------ |
| Replace False | Replace True |

---

# 10.8 clip() ⭐⭐⭐⭐⭐

Limits values.

Suppose

Salary

```text
50000

65000

120000

30000
```

Limit

```python
df["Salary"].clip(

    lower=40000,

    upper=80000

)
```

Output

```text
50000

65000

80000

40000
```

Useful for

* Handling outliers
* Winsorization
* Data normalization preparation

---

# 10.9 cut() ⭐⭐⭐⭐⭐

Creates bins.

Suppose

Age

```text
20

25

35

45

60
```

Create categories

```python
df["AgeGroup"] = pd.cut(

    df["Age"],

    bins=[0,18,30,50,100],

    labels=[

        "Child",

        "Young",

        "Adult",

        "Senior"

    ]

)
```

Output

| Age | Group |
| --- | ----- |
| 25  | Young |

---

ML Use

Many models benefit from categorical age groups instead of raw ages.

---

# 10.10 qcut() ⭐⭐⭐⭐⭐

Similar to

```python
cut()
```

Difference

```text
cut()

↓

Equal Width


qcut()

↓

Equal Number of Observations
```

Example

```python
pd.qcut(

    df["Salary"],

    q=4

)
```

Creates

```text
Quartiles
```

Useful for ranking customers.

---

# cut() vs qcut()

| cut                  | qcut                    |
| -------------------- | ----------------------- |
| Equal interval width | Equal number of records |
| Fixed bins           | Data-driven bins        |

Interview Question ⭐⭐⭐⭐⭐

Very common.

---

# Feature Engineering Examples

---

## Bonus

```python
df["Bonus"] = (
    df["Salary"]*0.10
)
```

---

## Total Salary

```python
df["TotalSalary"] = (

df["Salary"] +

df["Bonus"]

)
```

---

## High Salary

```python
df["HighSalary"] = (

df["Salary"]>60000

)
```

Output

```text
False

True

False

True
```

---

## Age Group

```python
pd.cut(...)
```

---

## Department Code

```python
map()
```

---

## Tax

```python
apply()
```

---

# Method Chaining ⭐⭐⭐⭐⭐

Professional code

```python
df = (

    df

    .assign(

        Bonus=df["Salary"]*0.10

    )

    .assign(

        Total=lambda x:

        x["Salary"]+x["Bonus"]

    )

)
```

Readable

Reusable

Professional

---

# Best Practices ⭐⭐⭐⭐⭐

✔ Create meaningful features.

✔ Avoid creating duplicate information.

✔ Use

```python
map()
```

for encoding.

✔ Use

```python
apply()
```

for custom logic.

✔ Use

```python
clip()
```

to control outliers.

✔ Use

```python
cut()
```

or

```python
qcut()
```

for binning.

---

# Common Mistakes

### ❌ Using `apply()` when `map()` is enough

Simple dictionary replacement?

Use

```python
map()
```

It is simpler and often faster.

---

### ❌ Creating too many useless features

More columns

≠

Better model.

Only create features with meaningful information.

---

### ❌ Forgetting missing values after `map()`

Unknown categories become

```text
NaN
```

---

### ❌ Confusing `where()` and `mask()`

Remember

```text
where()

↓

Keep True

mask()

↓

Replace True
```

---

# Interview Questions

### 1. What is Feature Engineering?

Creating new useful features from existing data to improve model performance.

---

### 2. Difference between `map()` and `apply()`?

* `map()` is mainly for value mapping or replacement.
* `apply()` executes a custom function on each element.

---

### 3. Difference between `where()` and `mask()`?

* `where()` keeps values where the condition is `True`.
* `mask()` replaces values where the condition is `True`.

---

### 4. Difference between `cut()` and `qcut()`?

* `cut()` creates bins with equal widths.
* `qcut()` creates bins containing approximately equal numbers of observations.

---

### 5. Why is `clip()` useful?

To limit extreme values (outliers) to specified lower and upper bounds.

---

# Practice Exercises

Using the sample DataFrame:

1. Create a `Bonus` column (10% of salary).
2. Create a `Salary_per_Age` feature.
3. Encode `Department` using `map()`.
4. Increase salary by 5% using `apply()`.
5. Create an `AgeGroup` using `cut()`.
6. Divide salaries into quartiles using `qcut()`.
7. Replace salaries below ₹55,000 with `0` using `where()`.
8. Replace salaries above ₹60,000 with `0` using `mask()`.
9. Clip salaries between ₹50,000 and ₹65,000.
10. Chain multiple transformations using `assign()`.

---

