# Case_Sleep_Health_Life_Style
## Objective of the Analysis
This report explores the relationship between sleep, lifestyle, and overall health in a diverse population. By analyzing variables such as sleep duration and quality, physical activity levels, stress, age, gender, and sleep disorders, the goal is to identify patterns that can contribute to improving quality of life.

## Report made with BigQuery and Google Looker Studio

[Report Looker Studio](https://lookerstudio.google.com/s/vzxNeVTvArY)

[PDF of report Sleep](https://github.com/Barbara-Lizama/Case_Sleep_Health_Life_Style/blob/master/Report/Report%20Sleep_Life_Health.pdf)

## Queries made with BigQuery

### How many data points does the dataset have? 
```bash
SELECT COUNT(* )
FROM `Sleep_health.Sleep_health_life` 
```
Results: 374 records

### What are the unique values in each column?
```bash
SELECT DISTINCT(Age) AS range_age
FROM `Sleep_health.Sleep_health_life`
ORDER BY range_age DESC 
```
Results: 31 unique values

```bash
SELECT DISTINCT(Occupation) 
FROM `Sleep_health.Sleep_health_life`
```
Results: 11 unique values

```bash
SELECT DISTINCT(Sleep_Duration) 
FROM `Sleep_health.Sleep_health_life` 
ORDER BY Sleep_Duration DESC
```
Results: 27 unique values

```bash
SELECT DISTINCT(BMI_Category) 
FROM `Sleep_health.Sleep_health_life`
```
Results: 4 unique values

```bash
SELECT DISTINCT(Daily_Steps) 
FROM `Sleep_health.Sleep_health_life`
ORDER BY Daily_Steps DESC  
```
Results: 20 unique values

### How the Age column is divided?
```bash
SELECT Age, COUNT(*) AS Total_Age
FROM `Sleep_health.Sleep_health_life` 
GROUP BY Age 
```
![alt text](https://github.com/Barbara-Lizama/Case_Sleep_Health_Life_Style/blob/master/Plots/Total_Age%20by%20Age.png?raw=true)

### Average sleep duration by BMI category?
```bash
SELECT BMI_Category, AVG(Sleep_Duration) AS Avg_Sleep_Duration
FROM `Sleep_health.Sleep_health_life` 
GROUP BY BMI_Category
ORDER BY Avg_Sleep_Duration DESC
```

![alt text](https://github.com/Barbara-Lizama/Case_Sleep_Health_Life_Style/blob/master/Plots/Avg_Sleep_Duration%20by%20BMI_Category.png?raw=true)

### Relationship between stress level and occupation
```bash
SELECT Occupation, ROUND(AVG(Stress_Level),1) AS Avg_Stress_Level  
FROM `Sleep_health.Sleep_health_life` 
GROUP BY Occupation
ORDER BY Avg_Stress_Level DESC
LIMIT 10
```

![alt text](https://github.com/Barbara-Lizama/Case_Sleep_Health_Life_Style/blob/master/Plots/Avg_Stress_Level%20by%20Occupation.png?raw=true)

### Relationship between stress level and Age 
```bash
SELECT Age, ROUND(AVG(Stress_Level),1) AS Avg_Stress_Level  
FROM `Sleep_health.Sleep_health_life` 
GROUP BY Age
ORDER BY Avg_Stress_Level

```

![alt text](https://github.com/Barbara-Lizama/Case_Sleep_Health_Life_Style/blob/master/Plots/Avg_Stress_Level%20by%20Age.png?raw=true)

### Average sleep quality by age group
```bash
SELECT 
    CASE 
        WHEN Age BETWEEN 18 AND 25 THEN '18-25'
        WHEN Age BETWEEN 26 AND 35 THEN '26-35'
        WHEN Age BETWEEN 36 AND 45 THEN '36-45'
        WHEN Age BETWEEN 46 AND 60 THEN '46-60'
        ELSE '60+' 
    END AS Age_Group,
    ROUND(AVG(Quality_Sleep),1) AS Avg_Quality_Sleep
FROM `Sleep_health.Sleep_health_life`
GROUP BY Age_Group
ORDER BY Age_Group;

```

![alt text](https://github.com/Barbara-Lizama/Case_Sleep_Health_Life_Style/blob/master/Plots/Avg_Quality_Sleep%20by%20Age_Group.png?raw=true)

### Average blood pressure by age group
```bash
SELECT
  CASE
    WHEN Age BETWEEN 18 AND 29 THEN '18-29'
    WHEN Age BETWEEN 30 AND 39 THEN '30-39'
    WHEN Age BETWEEN 40 AND 49 THEN '40-49'
    WHEN Age BETWEEN 50 AND 59 THEN '50-59'
    WHEN Age BETWEEN 60 AND 69 THEN '60-69'
    WHEN Age BETWEEN 70 AND 79 THEN '70-79'
    ELSE '80+'
  END AS Age_Group,
  ROUND(AVG(CAST(SPLIT(Blood_Pressure, '/')[OFFSET(0)] AS FLOAT64)), 1) AS Avg_Systolic,
  ROUND(AVG(CAST(SPLIT(Blood_Pressure, '/')[OFFSET(1)] AS FLOAT64)), 1) AS Avg_Diastolic
FROM
  `Sleep_health.Sleep_health_life`
WHERE
  Blood_Pressure IS NOT NULL
GROUP BY
  Age_Group
ORDER BY
  Age_Group;

```

![alt text](https://github.com/Barbara-Lizama/Case_Sleep_Health_Life_Style/blob/master/Plots/Avg_Systolic%2C%20Avg_Diastolic%20by%20Age_Group.png?raw=true)

### Average blood pressure by BMI category
```bash
SELECT
  BMI_Category,
  ROUND(AVG(CAST(SPLIT(Blood_Pressure, '/')[OFFSET(0)] AS FLOAT64)), 1) AS Avg_Systolic,
  ROUND(AVG(CAST(SPLIT(Blood_Pressure, '/')[OFFSET(1)] AS FLOAT64)), 1) AS Avg_Diastolic
FROM `Sleep_health.Sleep_health_life`
GROUP BY BMI_Category;

```

![alt text](https://github.com/Barbara-Lizama/Case_Sleep_Health_Life_Style/blob/master/Plots/Avg_Systolic%2C%20Avg_Diastolic%20by%20BMI_Category.png?raw=true)
