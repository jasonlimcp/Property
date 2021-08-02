## AIAP #9 Technical Assessment
### Jason Lim Chong Poh (cplim87@hotmail.com)

## Overview of Submitted Folder
The folder contains the following files:
1. run.sh <br>
This script installs the packages in 'requirements.txt', and executes both 'main.py' to perform data cleaning and modelling. <br>

2. [requirement.txt](requirement.txt) <br>
Lists all python packages necessary to run the script.<br>

3. [README.md](README.md) <br>
Documentation for this project. <br>

4. [mlp/main.py](mlp/main.py) <br>
This script runs the data cleaning and ML model modules.

## Instructions for Executing the Pipeline
We will be spliting the dataset into train-test set on 1 September 2012, any date before will be training, any date after will be test set. <br>
![Logo](https://user-images.githubusercontent.com/34482689/76684815-070b1000-664a-11ea-9184-6a2f9b03ff82.PNG)
<br>
Train set: **1 Jan 2011 to 30 Aug 2012 (16 months)**<br>
Test set: **1 Sep 2012 to 31 Dec 2012 (4 months)**

## 3.0 Logical Flow for the Pipeline

Exploratory Data Analysis can be found in file [eda.ipynb](eda.ipynb)

## 3.0 Key Findings for the EDA

**EDA Findings**

1. Students who undergo tuition spend more hours per week studying than students who do not. It is unclear whether tuition time is included in the study hours attribute, however based on subsequent pairwise analysis, there is no strong correlation. Interestingly, students who take up tuition are more likely to have an Auditory learning style. Students who take tuition are also consistently scoring better in exams.
2. Across both genders, students with no CCA overwhelmingly scored better in the Math exam. The difference is solely between having a CCA and not having one; there is no significant score difference between CCAs. There is also no significant disparity in CCA vs Exam performance between genders.
3. There does not appear to be any meaningful relation to how many classmates of the opposite gender a student has, to the amount of time a student spend studying Math. This is true for both Genders.
4. While the vast majority of students get 8 hours of sleep, it appears that students who sleep fewer hours, from less than 7 to even 4 a night, have a lower attendance rate the fewer hours they sleep.
5. A student's particular bag color does not appear to affect their final test score: it does seem however that having multiple bag colors might have a relationship. The reasons for this can only be speculated.

## 4.0 Choice of ML Models and Evaluation
In this notebook, we will be using **Mean Absolute Error (MAE)** as the performance metrics. MAE will be calculated seperately for both registered-user prediction and guest-user.

![Logo](https://user-images.githubusercontent.com/34482689/76684824-19854980-664a-11ea-9fcc-0068639aabb1.jpg)

MAE is chosen as the target variable is continueous and we do not want to over penalize outliers. Also, MAE is the most intuitive metric as we can directly use it to check the standard error of |the model.

## 5.0 Data Cleaning

1. All spelling mistakes in weather field are cleaned up

```
data = data.replace({'weather': {'loudy': 'cloudy', 'lear': 'clear',
                'CLEAR': 'clear', 'CLOUDY': 'cloudy',
                'LIGHT SNOW/RAIN': 'light snow/rain'}}
```

2. Date field is convert to date format and date + hr field is convert to datetime format

```
data['date'] = pd.to_datetime(data['date'], format = '%Y-%m-%d')
data['TDate'] = data['date'] + pd.to_timedelta(data.hr, unit='h')
```

3. Dataset is sorted according to time. This is important for time series model used later on

```
data=data.sort_values('TDate').reset_index()
```

4. Converting feels-like-temperature and feels_like_temperature_celcius to degree celcius. this is to gain better intuition on the dataset as we are more used to degree celcius

```
data['feels_like_temperature_celcius'] = (data['feels-like-temperature'].values-32) * 5/9
data['feels_like_temperature_celcius'] = data['feels_like_temperature_celcius'].round(1)
```

## 6.0 Feature Engineering

1. Create one hot encoding for weather
```
weather = pd.get_dummies(data['weather'], prefix = 'weather=')
data = pd.concat([data, weather], axis = 1)
```

2. Combine the variable heavy snow/rain and light snow/rain. Heavy snow/rain is a more intense version of light snow/rain, and hence they should not be two seperate variable. Heavy snow/rain mapped to 1, while light snow/rain mapped to 0.5
```
data['snow'] = data['weather=_heavy snow/rain'].values + 0.5*data['weather=_light snow/rain'].values
```

3. Create two variables for weekday and weekend, where weekdays is Sunday to Thursday, weekend is Friday and Saturday. Weekday and weekend are created not based on actual weekday/weekend, but rather based on usage. The usage is significantly highest on Friday/Saturday compared to Sun to Thursday.

```
data['day_of_week'] = data['date'].dt.dayofweek
data['weekday'] = np.where((data['day_of_week'].values >= 0)& (data['day_of_week'].values <=4), 1, 0)
data['weekend'] = np.where( (data['day_of_week'].values >=5), 1, 0)
```

4. Creating a two variables for time.
- Variable time from first peak: The time from first peak hour (08:00). The time is calculated accurately such that 23:00 is 9 hours away from 08:00.
- Variable time from second peak: The time from second peak hour (17:00)

```
data['time_from_first_peak_1'] = abs(data['hr'].values - 8)
data['time_from_first_peak_2'] = abs(data['hr'].values -24 - 8)
data['time_from_first_peak'] = data[['time_from_first_peak_1', 'time_from_first_peak_2']].min(axis=1)

data['time_from_second_peak_1'] = abs(data['hr'].values - 17)
data['time_from_second_peak_2'] = abs(data['hr'].values +24 - 17)
data['time_from_second_peak'] = data[['time_from_second_peak_1', 'time_from_second_peak_2']].min(axis=1)
```

## 7.0 Models Evaluation

Four models are used in this exercise. Models are only evaluated on **registered user**
1. Random Forest Regressor
2. Linear Regression
3. Ada Boost Regressor
4. Time Series - Neural Network LSTM

| Model Name      | Mean Absolute Error (MAE) |
| ----------- | ----------- |
| Random Forest Regressor      | 1129       |
| Linear Regression   | 1874        |
| Ada Boost Regressor   | 1500        |
| Time Series - Neural Network LSTM   | 925        |

**Time Series** give the best MAE score on the test set. Given the time-dependent nature of the dataset, it is not surprising that Time-Series on Neural Network performs better than traditional ML methods.

Links:<br>
[Codes for training Time Series Model](mlp/model-tf-training.ipynb) <br>

[Codes for other Models(RF, LR, Adaboost)](mlp/other-unused-models.ipynb)

## 8.0 Model Performance

| Predicted Users      | Mean Absolute Error (MAE) |
| ----------- | ----------- |
| Registered Users      | 925       |
| Guest   | 80        |

#### Registered Users - Visualization of Actual vs Predicted (Aggregated by day)
![Logo](https://user-images.githubusercontent.com/34482689/76684839-286bfc00-664a-11ea-90f6-6b98685347f3.JPG)

#### Guest Users - AVisualization of  Actual vs Predicted  (Aggregated by day)
![Logo](https://user-images.githubusercontent.com/34482689/76684850-3883db80-664a-11ea-905a-4042e69754ab.JPG)

## 8.0 Pipeline Design

1. [run.sh](run.sh) <br>
run.sh installs the packages from requirement and run the Time Series Model on Neural Network valuation script <br><br>

2. [requirement.txt](requirement.txt) <br>
All the packages and its versions necessary to run the model<br><br>

3. [README.md](README.md) <br>
Contains all information about the data, design consideration, pipeline design, and model<br><br>

4. [mlp/main.py](mlp/main.py) <br>
Runs the time series forecasting model, returns the Mean Absolute Error (MAE) of the prediction on registered rider prediction and guest rider prediction

## 9.0 Requirements

**System:**<br>
* Python 3.6.8 <br>
* Jupyter Notebook

**Python Libraries Required:**<br>
* numpy==1.18.1
* pandas==0.24.2
* tensorflow==2.1.0
