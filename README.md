# 🧠 AI Interview Concepts

A collection of **practical AI, Machine Learning, and Data Science concepts** that are commonly discussed in technical interviews.

This repository focuses less on memorizing definitions and more on understanding:

> **"What would you do if this situation occurs?"**

The notes contain practical scenarios, possible approaches, reasoning, and interview-ready explanations.

---

## 📚 Topics

### 📊 Data & Preprocessing

* Handling Missing Values
* Highly Imbalanced Datasets
* Outliers
* Duplicate Data
* Categorical Variables
* Feature Scaling
* Feature Selection
* Feature Engineering

### 🤖 Machine Learning

* Overfitting & Underfitting
* Data Leakage
* Bias & Variance
* Train-Test Distribution
* Cross-Validation
* Model Selection
* Hyperparameter Tuning

### 📈 Model Evaluation

* Accuracy
* Precision
* Recall
* F1-Score
* ROC-AUC
* Confusion Matrix
* Precision vs Recall
* Choosing the Right Evaluation Metric

### 🧠 Deep Learning & AI

* Vanishing & Exploding Gradients
* Transfer Learning
* CNN-related Interview Concepts
* Transformer-related Interview Concepts
* Model Inference
* Model Performance & Optimization

---

## 💡 How the Concepts Are Structured

Each topic tries to follow a practical reasoning approach:

```text
Interview Question
        ↓
Understand the Problem
        ↓
Identify Possible Causes
        ↓
Consider Different Approaches
        ↓
Choose an Appropriate Strategy
        ↓
Explain the Reasoning
```

The goal is to understand **why** a particular approach is used instead of simply memorizing an answer.

---

## 📝 Example

### What if a dataset contains 90% missing values?

Instead of immediately applying `fillna()`, first ask:

* Why is the data missing?
* Is the column useful?
* Is the feature numerical or categorical?
* Is missingness itself meaningful?
* Can the missing values be estimated using other features?

Depending on the situation, possible approaches include:

* Dropping the column
* Median or other statistical imputation
* Using an `Unknown` category
* Creating a missing-value indicator
* Model-based imputation

The appropriate choice depends on the **dataset and the reason for missingness**.

---

## 🎯 Purpose

This repository is created for:

* Technical interview preparation
* Revising important ML concepts
* Understanding practical data problems
* Improving problem-solving skills
* Preparing concise interview explanations

These are **learning notes**, not a collection of fixed answers. Different datasets and situations can require different approaches.

---

## 🔄 Learning Approach

I try to approach interview problems using:

```text
Understand
   ↓
Analyse
   ↓
Consider alternatives
   ↓
Choose
   ↓
Explain why
```

This helps me focus on **reasoning instead of memorization**.

---

## 🚧 Status

**Actively learning and updating.**

New interview concepts and practical scenarios will be added as I continue learning AI, Machine Learning, and Data Science.

---

## 👨‍💻 About

**Ankit Kumar Jha**

B.Tech CSE — Data Science

[![Portfolio](https://img.shields.io/badge/Portfolio-🌐_Visit_Site-00D2FF?style=for-the-badge)](https://ankit-kumar-jha-01.github.io/portfolio/)
[![GitHub](https://img.shields.io/badge/GitHub-Ankit--Kumar--Jha--01-181717?style=for-the-badge&logo=github)](https://github.com/Ankit-Kumar-Jha-01)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-ankit--kumar--jhaa-0A66C2?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/ankit-kumar-jhaa)


Interested in:

* Artificial Intelligence
* Machine Learning
* Data Science
* NLP
* Generative AI
* AI Agents

---

This repository is a personal learning resource and is continuously evolving. 
Found this useful? Star ⭐ the repository to keep track of new AI/ML interview concepts!
