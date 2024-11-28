# Goal and Introduction

The primary objective of this case study is to create a comprehensive report using fictitious data from a technology company called **Atlas Labs**. The HR team at Atlas Labs aims to monitor key employee metrics, and as a secondary goal, to understand the factors influencing employee attrition.

---

# Data Download and Preparation

The dataset consists of four tables: **Education Level**, **Employee**, **Performance Rating**, **Rating Level**, and **Satisfaction Level**. These tables will be designated as **Dim** tables, while the **Performance Rating** table will serve as the **Fact** table. Data will be reviewed to ensure proper formatting and naming conventions.

The next step involves modeling the data and establishing relationships between the tables. A **Date Table** will also be created to facilitate time-based analysis.

---

# Data Exploration and Dashboard Design

After preparing the data, the next step is to explore it and design an overview page. The following key metrics and visualizations will be created:

## Overview Visualizations

1. **Employee Count Cards**: Displaying the total number of employees, active employees, and inactive employees.
2. **Attrition Rate Bar Chart**: Showing the attrition rate over time.
3. **Employee Distribution by Department**: A column chart displaying active employees by department.
4. **Employee Hiring Trends**: A line chart showing employee hiring trends by year, with a filter for attrition status (Yes/No).
5. **Active Employees by Department and Job Role**: A Tree Map to show the highest number of employees in the Technology department, particularly in Software Engineering roles.

## Demographic Visualizations

1. **Active Employees by Age**: A bar chart displaying active employees by age group, with a filter for attrition status.
2. **Employee Distribution by Age and Gender**: A column chart illustrating employees by age and gender.
3. **Employee Distribution by Marital Status**: A donut chart displaying employees by marital status.
4. **Oldest and Youngest Employees**: Cards showing the oldest and youngest employees, with values being 51 and 18, respectively.

---

# DAX Measures and Their Explanations

The following DAX measures will be used to calculate key metrics:

1. **% Attrition Rate**  
   Formula: `DIVIDE([Inactive Employees], [Active Employees] + [Inactive Employees], 0)`  
   Calculates the percentage of employees who have left the organization.

2. **% Attrition Rate by Date**  
   Formula: `DIVIDE([Inactive Employees Date], [Total Employees Date], 0) * 100`  
   Tracks the attrition rate over time as a percentage.

3. **Active Employees**  
   Formula: `COUNTROWS(FILTER(DimEmployee, DimEmployee[Attrition] = "No"))`  
   Counts the number of active employees.

4. **Average Salary**  
   Formula: `AVERAGE(DimEmployee[Salary])`  
   Calculates the average salary of employees.

5. **Environment Satisfaction**  
   Formula: `CALCULATE(MAX(FactPerformanceRating[EnvironmentSatisfaction]), USERELATIONSHIP(FactPerformanceRating[EnvironmentSatisfaction], DimSatisfiedLevel[SatisfactionLevel]))`  
   Captures the maximum environment satisfaction level among employees.

6. **Inactive Employees**  
   Formula: `COUNTROWS(FILTER(DimEmployee, DimEmployee[Attrition] = "Yes"))`  
   Counts the number of employees who have left the organization.

7. **Job Satisfaction**  
   Formula: `MAX(FactPerformanceRating[JobSatisfaction])`  
   Represents the highest job satisfaction level among employees.

8. **Last Review Date**  
   Formula: (See code for details)  
   Determines the date of the last employee review or indicates if no review has occurred.

9. **Manager Rating**  
   Formula: `CALCULATE(MAX(FactPerformanceRating[ManagerRating]), USERELATIONSHIP(FactPerformanceRating[ManagerRating], DimRatingLevel[RatingLevel]))`  
   Represents the highest manager rating level.

10. **Work-Life Balance**  
    Formula: `CALCULATE(MAX(FactPerformanceRating[WorkLifeBalance]), USERELATIONSHIP(FactPerformanceRating[WorkLifeBalance], DimSatisfiedLevel[SatisfactionLevel]))`  
    Represents the highest work-life balance level among employees.

---

# Performance Tracker Page

A dedicated page will track performance metrics, including:

1. **Employee Review Dates**: Displaying cards for the start date, last review date, and next review date.  
2. **Line Chart for Satisfaction Metrics**: Job Satisfaction, Relationship Satisfaction, Self-Rating, Environmental Satisfaction, Work-Life Balance, and Manager Rating—tracked annually.  
3. **Tables for Satisfaction and Rating Details**: Two tables will provide more detailed insights into satisfaction levels and ratings, including **Satisfaction Level ID** and **Rating Level ID**.

---

# Attrition Page

The final page will focus on **attrition**, including:

1. **Attrition Rate Card**: Displaying the overall attrition rate.  
2. **Column Chart for Attrition Rate by Department and Job Role**: The highest attrition rate is observed in the **Sales** department, with **Sales Representatives** having the highest attrition rate at 39%.  
3. **Line Chart for Attrition by Hire Date**: The chart will show that the highest attrition occurred in **2022**, with **22%** of employees leaving that year.

---

# Key Insights 🧠

- **Atlas Labs** has employed over **1,470** people in total.
- The company currently has **1,200** active employees.
- The largest department is **Technology**.
- The overall **attrition rate** is **16%**.
- The majority of employees are aged **20-29**.
- **Women** represent only **2.7%** of the workforce, while **non-binary** employees make up **8.5%**.
- **White employees** have the highest average salary, while employees from **mixed or multiple ethnic backgrounds** have one of the lowest average salaries.
- The **Sales department**, particularly **Sales Representatives**, has the highest attrition rate at **39%**.
- **2022** had the highest attrition rate, with **22%** of employees leaving that year.

---

