# 🚨 "Help! 90% of My Data is Missing!" — The AI Interview Masterclass

> **Interview Scenario:** *The interviewer hands you a dataset where 90% of a column is filled with `NaN` values. They ask: "How do you handle this?"*
> 
> **❌ Bad Answer:** "I'll just run `df.fillna()` immediately."  
> **✅ Winning Answer:** "Hold on! Before filling anything, I need to diagnose the missingness and choose a strategy based on domain importance, column type, and feature engineering potential."

---

## 💡 The Mindset: Stop Guessing, Start Diagnosing

When data goes missing, jumping straight to `fillna()` is like applying a bandage before checking if the bone is broken. 

```
                               ┌──────────────────────────┐
                               │ 90% Null Values Detected │
                               └────────────┬─────────────┘
                                            │
                             ┌──────────────┴──────────────┐
                             ▼                             ▼
                  [ 🚮 Is column useless? ]     [ 💎 Is column crucial? ]
                             │                             │
                             ▼                             ▼
                       DROP COLUMN                 STRATEGIC HANDLING
                 df.drop(columns=['col'])          (Check Cases Below)
```

---

## 🛠️ The 5-Step Battle Plan

---

### Case 1: Is this column even worth saving? 🚮

Ask yourself: **"Does this feature drive business value?"**

* **The Reality:** Learning from 10% remaining data is often just learning noise. Imputing 90% means your model learns **your guesses**, not real patterns.
* **Action:** If it's a low-importance column, **drop it**.

```python
# Drop columns with >90% missing values
threshold = 0.90
df = df.drop(columns=df.columns[df.isnull().mean() > threshold])
```

---

### Case 2: Important Column + High Missingness 💎

What if the missing data is **Income**, **Age**, or **Medical Results**? You **cannot** just throw it away!

| Feature Type | Basic Strategy | Why? | Smarter Strategy 🧠 |
| :--- | :--- | :--- | :--- |
| **Numerical** (e.g., *Age, Income*) | **Median** Imputation | Robust against outliers/extreme values. | **Group-Based Imputation**<br>(e.g., average age of males vs. females). |
| **Categorical** (e.g., *City, Department*) | Fill with `"Unknown"` | Safe baseline; avoids making false assumptions. | **Mode by Sub-group**<br>(e.g., most common city per region). |

#### 🧠 Smarter Imputation in Action (Group-based)

Instead of filling missing ages with the global average (say, 35 years old), group by relevant categorical features first:

```python
# Group-based imputation: Fill Age based on Gender median
df['Age'] = df.groupby('Gender')['Age'].transform(lambda x: x.fillna(x.median()))
```

---

### Case 3: "Missingness" IS the Signal 🚨

Sometimes, **the fact that data is missing tells a story**. 

> 💡 **Real-world Example:** In a survey, people with extremely high or low incomes often skip the "Income" question. Missingness itself correlates with target behavior!

```
┌──────────────────────────────┐
│  Original Column: Age        │
│  [ 25, NaN, 40, NaN, 30 ]    │
└──────────────┬───────────────┘
               │
               ├──────────────────────────────────────────┐
               ▼                                          ▼
┌──────────────────────────────┐          ┌──────────────────────────────┐
│  1. Fill Missing Values      │          │  2. Create Indicator Column  │
│  [ 25, 31, 40, 31, 30 ]      │          │  Age_Is_Missing:             │
└──────────────────────────────┘          │  [ 0,  1,  0,  1,  0 ]       │
                                          └──────────────────────────────┘
```

#### Code Implementation:
```python
# Create an explicit missingness indicator
df['Age_is_missing'] = df['Age'].isnull().astype(int)

# Now safely fill the missing numerical values
df['Age'] = df['Age'].fillna(df['Age'].median())
```

---

### Case 4: Predict Missing Values using ML 🤖

Instead of blind statistics (mean/median), treat the missing column as a target variable and predict it using intact columns!

* **Example:** Predict missing `Age` using `Gender`, `Fare`, and `Pclass`.

```
                  ┌─────────────────────────────────────────┐
                  │ Intact Features: Gender, Fare, Pclass   │
                  └────────────────────┬────────────────────┘
                                       │
                                       ▼
                       ┌──────────────────────────────┐
                       │   ML Model (e.g., KNN / RF)   │
                       └───────────────┬──────────────┘
                                       │
                                       ▼
                       ┌──────────────────────────────┐
                       │  Predicted Values for [Age]  │
                       └──────────────────────────────┘
```

```python
from sklearn.impute import KNNImputer

imputer = KNNImputer(n_neighbors=5)
df_imputed = imputer.fit_transform(df[['Gender_Code', 'Fare', 'Pclass', 'Age']])
```

---

### Case 5: Inspect ROWS, Not Just Columns 🔍

Don't spend all your energy fixing columns while ignoring dirty rows. A row with almost no non-null values is dead weight.

```
Row Index │ Col A │ Col B │ Col C │ Col D │ Status
──────────┼───────┼───────┼───────┼───────┼─────────────────────────────
    1     │  25   │ Male  │  100  │  NYC  │ ✅ Keep (4/4 populated)
    2     │  NaN  │  NaN  │  NaN  │  NYC  │ ❌ Drop (Only 1 value!)
```

#### The `thresh` parameter trick:
Keep only rows that have **at least $N$ non-missing values**.

```python
# Keep rows with AT LEAST 2 non-null values
df = df.dropna(thresh=2)
```

---

## 📝 Summary Checklist for the Interviewer

When asked about missing data, structure your response as:

1. 🔍 **Diagnose:** Calculate missingness percentages across rows & columns.
2. 🚮 **Evaluate:** Drop non-essential columns with >90% missingness.
3. 🎯 **Impute Smartly:** Use Median (Numerical) or Group-Based strategies for vital features.
4. 🚩 **Flag Missingness:** Add binary indicators (`col_is_missing`) to preserve missingness signal.
5. 🤖 **Advanced:** Use KNN / Iterative Imputers when correlations exist across other columns.
6. 🧹 **Filter Rows:** Clean sparse rows using `df.dropna(thresh=N)`.
