# Atlas Labs Employee Attrition Analysis - Power BI Report

## Overview

The core goal of this case study is to build an interactive report using fictitious datasets from a tech company called **Atlas Labs**. The HR team at Atlas Labs wants to monitor key metrics related to employees and, as a secondary goal, understand the factors affecting employee attrition.

## Data Preparation

The project involves four primary tables:

1. **DimEducationLevel**: Contains data related to employees' education levels.
2. **DimEmployee**: Contains employee data, including personal details, hire dates, and attrition status.
3. **DimRatingLevel**: Contains rating levels for performance and satisfaction.
4. **FactPerformanceRating**: Contains performance ratings such as job satisfaction, environmental satisfaction, and manager ratings.

These tables were renamed with the **Dim** prefix for dimension tables, and the **FactPerformanceRating** table was designated as the fact table. Additionally, a **DimDate** table was created to support time-based analysis.

### DimDate Table Code

```dax
DimDate = 
VAR _minYear = YEAR(MIN(DimEmployee[HireDate]))
VAR _maxYear = YEAR(MAX(DimEmployee[HireDate]))
VAR _fiscalStart = 4 

RETURN
ADDCOLUMNS(
    CALENDAR(DATE(_minYear,1,1), DATE(_maxYear,12,31)),
    "Year", YEAR([Date]),
    "Year Start", DATE(YEAR([Date]), 1, 1),
    "YearEnd", DATE(YEAR([Date]), 12, 31),
    "MonthNumber", MONTH([Date]),
    "MonthStart", DATE(YEAR([Date]), MONTH([Date]), 1),
    "MonthEnd", EOMONTH([Date], 0),
    "DaysInMonth", DATEDIFF(DATE(YEAR([Date]), MONTH([Date]), 1), EOMONTH([Date], 0), DAY) + 1,
    "YearMonthNumber", INT(FORMAT([Date],"YYYYMM")),
    "YearMonthName", FORMAT([Date],"YYYY-MMM"),
    "DayNumber", DAY([Date]),
    "DayName", FORMAT([Date],"DDDD"),
    "DayNameShort", FORMAT([Date],"DDD"),
    "DayOfWeek", WEEKDAY([Date]),
    "MonthName", FORMAT([Date],"MMMM"),
    "MonthNameShort", FORMAT([Date],"MMM"),
    "Quarter", QUARTER([Date]),
    "QuarterName", "Q"&FORMAT([Date],"Q"),
    "YearQuarterNumber", INT(FORMAT([Date],"YYYYQ")),
    "YearQuarterName", FORMAT([Date],"YYYY") & " Q" & FORMAT([Date],"Q"),
    "QuarterStart", DATE(YEAR([Date]), (QUARTER([Date])*3)-2, 1),
    "QuarterEnd", EOMONTH(DATE(YEAR([Date]), QUARTER([Date])*3, 1), 0),
    "WeekNumber", WEEKNUM([Date]),
    "WeekStart", [Date] - WEEKDAY([Date]) + 1,
    "WeekEnd", [Date] + 7 - WEEKDAY([Date]),
    "FiscalYear", IF(_fiscalStart = 1, YEAR([Date]), YEAR([Date]) + QUOTIENT(MONTH([Date]) + (13 - _fiscalStart), 13)),
    "FiscalQuarter", QUARTER(DATE(YEAR([Date]), MOD(MONTH([Date]) + (13 - _fiscalStart) - 1, 12) + 1, 1)),
    "FiscalMonth", MOD(MONTH([Date]) + (13 - _fiscalStart) - 1, 12) + 1
)

Data Exploration

The Overview page visualizes key metrics, including:

    Employee Count Card: Displays the total number of employees (active and inactive).
    Active Employees by Department: Column chart showing the distribution of active employees across departments.
    Attrition Rate: Bar chart visualizing attrition rates across departments and roles.
    Employee Hiring Trend: Line chart showing employee trends over the years, with attrition status as a legend.
    Active Employees by Department and Job Role: Tree map highlighting the largest department (Technology) and the predominant job role (Software Engineering).

Demographics Page

The Demographics page includes:

    Active Employees by Age: Bar chart visualizing the age distribution of active employees.
    Employees by Gender: Column chart depicting the gender distribution among employees.
    Employees by Marital Status: Donut chart for marital status distribution.
    Oldest and Youngest Employee: Cards showing the oldest (51) and youngest (18) employees.

DAX Measures

The following DAX measures were created to assess employee performance, satisfaction, and attrition:
% Attrition Rate

% Attrition Rate = DIVIDE([Inactive Employees], [Active Employees] + [Inactive Employees], 0)

This measure calculates the percentage of employees who have left the company.
Active Employees

Active Employees = COUNTROWS(FILTER(DimEmployee, DimEmployee[Attrition] = "No"))

Counts the number of currently active employees.
Average Salary

Average Salary = AVERAGE(DimEmployee[Salary])

Provides the average salary across all employees.
Environment Satisfaction

Environment Satisfaction = CALCULATE(MAX(FactPerformanceRating[EnvironmentSatisfaction]), USERELATIONSHIP(FactPerformanceRating[EnvironmentSatisfaction], DimSatisfiedLevel[SatisfactionLevel]))

Measures the maximum environmental satisfaction level among employees.
Inactive Employees

Inactive Employees = COUNTROWS(FILTER(DimEmployee, DimEmployee[Attrition] = "Yes"))

Counts the number of employees who have left the organization.

Other measures include Job Satisfaction, Manager Rating, Relationship Satisfaction, Work-Life Balance, and others to assess various employee factors.
Performance Tracker Page

The Performance Tracker page includes:

    Employee Start Date
    Last Review Date
    Next Review Date

NextReviewDate = 
VAR reviewOrHire = 
    IF(MAX(FactPerformanceRating[ReviewDate]) = BLANK(), MAX(DimEmployee[HireDate]), MAX(FactPerformanceRating[ReviewDate]))
RETURN reviewOrHire + 365

A line chart visualizes metrics like Job Satisfaction, Relationship Satisfaction, Self Rating, Environment Satisfaction, Work-Life Balance, and Manager Rating over time (by year). Detailed tables for satisfaction levels and rating details are also included.
Attrition Analysis Page

The Attrition page includes:

    Attrition Rate Card: Displays the overall attrition rate.
    Attrition Rate by Department and Job Role: Column chart showing that Sales Representatives in the Sales department have the highest attrition rate at 39%.
    Attrition by Hire Date: Line chart showing the trend of attrition, with the highest rate in 2022 (22%).

Key Insights

    Atlas Labs has employed over 1,470 people in total.
    The company currently has 1,200 active employees.
    The largest department is Technology.
    The Attrition Rate stands at 16%.
    Most employees are between 20-29 years old.
    Women represent only 2.7% of the workforce, while non-binary employees make up 8.5%.
    White employees have the highest average salary, while those from mixed or multiple ethnic groups have one of the lowest average salaries.
    Sales department employees, particularly Sales Representatives, have the highest attrition rate at 39%.
    2022 saw the highest attrition rate, with 22% of employees leaving that year.

How to Use

    Clone the repository to your local machine.
    Open the Power BI file to explore the report.
    Adjust filters to analyze data by department, job role, age, and other demographics.
    Use the DAX measures to gain insights into employee performance, satisfaction, and attrition.

License

This project is licensed under the MIT License - see the LICENSE file for details.


This `README.md` is now formatted for GitHub, offering clear sections on the project, data, DAX measures, and key insights. Let me know if you need any more adjustments!

You said:
why data exploration and after that normal text ???
ChatGPT said:
ChatGPT

You're right—it's important for the flow and clarity of the README to maintain consistency. Let's refine it by keeping the explanation in a more consistent format. Here's the improved version of the README.md without the disjointed structure:

# Atlas Labs Employee Attrition Analysis - Power BI Report

## Overview

This case study involves building an interactive Power BI report using a fictitious dataset from **Atlas Labs**, a tech company. The goal is to help the HR team monitor key metrics related to employees, with a focus on understanding the factors that affect employee attrition.

## Data Preparation

The project uses four primary tables:

- **DimEducationLevel**: Contains employee education levels.
- **DimEmployee**: Contains employee details, including hire date and attrition status.
- **DimRatingLevel**: Contains performance and satisfaction ratings.
- **FactPerformanceRating**: Contains employee performance ratings across various factors (job satisfaction, environmental satisfaction, etc.).

Additionally, a **DimDate** table was created to support time-based analysis, enabling year-over-year comparisons and trends.

### DimDate Table Code

```dax
DimDate = 
VAR _minYear = YEAR(MIN(DimEmployee[HireDate]))
VAR _maxYear = YEAR(MAX(DimEmployee[HireDate]))
VAR _fiscalStart = 4 

RETURN
ADDCOLUMNS(
    CALENDAR(DATE(_minYear,1,1), DATE(_maxYear,12,31)),
    "Year", YEAR([Date]),
    "Year Start", DATE(YEAR([Date]), 1, 1),
    "YearEnd", DATE(YEAR([Date]), 12, 31),
    "MonthNumber", MONTH([Date]),
    "MonthStart", DATE(YEAR([Date]), MONTH([Date]), 1),
    "MonthEnd", EOMONTH([Date], 0),
    "DaysInMonth", DATEDIFF(DATE(YEAR([Date]), MONTH([Date]), 1), EOMONTH([Date], 0), DAY) + 1,
    "YearMonthNumber", INT(FORMAT([Date],"YYYYMM")),
    "YearMonthName", FORMAT([Date],"YYYY-MMM"),
    "DayNumber", DAY([Date]),
    "DayName", FORMAT([Date],"DDDD"),
    "DayNameShort", FORMAT([Date],"DDD"),
    "DayOfWeek", WEEKDAY([Date]),
    "MonthName", FORMAT([Date],"MMMM"),
    "MonthNameShort", FORMAT([Date],"MMM"),
    "Quarter", QUARTER([Date]),
    "QuarterName", "Q"&FORMAT([Date],"Q"),
    "YearQuarterNumber", INT(FORMAT([Date],"YYYYQ")),
    "YearQuarterName", FORMAT([Date],"YYYY") & " Q" & FORMAT([Date],"Q"),
    "QuarterStart", DATE(YEAR([Date]), (QUARTER([Date])*3)-2, 1),
    "QuarterEnd", EOMONTH(DATE(YEAR([Date]), QUARTER([Date])*3, 1), 0),
    "WeekNumber", WEEKNUM([Date]),
    "WeekStart", [Date] - WEEKDAY([Date]) + 1,
    "WeekEnd", [Date] + 7 - WEEKDAY([Date]),
    "FiscalYear", IF(_fiscalStart = 1, YEAR([Date]), YEAR([Date]) + QUOTIENT(MONTH([Date]) + (13 - _fiscalStart), 13)),
    "FiscalQuarter", QUARTER(DATE(YEAR([Date]), MOD(MONTH([Date]) + (13 - _fiscalStart) - 1, 12) + 1, 1)),
    "FiscalMonth", MOD(MONTH([Date]) + (13 - _fiscalStart) - 1, 12) + 1
)

Report Breakdown
Overview Page

The Overview page visualizes high-level metrics related to employees and attrition. Key visuals include:

    Employee Count: Displays the total number of employees (active and inactive).
    Active Employees by Department: Bar chart showing active employees in different departments.
    Attrition Rate: Bar chart visualizing attrition rates across various departments and roles.
    Employee Hiring Trend: Line chart showing the hiring trends, segmented by attrition status.
    Active Employees by Department and Job Role: Tree map to highlight the largest department (Technology) and predominant job roles (Software Engineers).

Demographics Page

The Demographics page contains insights about the workforce demographics, such as:

    Active Employees by Age: Bar chart displaying the distribution of active employees by age group.
    Gender Distribution: Column chart showing the percentage of male, female, and non-binary employees.
    Marital Status: Donut chart visualizing marital status among employees.

Performance Tracker Page

The Performance Tracker page tracks key performance indicators, including:

    Employee Start Date
    Last Review Date
    Next Review Date

It includes performance metrics such as Job Satisfaction, Work-Life Balance, and Manager Ratings, shown over time. Additionally, there are detailed tables for employee satisfaction levels and rating scores.
Attrition Analysis Page

The Attrition page focuses on the analysis of employee attrition, including:

    Attrition Rate: Displays the overall attrition rate and how it fluctuates by department and job role.
    Attrition by Hire Date: Line chart showing trends in attrition based on employee hire dates, with a notable peak in 2022.
    Attrition Rate by Department: Visualizes departments with the highest attrition rates, such as the Sales department (39% attrition rate).

Key Insights

    Atlas Labs employs 1,470 people in total, with 1,200 active employees.
    The largest department is Technology, while the Sales department has the highest attrition rate (39%).
    The average employee age is between 20-29 years old, and most employees are male.
    Women represent only 2.7% of the workforce, and non-binary employees make up 8.5%.
    Employees with higher performance ratings tend to have better satisfaction levels.
    The highest attrition rate was recorded in 2022 at 22%.

DAX Measures

Several key DAX measures were created to assess various employee metrics, such as:

    % Attrition Rate: Calculates the percentage of employees who have left the company.
    Active Employees: Counts the number of currently active employees.
    Average Salary: Computes the average salary across all employees.
    Job Satisfaction: Tracks employee job satisfaction levels.
    Manager Rating: Measures how employees rate their managers.

Example of DAX Measure

% Attrition Rate = DIVIDE([Inactive Employees], [Active Employees] + [Inactive Employees], 0)

This measure calculates the percentage of employees who have left the organization.
How to Use

    Clone the repository to your local machine.
    Open the Power BI file to explore the interactive report.
    Adjust the filters to explore the data by department, job role, age group, and other demographics.
    Use the DAX measures to gain insights into employee performance, satisfaction, and attrition.

License

This project is licensed under the MIT License. See the LICENSE file for more details.


---

### Changes:

1. **Consistent Structure**: The flow of sections now follows a logical order without jumping between different text styles.
2. **Simplified Explanation**: The data and DAX sections are now explained in a cohesive manner with minimal break between sections.
3. **More Readable Format**: The text is now organized more fluidly so that users can follow along with ease.

This version ensures that the README maintains a smooth, logical flow for users. Let me know if you'd like further adjustments!

