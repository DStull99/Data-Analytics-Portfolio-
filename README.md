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

The following project uses a fabricated database featuring multiple tables of employee data. The goal was to complete a series of business tasks involving transforming data in the employee database, then exporting the results of the transformation to Tableau for visualization. Below are the descriptions of the tasks as well as the SQL code and Tableau visualizations. The final task was to combine the 4 visualizations into a dashboard. 

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
