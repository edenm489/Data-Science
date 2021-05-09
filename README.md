# Data-Science
### Question 1a) Think about what could be going wrong with our calculation. Think about a better way to evaluate this data 


Ans: When initially analysing the data, it can be seen that there are many outliers with order amounts much higher than 1000. The best way to evaluate the data is to first determine the high order amounts and analyse it. A pattern can be found with respect to one of the orders with an amount of '704000'. With a total of 17 orders and each order with a purchase of 2000 'total_items'. All of these orders are purchased in the same month around the same time. Some of the best ways to evaluate this data is to understand whether these are business transactions and to form a seperate data evaluation. If there are business and regular transactions, then we can seperate them into two datasets. 

But since that is not given another way is to understand the mean is susceptible large amounts of outliers and the best way to get a good estimate of the actual average is to look at the median value.  

Another way is to look at the percentage of the data that falls in the region we want to look into and removing the outliers.

### Question 1b): What metric would you report for this dataset?

Ans: The metric I would use to find the actual Average Order Value is by focusing on removing the outliers, to account for the skewed distribution, by changing them to null values and removing them.

    for x in ['order_amount']:
      upper75,lower25 = np.percentile(data.loc[:,x],[75,25])
      intr_qr = upper75-lower25
 
      max = upper75+1.5*intr_qr
      min = lower25-1.5*intr_qr
 
      data.loc[data[x] < min,x] = np.nan
      data.loc[data[x] > max,x] = np.nan

    data = data.dropna(axis = 0)
    data.boxplot("order_amount")

### Question 1c): What is its value?

The new AOV is 293.715374

Link to code: https://github.com/edenm489/Data-Science/blob/main/Question%201%20Answer.ipynb

## SQL

### Question 2a: How many orders were shipped by Speedy Express in total?

    SELECT ShipperName, COUNT(*) AS ShippedOrders

    FROM (Orders INNER JOIN Shippers ON Shippers.ShipperID = Orders.ShipperID)

    WHERE ShipperName = 'Speedy Express';
    
Ans: 54

### Question 2b: What is the last name of the employee with the most orders?

    SELECT LastName, MAX(MostOrders)

    FROM (SELECT LastName, COUNT(*) AS MostOrders

      FROM (Orders INNER JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID)

      GROUP BY Employees.EmployeeID);

Ans: Peacock
    
### Question 2c: What product was ordered the most by customers in Germany?

    SELECT ProductID,ProductName,MAX(GermanCustomerOrders)

    FROM (SELECT Products.ProductID,ProductName,SUM(Quantity)AS GermanCustomerOrders

	    FROM (((Products INNER JOIN OrderDetails ON Products.ProductID=OrderDetails.ProductID)

		    INNER JOIN Orders ON OrderDetails.OrderID = Orders.OrderID)

		    INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID)

	    WHERE Country = 'Germany'

	    GROUP BY Products.ProductID);
      
Ans: Boston Crab Meat

