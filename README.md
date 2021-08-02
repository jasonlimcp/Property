## AIAP #9 Technical Assessment
### Jason Lim Chong Poh (cplim87@hotmail.com)

## Overview of Submitted Folder
The folder contains the following files:
1. run.sh <br>
This script installs the packages in 'requirements.txt', and executes both 'main.py' to perform data cleaning and modelling. <br>

2. requirements.txt <br>
Lists all python packages necessary to run the script.<br>

3. README.md <br>
Documentation for this project. <br>

4. src/main.py <br>
This folder contains main.py, the script which runs the associated data cleaning and ML model modules in this folder.

5. eda.ipynb <br>
This is a jupyter notebook file outlining the data preparation and data exploration process.

### Instructions for Executing the Pipeline
To execute the pipeline, simply type the following in command prompt:
<br> bash run.sh

## Logical Flow for the Pipeline

![Flow](https://github.com/jasonlimcp/Projects/blob/main/model.PNG)

## Key Findings for EDA

1. Students who undergo tuition spend more hours per week studying than students who do not. It is unclear whether tuition time is included in the study hours attribute, however based on subsequent pairwise analysis, there is no strong correlation. Interestingly, students who take up tuition are more likely to have an Auditory learning style. Students who take tuition are also consistently scoring better in exams.
2. Across both genders, students with no CCA overwhelmingly scored better in the Math exam. The difference is solely between having a CCA and not having one; there is no significant score difference between CCAs. There is also no significant disparity in CCA vs Exam performance between genders.
3. There does not appear to be any meaningful relation to how many classmates of the opposite gender a student has, to the amount of time a student spend studying Math. This is true for both Genders.
4. While the vast majority of students get 8 hours of sleep, it appears that students who sleep fewer hours, from less than 7 to even 4 a night, have a lower attendance rate the fewer hours they sleep.
5. A student's particular bag color does not appear to affect their final test score: it does seem however that having multiple bag colors might have a relationship. The reasons for this can only be speculated.

## Choice of ML Models and Evaluation
### Regression Models
<br> The models used for regression are Logistic Regression, Lasso Regression,Ridge Regression,ElasticNet Regression. The rationale for using a variety of models is due to the low cost of running for this dataset, allowing selection based on best evaluation criteria.
<br>Lasso regression is favored due as similar to Ridge, it penalizes absolute size of the regression coefficients, and is advantageous in this use case due to it being capable of improving the accuracy of the linear regression model.
<br> Evaluation metrics used were Mean Absolute Error, Root Mean Squared Errow and R-squared. Using this metrics, Ridge Regression had the best overall performance with a 58.2% r-squard value, ie. being able to explain 58% of variablity in the exam result data.

### Classification Models
<br>In contrast to regression, classification does not work for all models. In this situation, we chose to use two models, Gradient boosting and a Stacked Classifier with Naive Bayes, KNN and Decision Tree classifiers.
<br> The performance of these 2 models was poorer than for regression. The evaluation metrics we used was Precision, Recall and ultimately the F-score. In this regard, the Stacked ensemble classifier performed better, with a 0.55 weighted-avergage F-score.

## Data Cleaning Process
1. Remove the 'index' column; this is an artifact of the database structure that we will not be using.
2. Perform data quality check on attributes (consistency of values)
3. Check data type of each attribute. Convert to appropriate dtype as needed.
4. Set the 'student_id' attribute, as dataframe index.
5. Check for null/missing values among attributes

## Feature Engineering
<br> 4 new columns were created.
1. 'hours_rest' per student, based on sleep time and wake time
2. Number of student of same gender
3. Number of students of opposite gender
4. Binning of 'final_test' for classication.

## Requirements

<br>pandas==1.1.3
<br>numpy==1.19.2
<br>scikit-learn==0.24.2
