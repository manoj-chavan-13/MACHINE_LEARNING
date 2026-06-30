# 📘 Pandas for Machine Learning

# Module 9 — Date & Time Operations (Complete)

---

# 🎯 Learning Objectives

After completing this module, you will be able to:

* Convert strings into datetime objects.
* Extract year, month, day, hour, minute, and second.
* Calculate date differences.
* Filter data by dates.
* Handle invalid dates.
* Create date-based ML features.

---

# Why Date & Time Matters?

Suppose you're predicting **house prices**.

| House | Sale Date  | Price   |
| ----- | ---------- | ------- |
| A     | 2024-01-15 | 50 Lakh |
| B     | 2024-08-20 | 65 Lakh |

The model doesn't understand:

```text
2024-08-20
```

But it can understand:

* Year = 2024
* Month = 8
* Day = 20
* Quarter = 3
* Weekday = Tuesday

These become useful ML features.

---

# Sample Dataset

We'll use this DataFrame throughout the module.

```python
import pandas as pd

df = pd.DataFrame({
    "Employee": ["Rahul", "Amit", "Neha", "John"],
    "JoiningDate": [
        "2021-01-15",
        "2022-05-10",
        "2020-12-25",
        "2023-08-05"
    ]
})

print(df)
```

Output

| Employee | JoiningDate |
| -------- | ----------- |
| Rahul    | 2021-01-15  |
| Amit     | 2022-05-10  |
| Neha     | 2020-12-25  |
| John     | 2023-08-05  |

Currently,

`JoiningDate`

is still a **string**, not a date.

---

# 9.1 to_datetime() ⭐⭐⭐⭐⭐

Most important function.

Convert strings into datetime.

```python
df["JoiningDate"] = pd.to_datetime(df["JoiningDate"])
```

Now

```python
df.dtypes
```

Output

```text
JoiningDate    datetime64[ns]
```

Now Pandas recognizes it as a date.

---

# Why Convert?

Without conversion

```python
df["JoiningDate"].dt.year
```

gives an error.

After conversion,

everything works.

---

# Handling Invalid Dates

Suppose

```text
2022-01-10

ABC

2021-12-20
```

Convert safely

```python
pd.to_datetime(
    df["JoiningDate"],
    errors="coerce"
)
```

Output

```text
2022-01-10

NaT

2021-12-20
```

`NaT`

means

```text
Not a Time
```

Equivalent to `NaN` for datetime.

---

# Date Accessor

All datetime functions use

```python
.dt
```

Think

```text
Datetime

↓

.dt

↓

Property
```

---

# 9.2 Year ⭐⭐⭐⭐⭐

Extract year.

```python
df["JoiningDate"].dt.year
```

Output

```text
2021

2022

2020

2023
```

---

# ML Use

Sales often vary by year.

The model can learn

```text
Year

↓

Trend
```

---

# 9.3 Month ⭐⭐⭐⭐⭐

```python
df["JoiningDate"].dt.month
```

Output

```text
1

5

12

8
```

---

Useful for

* Seasonality
* Monthly sales
* Rainfall prediction
* Customer activity

---

# Month Name

```python
df["JoiningDate"].dt.month_name()
```

Output

```text
January

May

December

August
```

---

# 9.4 Day

```python
df["JoiningDate"].dt.day
```

Output

```text
15

10

25

5
```

---

# Day Name ⭐⭐⭐⭐

```python
df["JoiningDate"].dt.day_name()
```

Output

```text
Friday

Tuesday

Friday

Saturday
```

Useful in

* Retail
* Food delivery
* Banking

Weekend behavior is often different from weekdays.

---

# 9.5 Weekday

Returns number.

```python
df["JoiningDate"].dt.weekday
```

Output

```text
0 Monday

1 Tuesday

2 Wednesday

3 Thursday

4 Friday

5 Saturday

6 Sunday
```

Example

```text
Friday

↓

4
```

---

# Weekend Feature ⭐⭐⭐⭐⭐

A common feature.

```python
df["IsWeekend"] = (
    df["JoiningDate"].dt.weekday >= 5
)
```

Output

```text
False

False

False

True
```

Many ML models benefit from this.

---

# 9.6 Quarter

```python
df["JoiningDate"].dt.quarter
```

Output

```text
1

2

4

3
```

Quarter is useful in

* Finance
* Sales
* Business forecasting

---

# 9.7 Hour

Suppose

```text
2024-01-15 18:30:45
```

Extract

```python
df["Date"].dt.hour
```

Output

```text
18
```

---

# Minute

```python
df["Date"].dt.minute
```

---

# Second

```python
df["Date"].dt.second
```

---

Useful for

* IoT
* Sensor data
* Login activity
* Event logs

---

# 9.8 Date Difference ⭐⭐⭐⭐⭐

Suppose

```python
today = pd.Timestamp.today()
```

Calculate

```python
today - df["JoiningDate"]
```

Output

```text
1500 days

900 days

...
```

---

Convert to days

```python
(today - df["JoiningDate"]).dt.days
```

Output

```text
1500

900

...
```

---

Real ML Example

Employee Experience

Instead of

```text
Joining Date
```

Use

```text
Years Since Joining
```

Models learn from numbers more effectively.

---

# 9.9 Current Date

Current timestamp

```python
pd.Timestamp.now()
```

Today's date

```python
pd.Timestamp.today()
```

Current date only

```python
pd.Timestamp.today().date()
```

---

# 9.10 Filtering Dates ⭐⭐⭐⭐⭐

Suppose

Find employees who joined after

2021.

```python
df[
    df["JoiningDate"] >
    "2021-12-31"
]
```

Output

Only employees after 2021.

---

Find year

```python
df[
    df["JoiningDate"].dt.year == 2022
]
```

---

Find month

```python
df[
    df["JoiningDate"].dt.month == 12
]
```

---

# 9.11 Sorting Dates

```python
df.sort_values("JoiningDate")
```

Latest first

```python
df.sort_values(
    "JoiningDate",
    ascending=False
)
```

---

# 9.12 Creating Date Features ⭐⭐⭐⭐⭐

Instead of one column

```text
JoiningDate
```

Create

```python
df["Year"] = df["JoiningDate"].dt.year

df["Month"] = df["JoiningDate"].dt.month

df["Day"] = df["JoiningDate"].dt.day

df["Weekday"] = df["JoiningDate"].dt.weekday

df["Quarter"] = df["JoiningDate"].dt.quarter
```

Original

| JoiningDate |
| ----------- |
| 2023-08-05  |

Becomes

| Year | Month | Day | Weekday | Quarter |
| ---- | ----- | --- | ------- | ------- |
| 2023 | 8     | 5   | 5       | 3       |

Much more useful for ML.

---

# Date Formatting

Convert date back to string.

```python
df["JoiningDate"].dt.strftime("%d-%m-%Y")
```

Output

```text
15-01-2021
```

Common formats

```python
"%Y-%m-%d"

"%d/%m/%Y"

"%B %d, %Y"
```

---

# Best Practices ⭐⭐⭐⭐⭐

✔ Always convert strings using

```python
pd.to_datetime()
```

---

✔ Handle invalid dates

```python
errors="coerce"
```

---

✔ Extract

* Year
* Month
* Weekday
* Quarter

for ML.

---

✔ Calculate date differences.

---

✔ Remove original date column only after creating useful features (if your model doesn't require the raw timestamp).

---

# Common Mistakes

### ❌ Using string dates directly

Wrong

```text
2024-01-10
```

Convert first.

---

### ❌ Forgetting `pd.to_datetime()`

Without conversion,

`.dt`

will not work.

---

### ❌ Ignoring invalid dates

Use

```python
errors="coerce"
```

---

### ❌ Feeding raw dates directly into ML

Instead,

extract meaningful numerical features.

---

# Interview Questions

### 1. Why use `pd.to_datetime()`?

To convert strings into datetime objects so Pandas can perform date operations.

---

### 2. What does `.dt` do?

It provides access to datetime-specific properties and methods.

---

### 3. Difference between `NaN` and `NaT`?

* `NaN` → Missing numeric value.
* `NaT` → Missing datetime value.

---

### 4. Why extract year and month for ML?

Because most ML models work with numerical features rather than raw date strings.

---

### 5. How do you calculate the number of days between two dates?

```python
(date2 - date1).dt.days
```

---

# Practice Exercises

Using the sample DataFrame:

1. Convert `JoiningDate` to datetime.
2. Extract the year.
3. Extract the month.
4. Extract the day.
5. Extract the weekday.
6. Extract the month name.
7. Create an `IsWeekend` feature.
8. Calculate the number of days since joining.
9. Filter employees who joined in 2022.
10. Create `Year`, `Month`, `Quarter`, and `Weekday` columns.

---
