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




**Question 2:** For this question you’ll need to use SQL. [Follow this link](https://www.w3schools.com/SQL/TRYSQL.ASP?FILENAME=TRYSQL_SELECT_ALL) to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

1. How many orders were shipped by Speedy Express in total?
   
    - Thought process:
      
        1. Need to find what table(s) have the shipper names listed. 
           
            - Found the shipper names and id numbers in the `Shippers` table.
        2. Need to next find the table(s) where I would get all the orders that the shipper shipped.
           
            - Found that all orders and shipper ids are in the `Orders` table.
          
        3. Need to join the `Shippers` and `Orders` table together on the `ShipperID` column.
           
        4. Get the `COUNT` of how many orders were shipped by "Speedy Express" only.
    ```
    SELECT COUNT(O.ShipperID) as Speedy_Count
    FROM Orders AS O
        JOIN Shippers AS S ON O.ShipperID = S.ShipperID
    WHERE S.ShipperName = "Speedy Express";
   
    Output --> 54
    ```

2. What is the last name of the employee with the most orders?
    - Thought process:
      
        1. Need to find the table(s) with employee last names.
            - Found the `Employees` table has the `LastName` of the employees and their `EmpployeeID`.
            
        2. Need to find the table(s) with the employee ids, and the orders that employee processed.
            - Found that the `Orders` table has the information that I need.
          
        3. Need to calculate the total orders for each employee id.
           
        4. Then need to sort those totals by value in descending order.
           
        5. Lastly,  show only the employee's last name that is at the top of the list.
    
    ```
     SELECT E.LastName,COUNT(E.EmployeeID) as Most_OrdersBy
     FROM Employees AS E
          JOIN Orders AS O ON O.EmployeeID = E.EmployeeID
     GROUP BY E.EmployeeID
     ORDER BY COUNT(E.EmployeeID) DESC
     LIMIT 1;
    
    Output --> Peacock
    ```

3. What product was ordered the most by customers in Germany?
   
    - Thought process:
      
        1. Find the table(s) where the customer's country is listed.
           
            - Found the country where the customer is in the `Customers` table. 
            
        2. Look at the `Products` table to see how I have to link it back to the `Customers` table, since it was the only one with the country where the customer resides.
           
            - There is a `ProductID` in the `Products` table that links to the `OrderDetails` table, which also has the `Quanitity` of the product ordered.
              
            - Then the `Orders` table is what links the `Customers` table to the `OrderDetails` table with `CustomerID` and `OrderID`.
          
        3. Going to need to do 3 inner joins in order to get all the information linked that I need.
           
        4. Have to have a conditional statement to only look at customers in Germany.
           
        5. Need to get a sum for each product id.
           
        6. Sort all the sums in descending order.
           
        7. Finally, return the first value in the product name column.
    
    ```
    SELECT prod.ProductName
    FROM Products AS prod
        JOIN OrderDetails AS details ON details.ProductID = prod.ProductID
        JOIN Orders AS ord ON ord.OrderID = details.OrderID
        JOIN Customers AS cust ON cust.CustomerID = ord.CustomerID
    WHERE cust.Country = "Germany"
    GROUP BY prod.ProductName
    ORDER BY SUM(details.Quantity) DESC
    LIMIT 1;
   
    Output --> Boston Crab Meat
    ```
