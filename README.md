# WDM-Ex.No-3--IMPLEMENTATION-OF-APRIORI-ALGORITHM-IN-PYTHON-
### Aim:
To implement Apriori Algorithm in python.
### Implementation Steps:
Follow these steps to implement Apriori algorithm in Python: 

### Import the Libraries
The first step, as always, is to import the required libraries. Execute the following script to do so: 

import numpy as np 
import matplotlib.pyplot as plt 
import pandas as pd 
from apyori import apriori 
In the script above we import pandas, numpy, pyplot, and apriori libraries. 

### Importing the Dataset
Now let's import the dataset and see what we're working with. Download the dataset and place it in the "Datasets" 
folder of the "D" drive (or change the code below to match the path of the file on your computer) and execute the 
following script: 
store_data = pd.read_csv('D:\\Datasets\\store_data.csv') 
Let's call the head() function to see how the dataset looks: 
store_data.head() 

<img width="729" height="172" alt="image" src="https://github.com/user-attachments/assets/25db435e-c7f0-407d-a56d-8450d13abd09" />



A snippet of the dataset is shown in the above screenshot. If you carefully look at the data, we can see that the header 
is actually the first transaction. Each row corresponds to a transaction and each column corresponds to an item 
purchased in that specific transaction. The NaN tells us that the item represented by the column was not purchased in 
that specific transaction. 
In this dataset there is no header row. But by default, pd.read_csv function treats first row as header. To get rid of this 
problem, add header=None option to pd.read_csv function, as shown below: 
store_data = pd.read_csv('D:\\Datasets\\store_data.csv', header=None) 

Now execute the head() function: 
store_data.head() 
In this updated output you will see that the first line is now treated as a record instead of header as shown below: 
<img width="706" height="155" alt="image" src="https://github.com/user-attachments/assets/6a99a356-cdac-40d7-acd5-98d0ad9d2ae1" />

 
Now we will use the Apriori algorithm to find out which items are commonly sold together, so that store owner can 
take action to place the related items together or advertise them together in order to have increased profit. 
### Data Preprocessing
The Apriori library we are going to use requires our dataset to be in the form of a list of lists, where the whole dataset 
is a big list and each transaction in the dataset is an inner list within the outer big list. Currently we have data in the 
form of a pandas dataframe. To convert our pandas dataframe into a list of lists, execute the following script: 
records = [] 
for i in range(0, 7501): 
    records.append([str(store_data.values[i,j]) for j in range(0, 20)]) <br>
### Applying Apriori
The next step is to apply the Apriori algorithm on the dataset. To do so, we can use the apriori class that we imported 
from the apyori library. <br>
The apriori class requires some parameter values to work. The first parameter is the list of list that you want to extract 
rules from. The second parameter is the min_support parameter. This parameter is used to select the items with 
support values greater than the value specified by the parameter. Next, the min_confidence parameter filters those 
rules that have confidence greater than the confidence threshold specified by the parameter. Similarly, 
the min_lift parameter specifies the minimum lift value for the short listed rules. Finally, the min_length parameter 
specifies the minimum number of items that you want in your rules. 
Let's suppose that we want rules for only those items that are purchased at least 5 times a day, or 7 x 5 = 35 times in 
one week, since our dataset is for a one-week time period. The support for those items can be calculated as 35/7500 = 
0.0045. The minimum confidence for the rules is 20% or 0.2. Similarly, we specify the value for lift as 3 and 
finally min_length is 2 since we want at least two products in our rules. These values are mostly just arbitrarily 
chosen, so you can play with these values and see what difference it makes in the rules you get back out. 
Execute the following script: 
association_rules = apriori(records, min_support=0.0045, min_confidence=0.2, min_lift=3, min_length=2) 
association_results = list(association_rules) 
In the second line here we convert the rules found by the apriori class into a list since it is easier to view the results in 
this form. 
 

### Viewing the Results
Let's first find the total number of rules mined by the apriori class. Execute the following script: 
print(len(association_rules)) 
The script above should return 48. Each item corresponds to one rule. 
Let's print the first item in the association_rules list to see the first rule. Execute the following script: 
print(association_rules[0]) 
The output should look like this: 
RelationRecord(items=frozenset({'light cream', 'chicken'}), support=0.004532728969470737, 
ordered_statistics[OrderedStatistic(items_base=frozenset({'light cream'}), items_add=frozenset({'chicken'}), 
confidence=0.29059829059829057, lift=4.84395061728395)]) 
The first item in the list is a list itself containing three items. The first item of the list shows the grocery items in the 
rule. 
For instance, from the first item, we can see that light cream and chicken are commonly bought together. This makes 
sense since people who purchase light cream are careful about what they eat, hence they are more likely to buy 
chicken i.e., white meat instead of red meat i.e., beef. Or this could mean that light cream is commonly used in recipes 
for chicken. 
The support value for the first rule is 0.0045. This number is calculated by dividing the number of transactions 
containing light cream divided by total number of transactions. The confidence level for the rule is 0.2905 which 
shows that out of all the transactions that contain light cream, 29.05% of the transactions also contain chicken. Finally, 
the lift of 4.84 tells us that chicken is 4.84 times more likely to be bought by the customers who buy light cream 
compared to the default likelihood of the sale of chicken. 
The following script displays the rule, the support, the confidence, and lift for each rule in a clearer way: 
for item in association_rules: 
    # first index of the inner list 
    # Contains base item and add item 
    pair = item[0]  
    items = [x for x in pair] 
    print("Rule: " + items[0] + " -> " + items[1]) 
    #second index of the inner list 
    print("Support: " + str(item[1])) 
    #third index of the list located at 0th 
    #of the third index of the inner list 
    print("Confidence: " + str(item[2][0][2])) 
    print("Lift: " + str(item[2][0][3])) 
    print("=====================================") 

If you execute the above script, you will see all the rules returned by the apriori class. The first four rules returned by 
the apriori class look like this: 
Rule: light cream -> chicken 
Support: 0.004532728969470737 
Confidence: 0.29059829059829057 
Lift: 4.84395061728395 
===================================== 
Rule: mushroom cream sauce -> escalope 
Support: 0.005732568990801126 
Confidence: 0.3006993006993007 
Lift: 3.790832696715049 
===================================== 
Rule: escalope -> pasta 
Support: 0.005865884548726837 
Confidence: 0.3728813559322034 
Lift: 4.700811850163794 
===================================== 
Rule: ground beef -> herb & pepper 
Support: 0.015997866951073192 
Confidence: 0.3234501347708895 
Lift: 3.2919938411349285 
===================================== 
We have already discussed the first rule. Let's now discuss the second rule. The second rule states that mushroom 
cream sauce and escalope are bought frequently. The support for mushroom cream sauce is 0.0057. The confidence 
for this rule is 0.3006 which means that out of all the transactions containing mushroom, 30.06% of the transactions 
are likely to contain escalope as well. Finally, lift of 3.79 shows that the escalope is 3.79 more likely to be bought by 
the customers that buy mushroom cream sauce, compared to its default sale. 
 
 
 
 
 
### Program: 
```import numpy as np 
#import matplotlib.pyplot as plt 
import pandas as pd 
from apyori import apriori 
store_data = pd.read_csv('C:\\Users\\Welcome\\Downloads\\store_data1.csv') 
store_data.head() 
records = [] 
for i in range(0, 3): 
    records.append([str(store_data.values[i,j]) for j in range(0, 20)]) 
association_rules = apriori(records, min_support=0.0045, min_confidence=0.2, min_lift=3, min_length=2) 
association_results = list(association_rules) 
print(len(association_results)) 
print(association_results[0]) 
for item in association_results: 
    pair = item[0]  
    items = [x for x in pair] 
    print("Rule: " + items[0] + " -> " + items[1]) 
    print("Support: " + str(item[1])) 
    print("Confidence: " + str(item[2][0][2])) 
    print("Lift: " + str(item[2][0][3])) 
    print("=====================================") 
 
 
```    
 
### Output: 
 
 
 <img width="337" height="557" alt="image" src="https://github.com/user-attachments/assets/e885f01c-3af3-4bfb-99c9-ef14c5a61d01" />

 
 
### Result: 
             Thus, the python program for implementing Apriori Algorithm has been executed successfully. f
