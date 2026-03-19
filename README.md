# telco-customer-churn-sql-analysis
# 📊 Telco Customer Churn Analysis | SQL Portfolio Project

![SQL](https://img.shields.io/badge/SQL-PostgreSQL-blue)
![Portfolio](https://img.shields.io/badge/Portfolio-Data%20Analyst-brightgreen)

## 📌 Project Overview
Analyzed customer churn for a telecommunications company using PostgreSQL.
The analysis identifies key factors driving customer churn and provides actionable insights to improve customer retention.

## Business Problem
Customer churn is a significant concern for telecom companies.
Getting a new customer costs more than keeping an old one.

This project examines customer data to find patterns and variables contributing to churn.

## Dataset
This project uses the **Telco Customer Churn Dataset**, which contains customer information for a telecommunications company.

The dataset is used to analyze customer behavior and identify factors that contribute to customer churn.

Dataset Overview:

• Total Records: 7,043 customers  
• Total Features: 21 variables  
• Target Variable: Churn (Yes / No)

Churn represents whether a customer stopped using the company’s services.
Churn Distribution:
- No: 5,174 customers (~73%)
- Yes: 1,869 customers (~27%)

## 🛠️ Skills Demonstrated
- **SQL:** CTEs, Aggregations, CASE statements, JOINs, Subqueries
- **Data Cleaning:** Handling NULL values, data type conversions
- **Exploratory Data Analysis:** Pattern identification, trend analysis
- **PostgreSQL:** Table creation, CSV import, query optimization

## 🔍 Key Questions Answered
1. What is the overall customer churn rate?
2. How does contract type affect churn?
3. Do higher monthly charges lead to more churn?
4. Which internet service type has the highest churn?
5. Do senior citizens churn more?

## 📈 Key Findings

| Finding | Result | Business Impact |
|--------|--------|-----------------|
| **Overall Churn Rate** | 26.5% | 1 in 4 customers leave - significant revenue loss |
| **Month-to-Month Contracts** | 42.7% churn | Customers need incentives to commit longer |
| **Fiber Optic Customers** | 41.9% churn | Higher expectations = need better reliability |
| **Price Impact** | Churned customers pay $13 more/month | Price sensitivity is real |
| **Senior Citizens** | 41.7% churn vs 23.6% for others | Target retention for seniors |

## 💡 Business Recommendations
1. Offer discounts for annual contracts to move customers from month-to-month
2. Invest in fiber optic reliability - highest churn segment needs attention
3. Create senior citizen retention program - they churn at nearly double the rate
4. Review pricing strategy - price-sensitive customers need value packages

## 📊 Tableau Dashboard

Customer Churn Analysis Dashboard
![image alt](https://github.com/FarisZalfa/sql-telco-customer-churn-analysis/blob/063afc8f5e18c86fd0d2e28884081e58219b9758/Telco%20Customer%20Churn%20Analysis.png)

Tools Used
- Tableau
- Data Visualization
- Exploratory Data Analysis

Key Insights
- Overall churn rate is 26.5%
- Month-to-month contracts have highest churn
- Higher monthly charges correlate with churn
- Fiber optic customers churn more frequently

## 🚀 How to Run This Project
- Download dataset from Kaggle Telco Customer Churn
- Create PostgreSQL table using provided schema
- Import CSV data
- Run analysis queries

## 📁 Data Source
Dataset: [Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)

## 🖥️ SQL Queries
Below are the SQL queries used to perform the analysis.

```sql
-- Overall Churn Rate
 SELECT 
       ROUND(100.0 * SUM(CASE WHEN churn = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 1) AS churn_rate
   FROM customer_churn;

-- Contract Type Analysis
SELECT 
    contract,
    COUNT(*) AS total_customers,
    SUM(CASE WHEN churn = 'Yes' THEN 1 ELSE 0 END) AS churned,
    ROUND(100.0 * SUM(CASE WHEN churn = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) AS churn_rate
FROM customer_churn
GROUP BY contract
ORDER BY churn_rate DESC;

-- Compare average spending between churned and stayed customers
SELECT 
    churn,
    COUNT(*) AS customer_count,
    ROUND(AVG(monthlycharges), 2) AS avg_monthly_charges,
    ROUND(AVG(tenure), 1) AS avg_tenure_months
FROM customer_churn
GROUP BY churn;

-- Internet Service Impact
SELECT 
    internetservice,
    COUNT(*) AS customers,
    ROUND(AVG(monthlycharges), 2) AS avg_charges,
    ROUND(100.0 * SUM(CASE WHEN churn = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) AS churn_rate
FROM customer_churn
WHERE internetservice != 'No'  -- Exclude those without internet
GROUP BY internetservice
ORDER BY churn_rate DESC;

-- Senior Citizen Analysis
SELECT 
    CASE WHEN seniorcitizen = 1 THEN 'Senior' ELSE 'Non-Senior' END AS customer_type,
    COUNT(*) AS total,
    ROUND(100.0 * SUM(CASE WHEN churn = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) AS churn_rate
FROM customer_churn
GROUP BY seniorcitizen;
