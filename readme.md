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
WITH CTE AS (
	SELECT
		id,
		TRIM(unnest(string_to_array(fav_group_lesson,',')))::VARCHAR AS lessons
	FROM "Data".gm
	)
SELECT
	DISTINCT lessons, COUNT (lessons) as total_lessons
FROM CTE
GROUP BY lessons
```
insight : Analyze membership customers' favorite group lesson

4. Find the average number of visits a gym member within a week
```
SELECT ROUND(AVG(visit_per_week),0)as avg_visit from "Data".gm
```
insight : recognize average member visit the gym

5. Find the average check-in time by gym members (devide into 3 Shift)
```
SELECT
COUNT(CASE 
WHEN avg_time_check_in >= '08:00:00' AND avg_time_check_in <= '12:00:00'
THEN 'Shift1'
END) As shift_8_12,
COUNT(CASE 
WHEN avg_time_check_in >= '12:00:00' AND avg_time_check_in <= '16:00:00'
THEN 'Shift2'
END) As shift_12_16,
COUNT(CASE
WHEN avg_time_check_in >= '16:00:00' AND avg_time_check_in <= '21:00:00'
THEN 'Shift3'
END) As shift_16_21
FROM "Data".gm;
```
insight : Identify check-in times into 3 shifts and see which times are the busiest and need to be paid attention to

6. Find the average check-out time by gym members (devide into 3 Shift)
```
SELECT
COUNT(CASE 
WHEN avg_time_check_out >= '08:00:00' AND avg_time_check_in <= '12:00:00'
THEN 'Shift1'
END) As shift_8_12,
COUNT(CASE 
WHEN avg_time_check_out >= '12:00:00' AND avg_time_check_in <= '16:00:00'
THEN 'Shift2'
END) As shift_12_16,
COUNT(CASE
WHEN avg_time_check_out >= '16:00:00' AND avg_time_check_in <= '21:00:00'
THEN 'Shift3'
END) As shift_16_21
FROM "Data".gm;
```
insight : Identify check-in times into 3 shifts and see which times are the busiest and need to be paid attention to

7. Find the average membership spent time at the gym
```
SELECT AVG(avg_time_check_out-avg_time_check_in) as avg_time from "Data".gm
```
insight : recognize average member spent time at the gym

8. List membership gym's favorite drink flavour
```
WITH ud AS (
	SELECT
		id,
		TRIM(unnest(string_to_array(fav_drink,',')))::VARCHAR AS flavour_drink
	FROM "Data".gm
	)
SELECT
	DISTINCT flavour_drink, COUNT (flavour_drink) as total_drink
FROM ud
GROUP BY flavour_drink
Order by total_drink Desc;
```
insight : analyze member’s favorite drink

9. Find the gym member who use or not use a personal trainer by gender of gym member
```
SELECT gender, personal_training,COUNT (*) FROM "Data".gm Group BY gender,personal_training
```
insight : shows gym member based on use or not use personal trainer

10. List the top 3 of Favorite Personal Traineer
```
SELECT name_personal_trainer, COUNT (*)FROM "Data".gm 
WHERE name_personal_trainer IS NOT NULL
GROUP BY name_personal_trainer 
ORDER BY count DESC LIMIT 3;
```
insight : analyze top 3 of member’s favorite personal trainer 

11. Get the total number of gym member who use sauna (from 1000 member)
```
SELECT uses_sauna, COUNT (*) FROM "Data".gm GROUP BY uses_sauna;
```
insight : shows the member behavior based on use or not use sauna

12. List the top 5 most active Premium membership
```
SELECT id,gender, age, visit_per_week FROM "Data".gm
Where abonoment_type = 'Premium'
AND visit_per_week >=3 --ikutin avg visit
AND attend_group_lesson = 'TRUE'
AND avg_time_in_gym >=105 --ikutin avg waktuk d gym
AND drink_abo = 'TRUE'
AND personal_training = 'TRUE'
GROUP BY id, gender, age, visit_per_week
ORDER BY visit_per_week DESC LIMIT 5;
```
insight : analyze and identify the activeness of  Premium gym members in terms of the number of visits, and the average use of gym facilities they use compared to other members

13. List the top 5 most active Standard membership
```
SELECT id,gender,age, visit_per_week FROM "Data".gm
Where abonoment_type = 'Standard'
AND visit_per_week >=3 --ikutin avg visit
AND attend_group_lesson = 'TRUE'
AND avg_time_in_gym >=105 --ikutin avg waktuk d gym
AND drink_abo = 'TRUE'
AND personal_training = 'TRUE'
GROUP BY id, gender, age, visit_per_week
ORDER BY visit_per_week DESC LIMIT 5;
```
insight : analyze and identify the activeness of Standard gym members in terms of the number of visits, and the average use of gym facilities they use compared to other members
