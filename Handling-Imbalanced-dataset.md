Imbalanced dataset? 
 When one class is much more than the other. For ex: 95% are True and 5% are False. 
 Instead of using accuracy, these are good metrices for imbalanced datasets 
Precision, Recall, F1-score 
How to handle imbalance dataset? 
1. Most common thing we can do to handle imbalance dataset is Resampling. Always 
Apply resampling ONLY on training data. Never on test data. Resampling contains 
two things which are: 
o Oversampling (increase minority) Duplicate minority class or use SMOTE. 
o Undersampling (reduce majority) Remove some majority data. 
Disadvantage of this technique is somemes model overfits because we are 
playing with the exis ng data by augmen ng or duplica ng them. 
2. Use class weightsTell model: “Minority class is more important”. 
o model = LogisticRegression(class_weight='balanced') 
3. Change threshold (advanced but simple idea) 
Default: 0.5 but we can adjust it to 0.3 by which it detects more posi ves 
4. SMOTE = Synthe c Minority Oversampling Technique 
In smote we create a new data point ar ficially without resampling the dataset. 
Smote creates new fake (but realis c) data points. 
It doesn’t guess randomly. It creates points in between similar data. 
So, the dataset stays realis c and avoids duplicates. 
Borderline-SMOTE → focuses near decision boundary  
SMOTEENN → combines SMOTE + cleaning 
How SMOTE works? 
Suppose minority class has: A = (1,2), B = (2,3), C = (3,4) 
Step 1 Pick one point. Like Take A = (1,2) 
Step 2 Find its nearest neighbours (KNN) using KNN (usually k=5 by default). 
So, Nearest neighbours of A B, C. 
Step 3 Create new point between them. SMOTE randomly picks one  
neighbour (say B) and then creates: 
New = A + random * (B - A) 
For ex: (1,2) + 0.5 * [(2,3) - (1,2)]= (1.5, 2.5) 
Disadvantage of SMOTE: 
o Noise problem: If data has noise then SMOTE may create bad synthe c points 
o Overlapping classes: If classes are mixed, then SMOTE may generate 
confusing points. 
o Not great for categorical data: Because it creates a value like:  
Gender= 0.3 (which makes no sense).
