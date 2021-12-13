# Churn-Prediction--Survival-Analysis
Solution to Analytics Vidhya Job-a-thon November Hackathon


## Problem Statement
In recent years, attention has increasingly been paid to human resources (HR), since worker quality and skills represent a growth factor and a real competitive advantage for companies. After proving its mettle in sales and marketing, artificial intelligence is also becoming central to employee-related decisions within HR management. Organizational growth largely depends on staff retention. Losing employees frequently impacts the morale of the organization and hiring new employees is more expensive than retaining existing ones. 

You are working as a data scientist with HR Department of a large insurance company focused on sales team attrition. Insurance sales teams help insurance companies generate new business by contacting potential customers and selling one or more types of insurance. The department generally sees high attrition and thus staffing becomes a crucial aspect. 

To aid staffing, you are provided with the monthly information for a segment of employees for 2016 and 2017 and tasked to predict whether a current employee will be leaving the organization in the upcoming two quarters (01 Jan 2018 - 01 July 2018) or not, given:
1. Demographics of the employee (city, age, gender etc.)
2. Tenure information (joining date, Last Date)
3. Historical data regarding the performance of the employee (Quarterly rating, Monthly business acquired, designation, salary)


## Data Dictionary

### Train Data
Variable | Definition
--- | ---
MMMM-YY | Reporting Date (Monthly)
Emp_ID | Unique id for employees
Age | Age of the employee  
Gender | Gender of the employee
City | City Code of the employee
Education_Level | Education level : Bachelor, Master or College
Salary | Salary of the employee
Dateofjoining | Joining date for the employee
LastWorkingDate | Last date of working for the employee
Joining Designation | Designation of the employee at the time of joining
Designation | Designation of the employee at the time of reporting
Total_Business_Value | The total business value acquired by the employee in a month (negative business indicates cancellation/refund of sold insurance policies)
Quarterly Rating | Quarterly rating of the employee: 1,2,3,4 (higher is better)

### Test Data
Variable | Definition
--- | ---
Emp_ID | Unique identifier for a row


## Data Pre-processing
1. Total number of days that the employee has worked is calculated using reporting date, next reporting date, date of joining and last working date(if the employee left). 
2. Churn is determined based on not null values in last working date column. 
3. The train dataframe is grouped using ‘Emp_ID’. The values of the features for the entire 2 years are aggregated for each employee. 
4. Encoding categorical variables
   a. Gender column is one-hot encoded. 
   b. City is a column with high cardinality. Hence, it is encoded using frequency encoding.
   c. Education column is an ordinal column. It is converted to a numerical column by cat codes. 
5.  Rows from train dataset are merged into test dataset using Emp_ID as primary key.


## Evaluation Metric
The evaluation metric for this competition is macro f1_score. 


## Approach
- Since this is a churn prediction (classification) problem with time series data, survival analysis will be an ideal approach.
- Cox Proportional Hazards Regression is used here for prediction of employee churn for the first two quarters of 2018.
- Attrition (Churn) is considered as event and total number of days worked is the duration for the model. 
- Churn is determined from survival function on the 181th day. If the probability of survival is lower than 0.5, then the employee will leave the company within the first 6 months of 2018.

### Models & Model Evaluation
- The model used is Cox Proportional Hazards Regression
- The model performance is evaluated using K-fold cross validation with concordance index as score.
- For the leaderboard standings of the competition, Macro F1 score is used to evaluate the model using the predicted values.


## Leaderboard
- [Public Leaderboard](https://datahack.analyticsvidhya.com/contest/job-a-thon-november-2021/#LeaderBoard): 51st Rank
- [Private Leaderboard](https://datahack.analyticsvidhya.com/contest/job-a-thon-november-2021/#LeaderBoard): 44th Rank


