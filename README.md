# Data-Analytics-Portfolio-
This is my data analytics portfolio of projects I have worked on in the past. Tools used include Excel, SQL, Tableau, Python &amp; R.

## Project 1: Portfolio Analysis & Optimization Using Excel
*Skills & Tools Used: Web Scraping, Linear Optimization, Scenario Analysis, Statistical Analysis, Excel Data Analysis ToolPak*

For this project, my goal was to analyze various features of portfolio returns using historical stock data. We were tasked with:

(1) Gathering summary data for 10 stocks, including return rates, volatility measurements, and sharpe ratio calculations;

(2) Optimizing the returns of mixed asset portfolios, one with 2 stocks (MSFT and WFC) and one with all 10 stocks;

(3) Utilizing the capital asset pricing model (CAPM) to compare and rank the volatility of each of the 10 stocks;

(4) Optimizing the returns of a mixed asset portfolio featuring two Vanguard funds, VBTLX & VFIAX, then comparing the return rate and volatility of the portfolio relative to the return rate and volatility of the single security AAPL;

(5) Creating a PowerPoint presentation to communicate findings to clients and elaborate on the importance of portfolio diversification.

Below is the Excel file:

[Business and Financial Modeling Capstone Portfolio .xlsx](https://github.com/XSUPREMEXSKILLZ/Data-Analytics-Portfolio-/files/10060798/Business.and.Financial.Modeling.Capstone.Portfolio.xlsx)

## Project 2: Exploratory Data Analysis and Visualization of Employee Data Using SQL and Tableau
*Skills Used: Business Analytics, Data Modeling, ETL, Case When Statements, SQL Subqueries, Stored Procedures, Aggregate Functions, SQL Joins & Cross Joins, Software Integration With Tableau, Bar Charts, Line Charts, Area Charts, Side-By-Side Bar Charts*

The following project uses a fabricated database featuring multiple tables of employee data. The goal was to complete a series of business tasks involving transforming data in the employee database, then exporting the results of the transformation to Tableau for visualization. Below are the descriptions of the tasks as well as the SQL code and Tableau visualizations. Task #5 was to combine the 4 visualizations into a dashboard. 

### Task 1: Create a visualization that provides a breakdown between the male and female employees working in the company each year, starting from 1990. 
**Code:**

SELECT 
    YEAR(d.from_date) AS calendar_year,
    e.gender,
    COUNT(e.emp_no) AS num_of_employees

FROM
    t_employees e
        
JOIN
    t_dept_emp d ON d.emp_no = e.emp_no

GROUP BY calendar_year , e.gender

HAVING calendar_year >= 1990;

![Visualization 1](https://user-images.githubusercontent.com/29240952/203175746-4f33898b-3521-47ae-9d57-cd8c4521469b.PNG)

### Task 2: Compare the number of male managers to the number of female managers from different departments for each year, starting from 1990.
**Code:**

SELECT 
    d.dept_name,
    ee.gender,
    dm.emp_no,
    dm.from_date,
    dm.to_date,
    e.calendar_year,
   
CASE

WHEN YEAR(dm.to_date) >= e.calendar_year AND YEAR(dm.from_date) <= e.calendar_year THEN 1

ELSE 0

END AS active

FROM

(SELECT YEAR(hire_date) AS calendar_year
    
FROM t_employees
   
GROUP BY calendar_year) e
        
CROSS JOIN t_dept_manager dm
        
JOIN t_departments d ON dm.dept_no = d.dept_no
       
JOIN t_employees ee ON dm.emp_no = ee.emp_no

ORDER BY dm.emp_no , calendar_year;

![Visualization 2](https://user-images.githubusercontent.com/29240952/203175911-4856b0c6-fdeb-4e34-a236-0a8e305551a8.PNG)

### Task 3: Compare the average salary of female versus male employees in the entire company until year 2002, and add a filter allowing you to see that per each department.
**Code:**

SELECT e.gender, d.dept_name, ROUND(AVG(s.salary), 2) AS salary, YEAR(s.from_date) AS calendar_year

FROM t_salaries s

JOIN t_employees e ON s.emp_no = e.emp_no

JOIN t_dept_emp de ON de.emp_no = e.emp_no

JOIN t_departments d ON d.dept_no = de.dept_no

GROUP BY d.dept_no , e.gender , calendar_year

HAVING calendar_year <= 2002

ORDER BY d.dept_no;

![Visualization 3](https://user-images.githubusercontent.com/29240952/203175941-eb90b4e2-433c-4bff-8f0b-49e3ea12b664.PNG)

### Task 4: Create an SQL stored procedure that will allow you to obtain the average male and female salary per department within a certain salary range. Let this range be defined by two values the user can insert when calling the procedure.
**Code:**

DROP PROCEDURE IF EXISTS filter_salary;

DELIMITER $$

CREATE PROCEDURE filter_salary (IN p_min_salary FLOAT, IN p_max_salary FLOAT)

BEGIN

SELECT e.gender, d.dept_name, AVG(s.salary) as avg_salary

FROM t_salaries s

JOIN t_employees e ON s.emp_no = e.emp_no

JOIN t_dept_emp de ON de.emp_no = e.emp_no

JOIN t_departments d ON d.dept_no = de.dept_no

WHERE s.salary BETWEEN p_min_salary AND p_max_salary

GROUP BY d.dept_no, e.gender;

END$$

DELIMITER ;

CALL filter_salary(50000, 90000);

![Visualization 4](https://user-images.githubusercontent.com/29240952/203176023-37fbec24-645c-4950-a4b0-7914d370ac91.PNG)

## Task 5: Organize Visualizations 1-4 Into a Beautiful Dashboard:

![Dashboard of Visualizations](https://user-images.githubusercontent.com/29240952/203176137-1c82be5c-c1d8-41ad-8ab5-ef0364757154.PNG)
