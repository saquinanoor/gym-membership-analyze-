# GYM-MEMBERSHIP-ANALYSIS-WITH-SQL
![1](https://github.com/saquinanoor/gym-membership-analyze-/blob/main/1.jpg)

## overview
This project to analyze customer membership, their behavior, trend and gym service performance (Personal Trainee,  Group Lesson) for the gym business to managing and
upgrade their service for better future

## About project
This Exploratory Data Analysis (EDA) project demonstrates my ability to work with SQL queries.
In this project I fully use PostgreSQL to process data and analyze gym membership data to answer Data Exploration - Key Questions.
I will analyze to find out the value and things that can be known about the Trend & Behavior of gym members 
for the development of the gym's business in managing their membership.

## Dataset
This Dataset from Kaggle : https://www.kaggle.com/datasets/ka66ledata/gym-membership-dataset
![4](https://github.com/saquinanoor/gym-membership-analyze-/blob/main/4.jpg)

## Dependencies
PostgreSQL

## KEY-QUESTION, QUERY & INSIGHT
IMPORT DATA FROM CSV
```
CREATE TABLE "Data".gm (
id INTEGER PRIMARY KEY,
gender VARCHAR(6),
birthday DATE,
Age VARCHAR(2),
abonoment_type VARCHAR(8),
visit_per_week INTEGER,
days_per_week VARCHAR (50),
attend_group_lesson BOOL,
fav_group_lesson VARCHAR(50),
avg_time_check_in TIME,
avg_time_check_out TIME,
avg_time_in_gym INTEGER,
drink_abo BOOL,
fav_drink VARCHAR (50),
personal_training BOOL,
name_personal_trainer  VARCHAR (10),
uses_sauna BOOL
);
```

1. Count the age, gender and abonoment type by age of gym member
```
SELECT DISTINCT Age, gender, abonoment_type, COUNT (*) as Total_by_ages FROM "Data".gm 
GROUP BY Age, gender, abonoment_type
ORDER BY total_by_ages DESC;
```
insight : shows variations abanoment types of gym members based on age and gender

2. Find the busiest to quietest days of the gym by gym membership
```
WITH CTE AS (
	SELECT
		id,
		TRIM(unnest(string_to_array(days_per_week,',')))::VARCHAR AS days
	FROM "Data".gm
	)
SELECT
	DISTINCT days, COUNT (days) as total_days
FROM CTE
GROUP BY days
Order by total_days Desc;
```
insight : Analyze membership customers' favorite days

3. List the Favorite group lesson
```

'''
 Find the average number of visits a gym member within in a week
 Find the average check-in time by gym members (devide into 3 Shift)
 Find the average check-out time by gym members (devide into 3 Shift)
 Find the average membership time spent at the gym
 List membership gym's favorite drink flavour
 Find the gym member who use or not use a personal tariner by gender of gym member
 List the top 3 of Favorite Personal Traineer
 Get the total number of gym member who use sauna (from 1000 member)
 List the top 5 most active Premium membership 
 List the top 5 most active Standard membership