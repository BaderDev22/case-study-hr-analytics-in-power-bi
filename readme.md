Project Overview:

The core goal of this case study is to build an interactive report using fictitious datasets from a tech company called Atlas Labs. The HR team at Atlas Labs wants to monitor key metrics related to employees and, as a secondary goal, understand the factors affecting employee attrition.
Data Preparation:

The project involves four primary tables:

    DimEducationLevel: Contains data related to employees' education levels.
    DimEmployee: Contains employee data, including personal details, hire dates, and attrition status.
    DimRatingLevel: Contains rating levels for performance and satisfaction.
    FactPerformanceRating: Contains performance ratings such as job satisfaction, environmental satisfaction, and manager ratings.

To ensure proper analysis, we renamed these tables with the Dim prefix for dimension tables, and the FactPerformanceRating table was designated as the fact table. Additionally, we created a DimDate table to support time-based analysis. The following DAX code was used to create the DimDate table:

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

Data Exploration:

We created the Overview page to showcase key metrics using various visuals, such as:

    Employee Count Card: Displays the total number of employees (active and inactive).
    Active Employees by Department: Column chart showing the distribution of active employees across departments.
    Attrition Rate: A bar chart visualizing attrition rates across departments and roles.
    Employee Hiring Trend: Line chart showing employee trends over the years, with attrition status as a legend.
    Active Employees by Department and Job Role: Tree map highlighting the largest department (Technology) and the predominant job role (Software Engineering).

We also created a Demographics page with the following visuals:

    Active Employees by Age: Bar chart visualizing the age distribution of active employees.
    Employees by Gender: Column chart depicting the gender distribution among employees.
    Employees by Marital Status: Donut chart for marital status distribution.
    Oldest and Youngest Employee: Cards showing the oldest (51) and youngest (18) employees.

DAX Measures and Their Explanations:

    % Attrition Rate:

% Attrition Rate = DIVIDE([Inactive Employees], [Active Employees] + [Inactive Employees], 0)

Measures the percentage of employees who have left the company.

Active Employees:

Active Employees = COUNTROWS(FILTER(DimEmployee, DimEmployee[Attrition] = "No"))

Counts the number of currently active employees.

Average Salary:

Average Salary = AVERAGE(DimEmployee[Salary])

Provides the average salary across all employees.

Environment Satisfaction:

Environment Satisfaction = CALCULATE(MAX(FactPerformanceRating[EnvironmentSatisfaction]), USERELATIONSHIP(FactPerformanceRating[EnvironmentSatisfaction], DimSatisfiedLevel[SatisfactionLevel]))

Measures the maximum environmental satisfaction level among employees.

Inactive Employees:

    Inactive Employees = COUNTROWS(FILTER(DimEmployee, DimEmployee[Attrition] = "Yes"))

    Counts the number of employees who have left the organization.

Additional measures include Job Satisfaction, Manager Rating, Relationship Satisfaction, Work-Life Balance, and others to assess various employee factors.
Performance Tracker Page:

We also created a Performance Tracker page with the following metrics:

    Employee Start Date
    Last Review Date
    Next Review Date

    NextReviewDate = 
    VAR reviewOrHire = 
      IF(MAX(FactPerformanceRating[ReviewDate]) = BLANK(), MAX(DimEmployee[HireDate]), MAX(FactPerformanceRating[ReviewDate]))
    RETURN reviewOrHire + 365

A line chart visualizes metrics like Job Satisfaction, Relationship Satisfaction, Self Rating, Environment Satisfaction, Work-Life Balance, and Manager Rating over time (by year). We also added tables for satisfaction levels and rating details for deeper insights.
Attrition Analysis Page:

Finally, we created an Attrition page that includes:

    Attrition Rate Card: Displays overall attrition rate.
    Attrition Rate by Department and Job Role: Column chart showing that Sales Representatives in the Sales department have the highest attrition rate at 39%.
    Attrition by Hire Date: Line chart showing the trend of attrition, with the highest rate in 2022 (22%).

Key Insights:

    Atlas Labs has employed over 1,470 people in total.
    The company currently has 1,200 active employees.
    The largest department is Technology.
    The Attrition Rate stands at 16%.
    Most employees are between 20-29 years old.
    Women represent only 2.7% of the workforce, while non-binary employees make up 8.5%.
    White employees have the highest average salary, while those from mixed or multiple ethnic groups have one of the lowest average salaries.
    Sales department employees, particularly Sales Representatives, have the highest attrition rate at 39%.
    2022 saw the highest attrition rate, with 22% of employees leaving that year.