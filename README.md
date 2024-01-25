# Bank Churn Report

## Table Of Contents
- [Overview](##Overview)
- [Objectives](##Objectives)
- [Tools Used](##Tools-Used)
- [Data exploration/preparation](##Data-exploration/preparation)
- [Explanatory data analysis](###Explanatory-data-analysis)
- [SQL Analysis](##Data-Analysis)
- [Result](##Result)
- [Recommendation](###Recommendation)
  
## Overview
This report offers a comprehensive report of customer data for a banking institution. It encompasses various attributes related to customers, providing insights into their demographics, financial behavior, exited customers and interactions with the bank. This report will look into the key reasons and customers that left the bank.

## Objectives
- Create a comprehensive report using Power BI to visualize and explore customer-related metrics.
- Connect to a SQL database to fetch and update the dataset.
- Provide insights into customer demographics, behavior, and churn.

 
## Tools Used
- Power Bi
- SQL

## Data Exploration and preparation
- Check for missing values, outliers, and duplicate records.
- Explore the distribution of each feature in the dataset
- Dataset cleaning commenced
- Grouping of values such as age and creditscore
  
  ## Explanatory data analysis
  
 ### Insights

- Descriptive Statistics: Utilize Power BI to compute and visualize descriptive statistics for key numerical features and also utilized SQL to explore 
 measures such as Average, Sum and percentages

- Demographic Analysis: Explore the distribution of customers based on geography, gender, and age to understand the bank's customer base.

- Creditworthiness and Financial Health: Analyze the credit scores and account balances to assess the financial health of customers.

- Customer Churn Prediction: Explored the 'Exited' customers to predict and understand factors contributing to customer churn.

- Customer Engagement: Examine the active membership status and its impact on customer engagement.

  ## SQL Analysis


```--BANK CHURN ANALYSIS[EXITED CUSTOMERS BREAKDOWN]


--View Table
SELECT
	*
FROM
	Churn

-- Checked for duplicates
SELECT 
	CustomerId, 
		COUNT(*)
	AS DuplicateCount
FROM 
	Churn
GROUP BY 
	CustomerId
HAVING 
	COUNT(*) > 1

--Churn Rate
SELECT 
	SUM(Exited)
	/
	COUNT(CustomerId)*
	100
	AS Churn_rate
FROM 
	Churn
		



--Average creditscore
SELECT
	AVG(
		Creditscore)
	AS Average_score
FROM
	Churn
WHERE 
	Exited = 1


--Average Balance
SELECT
	ROUND(
	AVG(
		Balance),2)
	AS Average_Balance
FROM
	Churn
WHERE 
	Exited = 1


--Percentage of Active members
SELECT
	ROUND(
	SUM(
	isActiveMember)
	/
	COUNT(
	CustomerId)*
	100,
	2)
	AS Percent_of_Active_members
FROM
	Churn
WHERE 
	Exited = 1


--Percentage of inactive members
SELECT
	ROUND(
	(COUNT(*) - 
	SUM(IsActiveMember))*100
	/
	COUNT(*),
	2)
FROM
	Churn
WHERE 
	Exited = 1


--Age group 
SELECT 
	CASE
		WHEN Age <= 30 THEN '18-30'
		WHEN Age BETWEEN 31 AND 45 THEN '31-44'
		ELSE '45-100'
	END
		AS Age_grouping,
	COUNT(*) 
		AS Total
FROM
	Churn
WHERE 
	Exited = 1
GROUP BY 
	CASE
		WHEN Age <= 30 THEN '18-30'
		WHEN Age BETWEEN 31 AND 45 THEN '31-44'
		ELSE '45-100'
	END



--Average Tenure
SELECT
	AVG(Tenure)
	AS
	Avg_Tenure
FROM
	Churn
WHERE 
	Exited = 1


--Number of customers with credit cards
SELECT
	Count(HasCrCard)
	AS #_of_customer
FROM
	Churn
WHERE 
	HasCrCard = 1 AND Exited = 1


--Number of customers without credit card
SELECT
	(COUNT(*) - SUM(HasCrCard))
	AS Total
FROM
	Churn
WHERE 
	Exited = 1


--Customers by countries
SELECT
	Geography,
	  COUNT(
	DISTINCT
	(CustomerId)) 
	AS #_Of_customers
FROM
	Churn
WHERE 
	Exited = 1
GROUP BY
	Geography


--Customers by gender
SELECT
	COUNT(
	DISTINCT(CustomerId)) AS #_Of_customers,
	Gender
FROM
	Churn
WHERE 
	Exited = 1
GROUP BY
	Gender



--Creditscore grouping
SELECT 
	CASE
		WHEN Creditscore <=579 THEN 'Poor'
		WHEN Creditscore BETWEEN 580 AND 669 THEN 'Fair'
		WHEN Creditscore BETWEEN 670 AND 739 THEN 'Good'
		WHEN Creditscore BETWEEN 740 AND 799 THEN 'Very Good'
		ELSE 'Excellent'
	END
		AS Credit_categories,
	COUNT(*) 
		AS #_Of_cat
FROM
	Churn
WHERE 
	Exited = 1
GROUP BY 
	CASE
		WHEN Creditscore <=579 THEN 'Poor'
		WHEN Creditscore BETWEEN 580 AND 669 THEN 'Fair'
		WHEN Creditscore BETWEEN 670 AND 739 THEN 'Good'
		WHEN Creditscore BETWEEN 740 AND 799 THEN 'Very Good'
		ELSE 'Excellent'
	END
ORDER BY 
	2 
	DESC


--Products 
SELECT
	CASE
		WHEN NumOfProducts = 1 THEN 'Single product'
		ELSE 'Multiple product'
	END 
	AS Products,
	COUNT(*) AS Total
FROM
	Churn
WHERE
	Exited = 1
GROUP BY
	CASE
		WHEN NumOfProducts = 1 THEN 'Single product'
		ELSE 'Multiple product'
	END
ORDER BY 
	2
	DESC;
```

## Result  
- Churn percentage rate is 20.37%
- Customers who churn the bank are mostly older customers age group of 45-100
- Customers with credit card and credit score of Fair and poor category has the highest churn rate
- Customers that uses single product churned the most
- Customers in germany has the highest churn rate
- Inactive members churned the most

## Recommendation
1. Due to the analysis result that most churn customers are of older age group: This may be due to the financial instability of the customers in the older age group as most will be getting into retirement
   - solution: Introducing flexible retirement and investment schemes for customers  
2. Customers with credit card tend to churn more: This might be because of high interest rate or the customers are getting a competitive offers from other competing banks
   - Solution: Fees and interest rate should be reviewed so as to stay competitive in the market and help retain customers
3. Customers using just one product tend to churn more as they might not be fully informed or aware of other products
   - Solution: Awareness and educating customers on values of other products should be considered
  
  






