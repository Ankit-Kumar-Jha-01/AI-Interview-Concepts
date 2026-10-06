# ⚖️ Mastering Imbalanced Datasets & SMOTE — AI Interview Guide

> **Interview Scenario:** *"Your dataset has 95% negative samples and only 5% positive samples. Your accuracy is 95%, but your model fails completely in production. What went wrong, and how do you fix it?"*

---

## 🚨 The Illusion of Accuracy

When class proportions are severely unequal (e.g., $95\%$ Non-Fraud vs. $5\%$ Fraud), **Accuracy is a trap**. 

A dummy model that blindly predicts "Non-Fraud" for every single row will achieve $95\%$ accuracy while missing $100\%$ of actual fraud!

### 📊 Right Metrics for Imbalanced Data

Instead of Accuracy, evaluate using:

```
                  ┌──────────────────────────────────────────────┐
                  │          Imbalanced Data Metrics             │
                  └──────────────────────┬───────────────────────┘
                                         │
        ┌────────────────────────────────┼──────────────────────────────┐
        ▼                                ▼                              ▼
   [ Precision ]                    [ Recall ]                     [ F1-Score ]
"Out of all predicted             "Out of all actual             Harmonic mean of
 positives, how many              positives, how many             Precision & Recall
 were correct?"                   did we catch?"                   (Overall balance)
```

$$\text{F1-Score} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$$

---

## 🛠️ 4 Strategies to Handle Imbalanced Data

```
                                  ┌─────────────────────────┐
                                  │   Handling Imbalance    │
                                  └────────────┬────────────┘
                                               │
         ┌──────────────────┬──────────────────┼──────────────────┐
         ▼                  ▼                  ▼                  ▼
   1. Resampling    2. Class Weights    3. Threshold Adjust  4. SMOTE (Synthetic)
 (Over/Under-sample)  (Cost-Sensitive)  (Shift default 0.5) (KNN-based Generation)
```

---

### Strategy 1: Resampling (Oversampling & Undersampling) 🔄

> ⚠️ **Golden Rule:** ALWAYS apply resampling **ONLY on the Training Set**. Never touch the Test Set! Resampling test data distorts true evaluation metrics.

* **Oversampling:** Duplicate existing minority instances to match the majority count.
  * *Risk:* Leads to **overfitting** because the model memorizes exact duplicates.
* **Undersampling:** Randomly discard majority instances to match minority count.
  * *Risk:* Drops potentially valuable information from discarded rows.

---

### Strategy 2: Adjust Class Weights ⚖️

Instead of modifying the dataset size, penalize the model heavily whenever it misclassifies the minority class.

```python
from sklearn.linear_model import LogisticRegression

# Automatically balance class weights inversely proportional to class frequencies
model = LogisticRegression(class_weight='balanced')
model.fit(X_train, y_train)
```

---

### Strategy 3: Shift the Decision Threshold 🎯

By default, classification algorithms assign positive predictions when probability $P \ge 0.5$.

* **Adjustment:** Lower the threshold to $P \ge 0.3$ (or $0.2$).
* **Result:** The model becomes more sensitive to minority positive instances, increasing **Recall** (vital for Medical Diagnostics / Fraud Detection).

---

### Strategy 4: SMOTE (Synthetic Minority Oversampling Technique) 🧬

Instead of blindly duplicating minority samples, **SMOTE creates brand-new, realistic synthetic data points** along feature space vectors.

```
       Existing Points                         SMOTE Synthetic Generation
  
     B ●                                            B ●
        \                                              \  ★ (New Synthetic Point)
         \                                              \ 
          ● A                                            ● A
```

#### 🔬 Variations of SMOTE:
* **Borderline-SMOTE:** Focuses synthetic generation specifically near decision boundaries where classification errors happen most.
* **SMOTEENN:** Combines SMOTE synthetic oversampling with **Edited Nearest Neighbors (ENN)** cleaning to strip away noisy boundary points.

---

## 📐 How SMOTE Works: Step-by-Step Math Intuition

Suppose our minority class has three points: $A = (1, 2)$, $B = (2, 3)$, $C = (3, 4)$.

### **Step 1:** Select a point
Select a random point from the minority class. Let's pick $A = (1, 2)$.

### **Step 2:** Find Nearest Neighbors (KNN)
Compute $k$-Nearest Neighbors (typically $k = 5$ default). $A$'s nearest neighbors are $B$ and $C$.

### **Step 3:** Interpolate a New Point
Pick one neighbor at random (say $B = (2, 3)$) and create a synthetic point along the line segment between $A$ and $B$:

$$\text{New Point} = A + \text{rand}(0, 1) \times (B - A)$$

Substitute values assuming $\text{rand}(0, 1) = 0.5$:

$$\text{New Point} = (1, 2) + 0.5 \times \left((2, 3) - (1, 2)\right)$$

$$\text{New Point} = (1, 2) + 0.5 \times (1, 1) = (1, 2) + (0.5, 0.5) = \mathbf{(1.5, 2.5)}$$

---

## 🛑 Disadvantages & Limitations of SMOTE

While SMOTE is powerful, watch out for these interview pitfalls:

1. **Noise Amplification:** If minority data contains noisy or corrupted samples, SMOTE will generate bad synthetic points around that noise.
2. **Overlapping Classes:** In overlapping distributions, SMOTE may create synthetic points deep inside majority class territory, increasing confusion.
3. **Incompatible with Categorical Features:** Standard SMOTE relies on Euclidean distance, producing non-sensical values for categorical variables (e.g., $\text{Gender} = 0.3$).
   * *Solution:* Use **SMOTENC** (SMOTE for Nominal and Continuous features).

---

## 📌 Key Takeaways

1. **Accuracy is Misleading:** Use **Precision, Recall, and F1-Score** for imbalanced evaluation.
2. **Train Set Isolation:** Always perform resampling/SMOTE **ONLY on training splits**.
3. **Class Weights are Simple & Fast:** Use `class_weight='balanced'` before jumping straight to synthetic generation.
4. **SMOTE Interpolates, Doesn't Copy:** SMOTE constructs points using vector math: $A + \text{rand} \times (B - A)$.
5. **Categorical Caution:** Use **SMOTENC** when handling non-numeric features to prevent invalid synthetic states.

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
