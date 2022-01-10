# Shopify2021DataScienceChallenge

## Winter 2021 Data Science Intern Challenge 

Please complete the following questions, and provide your thought process/work. You can attach your work in a text file, link, etc. on the application page. Please ensure answers are easily visible for reviewers!


### Question 1: Given some sample data, write a program to answer the following: click here to access the required data set
- Please see the code attached which includes visualizations and methods used to solve the problem!

On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

- Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 
  - When calculating the Average Order Value, an outlier of 704000 was also included in the calculation. This caused the AOV to be skewed much higher than it should be. We can see that this comes from one user making a very large purchase several times in March. A better way to evaluate this data would be to use a method that is more robust to outliers, such as the median of the dataset or the median of the Interquartile Range of the dataset (middle 50%). 

- What metric would you report for this dataset?
  - I would use the median of the middle 50% of the dataset. This would allow me to take into consideration only the values that make up the middle 50% of the dataset (thereby dropping from consideration values that are very low or very high). 

- What is its value?
  - 276.5



### Question 2: For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

- How many orders were shipped by Speedy Express in total?
  - SELECT COUNT(Orders.OrderID) as 'Speedy Count'
  FROM Orders
  INNER JOIN Shippers ON Orders.ShipperID=Shippers.ShipperID
  WHERE Shippers.ShipperName = 'Speedy Express';
  - 54

- What is the last name of the employee with the most orders?
  - SELECT TOP 1 Employees.LastName
  FROM Orders
  INNER JOIN Employees ON Orders.EmployeeID=Employees.EmployeeID
  GROUP BY Employees.LastName
  ORDER BY Count(Orders.OrderID) DESC;
  - Peacock

- What product was ordered the most by customers in Germany?
  - I interpreted this question as “the product that has had the most orders placed in Germany”. 
  - SELECT Products.ProductName
  FROM Orders
  INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID
  INNER JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
  INNER JOIN Products ON OrderDetails.ProductID = Products.ProductID
  WHERE Customers.Country = 'Germany'
  GROUP BY Products.ProductName
  ORDER BY COUNT(Products.ProductName) DESC
  LIMIT 1;
  - Gorgonzola Telino
