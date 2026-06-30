

# 📘 Pandas for Machine Learning

# Module 8 — String Operations (Complete)



# 🎯 Learning Objectives

After completing this module, you will be able to:

* Clean text data
* Convert text to lowercase/uppercase
* Remove unwanted spaces
* Replace text
* Search text
* Split and join strings
* Extract patterns
* Calculate string length
* Prepare text columns for Machine Learning

---

# Why String Cleaning?

Imagine this dataset.

| Name  |
| ----- |
| Rahul |
| RAHUL |
| rahul |
| Rahul |
| Rahul |

Although they represent the same person,

Pandas treats them as **different values**.

This creates problems during:

* Grouping
* Encoding
* Machine Learning

Therefore,

Text cleaning is one of the first preprocessing steps.

---

# Sample Dataset

We'll use this dataset throughout the chapter.

```python
import pandas as pd

df = pd.DataFrame({
    "Name": [
        " Rahul ",
        "AMIT",
        "neha",
        "Priya",
        "John"
    ],

    "Email":[
        "rahul@gmail.com",
        "amit@yahoo.com",
        "neha@gmail.com",
        "priya@gmail.com",
        "john@yahoo.com"
    ],

    "Department":[
        "IT",
        "HR",
        "Sales",
        "IT",
        "HR"
    ]
})

print(df)
```

---

# Important Rule

String methods are accessed using

```python
.str
```

Example

```python
df["Name"].str.lower()
```

Remember

```text
Series

↓

.str

↓

Method
```

---

# 8.1 lower() ⭐⭐⭐⭐⭐

Converts all characters to lowercase.

```python
df["Name"].str.lower()
```

Output

```text
rahul

amit

neha

priya

john
```

---

## Why use lower()?

Suppose

```text
Rahul

RAHUL

rahul
```

Without cleaning

These become

3 different categories.

After

```python
.str.lower()
```

All become

```text
rahul
```

Very important before encoding categorical variables.

---

# 8.2 upper()

Converts text into uppercase.

```python
df["Department"].str.upper()
```

Output

```text
IT

HR

SALES
```

Useful for standardization.

---

# 8.3 title()

Converts every word to title case.

```python
df["Name"].str.title()
```

Output

```text
Rahul

Amit

Neha

Priya

John
```

Useful for names.

---

# 8.4 capitalize()

Only first character becomes uppercase.

```python
df["Name"].str.capitalize()
```

Output

```text
Rahul

Amit

Neha
```

Difference

```text
title()

↓

Every word starts with uppercase


capitalize()

↓

Only first letter of entire string
```

Example

```text
machine learning
```

title()

↓

```text
Machine Learning
```

capitalize()

↓

```text
Machine learning
```

---

# 8.5 strip() ⭐⭐⭐⭐⭐

One of the most useful functions.

Removes spaces.

Example

```text
" Rahul "
```

After

```python
df["Name"].str.strip()
```

Output

```text
Rahul
```

---

Why?

Real datasets often contain

```text
Rahul

Rahul

Rahul
```

Actually

```text
Rahul

 Rahul

Rahul
```

Spaces make them different values.

---

## Remove Left Spaces

```python
.str.lstrip()
```

---

## Remove Right Spaces

```python
.str.rstrip()
```

---

# 8.6 replace() ⭐⭐⭐⭐⭐

Replace text.

Example

```python
df["Department"].str.replace(
    "IT",
    "Information Technology"
)
```

Output

```text
Information Technology
```

---

Replace spaces

```python
.str.replace(
    " ",
    "_"
)
```

Useful when creating IDs.

---

Replace multiple occurrences

```python
.str.replace(
    "a",
    "@"
)
```

---

# 8.7 contains() ⭐⭐⭐⭐⭐

Search text.

Suppose

Find Gmail users.

```python
df[
    df["Email"].str.contains(
        "gmail"
    )
]
```

Output

Only Gmail records.

---

Case insensitive

```python
.str.contains(
    "gmail",
    case=False
)
```

---

Regular Expression

```python
.str.contains(
    r"\d"
)
```

Checks whether digits exist.

---

# 8.8 startswith()

```python
df["Email"].str.startswith(
    "rahul"
)
```

Output

```text
True

False

False
```

---

Useful for

* IDs
* Prefix matching
* Product codes

---

# 8.9 endswith()

Find Gmail users.

```python
df["Email"].str.endswith(
    "gmail.com"
)
```

Output

```text
True

False

True
```

---

# 8.10 split() ⭐⭐⭐⭐⭐

Break text into parts.

Suppose

```text
rahul@gmail.com
```

Split

```python
df["Email"].str.split("@")
```

Output

```text
["rahul","gmail.com"]
```

---

Expand into columns

```python
df["Email"].str.split(
    "@",
    expand=True
)
```

Output

| Username | Domain    |
| -------- | --------- |
| rahul    | gmail.com |

Very useful.

---

# 8.11 join()

Opposite of split.

Suppose

```text
["Machine","Learning"]
```

Join

```python
"-".join(
    ["Machine","Learning"]
)
```

Output

```text
Machine-Learning
```

Less common in Pandas preprocessing, but useful to know.

---

# 8.12 len() ⭐⭐⭐⭐

Length of text.

```python
df["Name"].str.len()
```

Output

```text
5

4

5
```

Useful for

* Password validation
* Feature engineering
* Detecting short/long strings

---

# 8.13 find()

Returns first position.

```python
df["Email"].str.find("@")
```

Output

```text
5

4

5
```

Meaning

```text
@

↓

Position
```

If not found

```text
-1
```

---

# 8.14 extract() ⭐⭐⭐⭐⭐

Very useful.

Suppose

```text
EMP101

EMP102

EMP103
```

Extract numbers.

```python
s = pd.Series([
    "EMP101",
    "EMP102",
    "EMP103"
])

s.str.extract(
    r"(\d+)"
)
```

Output

```text
101

102

103
```

Useful for

* IDs
* Phone numbers
* ZIP codes
* Invoice numbers

---

# 8.15 slice()

Extract part of string.

```python
df["Email"].str.slice(
    0,
    5
)
```

Output

```text
rahul

amit

neha
```

Equivalent to Python slicing.

---

# 8.16 cat()

Concatenate strings.

```python
df["Name"].str.cat(
    df["Department"],
    sep="-"
)
```

Output

```text
Rahul-IT

Amit-HR
```

Useful for creating combined features.

---

# Chaining String Methods ⭐⭐⭐⭐⭐

Very common.

```python
df["Name"] = (
    df["Name"]
        .str.strip()
        .str.lower()
        .str.replace(" ","_")
)
```

Example

Before

```text
" Rahul Kumar "
```

After

```text
rahul_kumar
```

This is how professionals usually clean text.

---

# Real ML Example

Dataset

| City   |
| ------ |
| Mumbai |
| MUMBAI |
| mumbai |
| Mumbai |

Clean

```python
df["City"] = (
    df["City"]
        .str.strip()
        .str.lower()
)
```

Now

All values become

```text
mumbai
```

This avoids creating multiple categories for the same city.

---

# Feature Engineering Example

Extract email domain.

```python
df["Domain"] = (
    df["Email"]
        .str.split("@")
        .str[1]
)
```

Output

| Email                                     | Domain    |
| ----------------------------------------- | --------- |
| [rahul@gmail.com](mailto:rahul@gmail.com) | gmail.com |
| [amit@yahoo.com](mailto:amit@yahoo.com)   | yahoo.com |

Now

Domain becomes a feature for ML.

---

# Best Practices ⭐⭐⭐⭐⭐

✔ Always remove spaces.

```python
.str.strip()
```

---

✔ Convert to lowercase before encoding.

```python
.str.lower()
```

---

✔ Replace inconsistent spellings.

---

✔ Extract useful information.

Example

```text
Email

↓

Domain
```

---

✔ Chain methods whenever possible.

---

# Common Mistakes

### ❌ Forgetting `.str`

Wrong

```python
df["Name"].lower()
```

Correct

```python
df["Name"].str.lower()
```

---

### ❌ Ignoring spaces

```text
Rahul

 Rahul
```

These are different values.

---

### ❌ Not standardizing text

```text
IT

it

It

iT
```

Should become

```text
it
```

---

### ❌ Using `replace()` without understanding regex

`str.replace()` supports regular expressions in many cases. Be aware of the `regex` parameter when replacing patterns versus literal strings.

---

# Interview Questions

### 1. Why use `.str.lower()` before Machine Learning?

To standardize text and prevent identical categories from being treated as different values.

---

### 2. Difference between `strip()` and `replace()`?

* `strip()` removes leading and trailing whitespace.
* `replace()` substitutes matching text or patterns with new values.

---

### 3. Difference between `split()` and `extract()`?

* `split()` separates text based on a delimiter.
* `extract()` retrieves specific parts using regular expressions.

---

### 4. Why is `contains()` useful?

It enables filtering rows based on whether a string contains a given pattern or substring.

---

### 5. What does `expand=True` do in `split()`?

It returns the split components as separate DataFrame columns instead of lists.

---

# Practice Exercises

Using the sample DataFrame:

1. Convert all names to lowercase.
2. Convert names to title case.
3. Remove leading and trailing spaces.
4. Replace `"IT"` with `"Information Technology"`.
5. Find all Gmail users.
6. Find all Yahoo users.
7. Split emails into username and domain.
8. Calculate the length of every name.
9. Extract the domain from each email into a new column.
10. Chain multiple methods to produce clean, lowercase names without extra spaces.

---
