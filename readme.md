HR Dashboard - Power BI Case Study 📊
Overview 🏢

In this Power BI case study, you will be exploring a dataset for a fictitious software company called Atlas Labs. The primary goal of this case study is to analyze and visualize the organization's HR data, with a secondary focus on understanding factors affecting employee attrition. This analysis will help the company improve retention strategies and gain deeper insights into their workforce.
Project Objectives 🎯

The core goal of this project is to:

    Build a report to help Atlas Labs' HR team monitor key employee metrics.
    Analyze employee attrition and identify the factors affecting it.
    Create an interactive and insightful Power BI dashboard to communicate the findings.

Data Preparation 🧑‍💻

The dataset consists of the following tables:

    DimEducationLevel: Education levels of employees.
    DimEmployee: Employee details including hire date, age, gender, department, and job role.
    DimPerformanceRating: Employee performance ratings.
    DimSatisfactionLevel: Employee satisfaction levels (e.g., job, environment, work-life balance).
    FactPerformanceRating: Performance-related data.

Steps Involved:

    Data Cleaning and Transformation using Power Query.

    Date Table Creation:
        A custom DimDate table was created to facilitate time-based analysis, providing fields like year, month, quarter, fiscal year, and week.
        The following DAX formula was used to create the DimDate table:

    DimDate = 
    VAR _minYear = YEAR(MIN(DimEmployee[HireDate]))
    VAR _maxYear = YEAR(MAX(DimEmployee[HireDate]))
    VAR _fiscalStart = 4 

    RETURN
    ADDCOLUMNS(
        CALENDAR(DATE(_minYear,1,1), DATE(_maxYear,12,31)),
        "Year", YEAR([Date]),
        "Year Start", DATE(YEAR([Date]),1,1),
        "YearEnd", DATE(YEAR([Date]),12,31),
        "MonthNumber", MONTH([Date]),
        "MonthStart", DATE(YEAR([Date]), MONTH([Date]), 1),
        "MonthEnd", EOMONTH([Date], 0),
        ...
    )

    Table Relationships were set up to link the relevant dimensions to the fact tables.

Visualizations and Key Metrics 📊
Overview Page 🌐

    Employee Count: Total employees, Active vs Inactive employees.
    Attrition Rate: Calculated as the percentage of employees who have left the company.
    Performance Metrics: Job satisfaction, manager rating, work-life balance, and environment satisfaction.

Attrition Insights 📉

    Attrition Rate: The rate of employees leaving the organization is 16%.
    Employee Trends: Visualizations on the number of employees hired by year and attrition trends.

Demographics Insights 📅

    Age Distribution: Most employees are between the ages of 20-29 years.
    Gender and Diversity: Only 2.7% of active employees are women, and 8.5% identify as non-binary.
    Ethnic Group Insights: White employees have the highest average salary, while those identifying as mixed or multiple ethnic groups have one of the lowest average salaries.

DAX Measures and Calculations ➗

    % Attrition Rate: Calculates the attrition percentage.

% Attrition Rate = DIVIDE([Inactive Employees], [Active Employees] + [Inactive Employees], 0)

Active Employees: Counts the number of currently active employees.

Active Employees = COUNTROWS(FILTER(DimEmployee, DimEmployee[Attrition] = "No"))

Job Satisfaction: Measures the highest job satisfaction level among employees.

    Job Satisfaction = MAX(FactPerformanceRating[JobSatisfaction])

    Age Insights: Measures the youngest and oldest employee ages.

Key Insights 🧠

    Atlas Labs has employed over 1470 people in total.
    Currently, 1200 employees are active within the company.
    The largest department by far is Technology.
    The attrition rate for employees leaving the organization is 16%, which highlights the need to investigate factors impacting retention.
    The majority of employees hired are between the ages of 20-29 years old, indicating a younger workforce.
    Among active employees, women make up only 2.7% of the workforce.
    Non-binary employees make up 8.5% of the workforce.
    Employees who identify as white have the highest average salary.
    Employees identifying as mixed or from multiple ethnic groups have one of the lowest average salaries.
    Sales department employees, particularly Sales Representatives, have the highest attrition rate at 39%.
    2022 saw the most attrition with 22% of employees leaving that year.

Final Thoughts and Next Steps 🔍

This analysis provides a comprehensive view of the workforce dynamics at Atlas Labs, from attrition rates to salary trends. The insights gained through this dashboard can help HR teams take proactive steps in improving retention, enhancing diversity and inclusion efforts, and ensuring fair compensation practices.

Future improvements could include:

    Employee Retention Strategies: Further analysis to determine the root causes of high attrition.
    Diversity Initiatives: Implement programs to increase female representation and promote equity across all ethnic groups.
    Salary Adjustments: Investigate salary disparities between different ethnic groups and job roles.

This Power BI dashboard serves as a dynamic tool for HR leaders to continuously monitor and optimize their workforce strategies.

Feel free to reach out with any questions or further requests for details! 📧

This updated README should provide a comprehensive understanding of your Power BI HR dashboard project, its objectives, and key insights.