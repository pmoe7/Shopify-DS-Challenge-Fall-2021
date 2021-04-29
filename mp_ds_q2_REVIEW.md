# Shopify Fall 2021 Data Science Intern Challenge
### By Mohammed Perves
### 4th Year BBA & CS Student at Wilfrid Laurier University

## Question 2
### A) How many orders were shipped by Speedy Express in total?

```sql
SELECT COUNT(*) as NumOrders
FROM Orders 
WHERE ShipperID = (SELECT ShipperID FROM Shippers WHERE ShipperName = "Speedy Express");
```
### Result:
Number of Records: 1  

**NumOrders:** 54


### B) What is the last name of the employee with the most orders?

```sql
SELECT e.LastName FROM Employees e 
INNER JOIN Orders o ON e.EmployeeID = o.EmployeeID 
GROUP BY e.EmployeeID, e.LastName 
HAVING COUNT(*) = (	SELECT MAX(numberOrders) FROM (	SELECT EmployeeID, COUNT(OrderID) AS numberOrders 
FROM Orders 
GROUP BY EmployeeID) maxOrders);
```
### Result:
Number of Records: 1  

**LastName:** Peacock


### C) What product was ordered the most by customers in Germany?

```sql
SELECT p.ProductName
FROM Products p
INNER JOIN (
            SELECT ProductID, MAX(Quantity)
            FROM (
                  SELECT ProductID, SUM(Quantity) as Quantity
                  FROM Orders o
                  INNER JOIN (
                    SELECT CustomerID
                    FROM Customers
                    WHERE Country = 'Germany') germanCustomer 
                  ON o.CustomerID = germanCustomer.CustomerID
                  INNER JOIN OrderDetails od
                  ON od.OrderID = o.OrderID
                  GROUP BY ProductID )
                  ) mP
ON mP.ProductID = p.ProductID
```

### Result:
Number of Records: 1  

**ProductName:** Boston Crab Meat
