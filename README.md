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
###Regression Models - 

###Classification Models - 

## Data Cleaning
1. Remove the 'index' column; this is an artifact of the database structure that we will not be using.
2. Perform data quality check on attributes (consistency of values)
3. Check data type of each attribute. Convert to appropriate dtype as needed.
4. Set the 'student_id' attribute, as dataframe index. <br> Our assumption for this action is that the student_id is a fully random element for each student and has no bearing on their final math score, hence will not be further analyzed on used for modelling. Prior to this, we will first check for duplicate rows.
5. Check for null/missing values among attributes

## Feature Engineering
4. Create 3 new columns: First is 'hours_rest' per student, based on sleep time and wake time. Remove 'sleep_time' and 'wake_time' as we will no longer need it these columns. Second and third column is number of student of the same and opposite gender, based on 'n_male' and 'n_female' columns'.







## 9.0 Requirements

**System:**<br>
* Python 3.6.8 <br>
* Jupyter Notebook

**Python Libraries Required:**<br>
* numpy==1.18.1
* pandas==0.24.2
* tensorflow==2.1.0
