What if dataset contains 90% of null values. 
Instead of directly jump to fillna() [used to replace missing values (NaN) in a DataFrame or 
Series] immediately, First understand the missingness. 
Analyse missing data and it shows that which column contains 90%< of null values. 
Case 1: Ask this ques on to yourself that “Is this column even useful?” 
If it contains more than 90% of missing values along with the columns aren’t useful, 
then usually we drop that column by df = df.drop(columns=['column_name']). 
Because Too li le data to learn from, filling it = mostly guessing.  
Case 2: Important column (but missing a lot), for ex: Income, Age, Medical data. Then don’t 
drop immediately
