https://www.mysqltutorial.org/tryit/query/mysql-inner-join/#1

```sql
SELECT productCode, textDescription
FROM products T1
INNER JOIN productlines T2 USING(productline);
```

```sql
SELECT 
    productCode, productName, textDescription
FROM
    products T1
        INNER JOIN
    productlines T2 ON T1.productline = T2.productline;
```    


```sql
SELECT 
    T1.orderNumber,
    status,
    SUM(quantityOrdered * priceEach) total
FROM
    orders AS T1
        INNER JOIN
    orderdetails AS T2 ON T1.orderNumber = T2.orderNumber
GROUP BY orderNumber;
```
  
