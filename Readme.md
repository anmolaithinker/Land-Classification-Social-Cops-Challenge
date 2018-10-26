# Task - Land Classification

## Column Description:
- Numeric columns *X1 to X6 and I1 to I6* define characteristics about the land piece
- *target *is the output categorical column which needs to be found for the test dataset
    * 1 = Green Land
    * 2 = Water
    * 3 = Barren Land
    * 4 = Built-up 

---------------------------------------------------------------------------------------------------------

# Explanation of My Solution to the given task :

I have tried all the possibilites and check the model performance by training the model on the preprocessed dataset.

-----------------------------------------------------------------------------------------------

## EDA (Exploratory Data Analysis)

1. Checking whether there are any null values.
2. Checking the description of data.
3. Correlated Features :
     1. PairPlot
     2. HeatMap 
4. Checking the distribution Percentage of the target variable.
5. Checking of Outliers using Box Plot.


## Procedure that I have done with Observations: 

#### 1. Checking the null counts
- No Null Counts are present in the dataset

#### 2. Checking the description of the dataset
- mean value, std , minimum value and maximum value is small in all I columns and very much larger in X Columns.
 - To handle this standardization of features are done.
 
#### 3. Checking What are the Correlated Features in the Dataset ?
- Graphs made to visualize the correlation between features.
 - PairPlot
 - Correlation HeatMap
- I have Grouped together those features whose pearson correlation value >0.90 or <-0.90 . Threshold value = 0.90 is taken here .
 - [ X1 , X2 , X3 ] are correlated heavily 
 - [ X5 , X6 ] are correlated heavily
 - [ I1 , I3 , I4 ] are correlated heavily
 
#### 4. I have made a new dataset after taking only one feature among these correlated features.
- I have used Mutual Information to select the best feature which is more dependant on target variable.
 - Mutual information (MI) between two random variables is a non-negative value, which measures the dependency between the variables. It is equal to zero if and only if two random variables are independent, and higher values mean higher dependency.
- I1 is best among I1,I3,I4
- X3 is best among X1,X2,X3
- X6 seems to be the best among X5,X6
 
#### 5. Removing correlated features
- After Removing features we get : X3 , X4 , X6 , I1 , I2 , I5 , I6 

#### 6. I have seen the distribution of target variables.
- Observations (Imbalanced distribution of target variable):
 - Percentage of 1 : 45.4545454545
 - Percentage of 2 : 27.2727272727
 - Percentage of 3 : 18.1818181818
 - Percentage of 4 : 9.09090909091
- To Handle this problem I have used oversampling technique - SMOTE

#### 7. Now I have trained the models on the preprocessed data in which I have removed the correlated features.
- Models that are trained Along with the Observations : 
 - Logistic Regression : 
   - Accuracy : Training Accuracy ( 89 % ) , Testing Accuracy (89 % ) 
    - Precision (Test Set) : 0.90 
    - Recall (Test Set):  0.89
    - F1 Score ( weighted ) (Test Set): 0.89

 - SVM : 
   - Accuracy : Training Accuracy ( 91 % ) , Testing Accuracy (91 % ) 
   - Precision (Test Set): 0.92
   - Recall (Test Set): 0.91
   - F1 Score ( weighted ) (Test Set): 0.92

 - XGBoost : 
   - Accuracy : Training Accuracy (96.22 % ) , Testing Accuracy (96.07 % ) 
   - F1 Score ( weighted )(Test Set) :  0.9612
   
   
### Note Very Important Thing :

- We have just removed the correlated features from the dataset . But we should not just remove the features directly from the dataset as there will be loss of information which they will provide the model to classify.

- Instead just removing the features from the dataset. We should train the model with L2 penalty keeping all the correalted features in the dataset. This is the best practises to train the model with correlated features.   


#### 8.  I check the model performance by training it to preprocessed data without removing the correlated features.
- Reason : 
 - Check Above Note.
 - By Checking out the performance of the model ,I found out that dataset with correlated features gives the model more information to classify than dataset having removed correlated features.
 
- Models that are trained Along with the Observations :
 - Logistic Regression
   - Accuracy : Training Accuracy ( 93 % ) , Testing Accuracy (94 % ) 
   - Precision (Test Set) : 0.94
   - Recall (Test Set): 0.94
   - F1 Score ( weighted ) (Test Set) : 0.94

 - SVM
   - Accuracy : Training Accuracy (94 % ) , Testing Accuracy (94 % ) 
   - Precision (Test Set): 0.95
   - Recall (Test Set): 0.94
   - F1 Score ( weighted ) (Test Set):  0.94

 - XGBoost
   - Accuracy : Training Accuracy ( 96.92% ) , Testing Accuracy ( 96.79% ) 
   - F1 Score ( weighted ) (Test Set):  0.9683
   - Plot Importance Graph:
     - It shows that I1 is the most important feature of all

#### Till now I observed that removing correlated features in this dataset will not benefit the models to classify more accurate.

#### 9. Now I have Visualized the Outliers inside the dataset :
- Graphs :
 - BoxPlot
 - I Observed that X1 , X2 , X3 , X4 , X5 , X6 , I6 contains Outliers.

#### 10. Remove the Outliers from the dataset.

#### 11. Train the model on this dataset and check the model performance : 

- Logistic Regression :  
 - Accuracy :   Training Set ( 94% ) and Testing Set( 94% )
 - Precision(weighted) : Testing Set ( 0.95 )
 - Recall(weighted) : Testing Set ( 0.94 )
 - F1 Score(weighted) : Testing Set ( 0.94 )

- SVM : 
 - Accuracy : Training Set ( 94% ) and Testing Set( 94% )
 - Precision(weighted) : Testing Set( 0.95 )
 - Recall(weighted) : Testing Set( 0.94 )
 - F1 Score(weighted) : Testing Set( 0.95 )


- XGBoost : 
 - Accuracy : Training Set ( 97.10% ) and Testing Set( 96.87% )
 - F1 Score(weighted) : Testing Set( 0.9691 )

#### I Observed that by removing outliers , model performance on testing dataset slightly increases.

##### So, after going through all the procedures above , I conclude that removing of correlated features will not benefit us whereas
##### removing of outliers from the dataset will slightly benefit us.

#### 12. Now I have done all the preprocessing tasks. Now it's time to make efficient and powerful model for this task.
- I have used one of the Ensembling Technique , known as Stacking 
- Description of My Stacked Model : 
 - It Contains 2 Layers
   - First Layer consists of 3 models : Logistic Regression , SVM , GradientBoostingClassifier.
   - Second Layer consists of 1 model : XGBoost.
- Accuracy : 97.14%
- F1 Score : 0.9717   



## Files : 
- ChallengeSolution.ipynb
 - It is the file where I have done all the procedures for this task with well documentation.
- ChallengeSolution.html 
 - It is the HTML file of ChallengeSolution.ipynb
- Submit.csv
 - It is the file which contains prediction for land_test.csv

# Note
#### In Test File there are 20,00,000 rows which takes very much time even to load it.
#### I will just take first 2,00,000 rows of data to make predictions. 
#### I can also use colab for this, But I start this task on my laptop so I can't shift to the colab.
