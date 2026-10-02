# What If a Dataset Contains 90% Missing Values?

## 🎯 Core Idea

Don't immediately use `fillna()` just because a column contains missing values.

First, **understand why the data is missing and whether the column is useful**.

A good approach is:

```text
Identify missing values
        ↓
Analyse the missingness
        ↓
Check whether the column is useful
        ↓
Choose a strategy
```

---

## 1️⃣ Check the Missing Values

First, identify which columns contain a large percentage of missing values.

```python
df.isnull().mean() * 100
```

This tells us the percentage of missing values in each column.

---

## 2️⃣ Case 1 — Column Is Not Useful

If a column contains around 90% or more missing values **and the column does not provide useful information**, we can consider dropping it.

```python
df = df.drop(columns=['column_name'])
```

### Why?

There may be too little information available to learn from.

Filling most of the column would mean making assumptions about data that we don't actually have.

> **High missingness + low usefulness → consider dropping the column.**

---

## 3️⃣ Case 2 — Important Column With Many Missing Values

Sometimes a column is important even though a large percentage of its values are missing.

For example:

* Income
* Age
* Medical information

In this case, we should not automatically drop the column.

The strategy depends on the data type.

### Numerical Column

For numerical data, one option is to use the **median**.

```python
df['income'] = df['income'].fillna(df['income'].median())
```

Median can be useful when the data contains outliers because it is less affected by extreme values than the mean.

> This is still an assumption, so the choice should depend on the dataset.

### Categorical Column

For categorical data, we can create a separate category such as:

```text
Unknown
```

For example:

```text
City:
Delhi
Mumbai
Unknown
Kolkata
```

This preserves the information that the original value was missing.

---

## 4️⃣ Case 3 — Treat Missingness as Information

Sometimes **the fact that a value is missing can itself contain useful information**.

For example:

```text
Age = Missing
```

may represent a different group of users from:

```text
Age = 25
```

In such cases, we can create a missing-value indicator:

```python
df['age_missing'] = df['age'].isnull().astype(int)
```

Now:

```text
0 → Age was available
1 → Age was missing
```

The model can potentially learn whether missingness itself is related to the target.

---

## 5️⃣ Case 4 — Predict the Missing Values

For important columns with substantial missing data, we can also estimate the missing values using other features.

For example:

```text
Age
Income
Occupation
Education
Location
```

If `Income` is missing, other available features could potentially be used to estimate it.

This is called **model-based imputation**.

Examples include:

* KNN imputation
* Regression-based imputation
* Iterative imputation

This can be more sophisticated than simply replacing every missing value with one constant.

---

## 🧠 Important Interview Point

There is **no single correct solution** for a column containing 90% missing values.

The decision depends on:

* Why the values are missing
* Whether the column is useful
* Data type
* Relationship with the target
* Dataset size
* Whether missingness itself is informative

---

## 💬 Interview Answer

> "If a dataset contains around 90% missing values in a column, I wouldn't immediately apply `fillna()`. First, I would analyse the missingness and determine whether the column is useful. If it has very high missingness and provides little value, I may drop it. If it's an important numerical feature, I could consider median or more advanced imputation depending on the data. For categorical features, I could use an 'Unknown' category. I would also check whether the missingness itself contains useful information by creating a missing-value indicator. For important features, model-based imputation could also be considered."

---

## 🔑 Key Takeaway

**Don't ask:**

> "How should I fill the missing values?"

First ask:

> **"Why are the values missing, and what information can I preserve?"**

That determines the appropriate strategy.
