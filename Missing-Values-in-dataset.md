What If a Dataset Contains 90% Missing Values?

🎯 Core Idea

Don't immediately use fillna() just because a column contains many
missing values.

First, understand the missingness and decide whether the column or row
is useful.

A good approach is:

Identify missing values
        ↓
Analyse the missingness
        ↓
Check whether the column/row is useful
        ↓
Choose an appropriate strategy

1. Check the Missing Values

First, identify which columns contain a large percentage of missing
values.

df.isnull().mean() * 100

This gives the percentage of missing values in each column.

2. Case 1 --- Column Is Not Useful

If a column contains around 90% or more missing values and the column
provides little useful information, we can consider dropping it.

df = df.drop(columns=['column_name'])

Why?

There may be too little information available to learn from.

Filling most of the column would require making assumptions about data
that we do not actually have.

High missingness + low usefulness → consider dropping the column.

3. Case 2 --- Important Column With Many Missing Values

Sometimes a feature is important even though a large percentage of its
values are missing.

For example:

Income

Age

Medical information

In this situation, we should not automatically drop the column.

The strategy depends on the data type.

Type 1 --- Numerical Column

For numerical data, one possible approach is median imputation.

df['income'] = df['income'].fillna(df['income'].median())

Median can be useful when the data contains outliers because it is less
affected by extreme values than the mean.

Median is not automatically the correct choice. The appropriate
strategy depends on the dataset.

Type 2 --- Categorical Column

For categorical data, we can use a separate category such as:

Unknown

For example:

City
----
Delhi
Mumbai
Unknown
Kolkata

This preserves the information that the original value was missing.

Group-Based Imputation

Instead of blindly using the same value for everyone, we can sometimes
use relevant groups.

For example, if age is missing:

Male   → use median age of males
Female → use median age of females

The grouping feature should be chosen based on the dataset and its
relationship with the missing feature.

4. Case 3 --- Treat Missingness as Information

Sometimes the fact that a value is missing can itself contain useful
information.

For example, people whose age information is missing might represent a
different group from people whose age is available.

We can create a missing-value indicator:

df['age_missing'] = df['age'].isnull().astype(int)

This creates:

0 → Age was available
1 → Age was missing

The model can then potentially learn whether missingness itself is
related to the target.

5. Case 4 --- Predict the Missing Values

For important features with substantial missing data, we can estimate
the missing values using other available features.

For example, missing Age could potentially be estimated using:

Gender

Fare

Class

Other relevant features

The idea is:

Other available features
          ↓
   Imputation model
          ↓
   Estimated value

Examples of model-based imputation include:

KNN Imputation

Regression-based imputation

Iterative Imputation

This can be more sophisticated than replacing every missing value with
one constant.

6. Case 5 --- Check Rows, Not Just Columns

So far, we have focused on columns.

We should also check whether some rows contain almost no useful
information.

For example, if a row contains only one known value and everything else
is missing, that row may provide very little information for the model.

We can remove rows based on the minimum number of non-missing values.

df = df.dropna(thresh=2)

What does thresh=2 mean?

It means:

Keep only rows that contain at least 2 non-missing values.

So:

Row A → 5 valid values → Keep
Row B → 2 valid values → Keep
Row C → 1 valid value  → Drop
Row D → 0 valid values  → Drop

The threshold should be chosen according to the dataset rather than
using 2 blindly.

🧠 Important Interview Point

There is no single correct solution for a column containing 90%
missing values.

The decision depends on:

Why the values are missing

Whether the feature is useful

Data type

Relationship with the target

Dataset size

Whether missingness itself is informative

How much information remains in each row

💬 Interview Answer

"If a dataset contains around 90% missing values in a column, I
wouldn't immediately apply fillna(). First, I would analyse the
missingness and determine whether the column is useful. If it has very
high missingness and provides little value, I may drop it. If it is an
important numerical feature, I could consider median or more advanced
imputation depending on the data. For categorical features, I could
use an 'Unknown' category. I would also check whether the missingness
itself contains useful information by creating a missing-value
indicator. For important features, model-based imputation could also
be considered. I would also check for rows with very little
information."

🔑 Key Takeaway

Don't ask only:

"How should I fill the missing values?"

First ask:

"Why are the values missing, and what information can I preserve?"

That determines the appropriate strategy.
