https://www.mysqltutorial.org/tryit/query/mysql-inner-join/#1

```sql
SELECT productCode,
         textDescription
FROM products T1
INNER JOIN productlines T2 USING(productline);
```
  
