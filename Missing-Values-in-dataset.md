# 🚨 "Help! 90% of My Data is Missing!" — The AI Interview Masterclass

> **Interview Scenario:** *The interviewer hands you a dataset where 90% of a column is filled with `NaN` values. They ask: "How do you handle this?"*
>
> **❌ Bad Answer:** "I'll just run `df.fillna()` immediately."
>
> **✅ Winning Answer:** "Hold on! Before filling anything, I need to diagnose the missingness and choose a strategy based on domain importance, column type, and feature engineering potential."

---

## 💡 The Mindset: Stop Guessing, Start Diagnosing

When data goes missing, jumping straight to `fillna()` is like applying a bandage without checking the diagnosis.

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

### Case 1: Is this column even worth saving? 🚮

Ask yourself: **"Does this feature drive real business value?"**

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
| **Numerical** (e.g., *Age, Income*) | **Median** Imputation | Robust against outliers / extreme skewed values. | **Group-Based Imputation**: Impute using sub-group logic. <br>`"Male" → Avg Male Age` |
| **Categorical** (e.g., *City, Job*) | **"Unknown"** String | Safe placeholder; doesn't assume facts. | **Group-based Mode / Predictive Imputation** |

#### 🧠 Smarter Strategy Example: Group-Based Imputation

Instead of filling `Age` blindly with the global median, fill it based on sub-categories:

```python
# Fill missing age based on Gender group median
df['Age'] = df.groupby('Gender')['Age'].transform(lambda x: x.fillna(x.median()))
```

---

### Case 3: Tell the Model Data was Missing 🚩

> **Pro Tip:** Not having data is itself a signal!

* **Example:** People who don't disclose their income might belong to a specific high-net-worth or sensitive demographic group.
* **Action:** Add a binary indicator flag before filling missing values.

```python
# Create an indicator column
df['Age_Was_Missing'] = df['Age'].isnull().astype(int)

# Now safely fill original missing values
df['Age'] = df['Age'].fillna(df['Age'].median())
```

---

### Case 4: Predict Missing Values (Smart ML Filling) 🔮

Use other complete features to predict the missing values using machine learning (e.g., `IterativeImputer` / KNN Imputer).

```
  Existing Features                     Missing Target
┌──────────────────┐                  ┌────────────────┐
│ Gender | Fare | Class │  ─────────► │ Predict: Age   │
└──────────────────┘                  └────────────────┘
```

```python
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer

imputer = IterativeImputer(random_state=42)
df_imputed = imputer.fit_transform(df[['Gender_Code', 'Fare', 'Pclass', 'Age']])
```

---

### Case 5: Inspect ROWS, Not Just Columns 🧹

Sometimes individual rows are completely empty. Filter out worthless rows before touching columns!

```python
# Keep ONLY rows with at least 2 non-null values
df = df.dropna(thresh=2)
```

---

## 📌 Key Takeaways

1. **Diagnose First:** Never jump straight to `fillna()`. Analyze missingness before touching the data.
2. **Evaluate Importance:** If a column has $>90\%$ missing data and low feature importance, **drop it**.
3. **Use Median over Mean:** Numerical data with extreme outliers should be imputed with the **median**.
4. **Group-based > Global:** Use `groupby()` logic (e.g., age by gender/class) for smarter imputation.
5. **Treat Missingness as a Signal:** Create binary flags (`is_missing`) to let models learn from missingness patterns.
6. **Row-level Hygiene:** Use threshold parameters like `df.dropna(thresh=k)` to prune empty records early.

---

---

## 👨‍💻 About the Author

<div align="center">

### **Ankit Kumar Jha**  
*Data Science & Machine Learning Enthusiast*

[![Portfolio](https://img.shields.io/badge/Portfolio-🌐_Visit_Site-00D2FF?style=for-the-badge)](https://ankit-kumar-jha-01.github.io/portfolio/)
[![GitHub](https://img.shields.io/badge/GitHub-Ankit--Kumar--Jha--01-181717?style=for-the-badge&logo=github)](https://github.com/Ankit-Kumar-Jha-01)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-ankit--kumar--jhaa-0A66C2?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/ankit-kumar-jhaa)

---
*This repository is a personal learning resource and is continuously evolving. Found this useful? Star ⭐ the repository to keep track of new AI/ML interview concepts!*

</div>
