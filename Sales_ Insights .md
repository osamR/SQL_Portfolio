
SELECT * FROM customers;



SELECT count(*) FROM customers;



SELECT * FROM transactions where market_code='Mark001';

#Show distrinct product codes that were sold in chennai

SELECT distinct product_code FROM transactions where market_code='Mark001';

Show transactions where currency is US dollars

#SELECT * from transactions where currency="USD"

Show transactions in 2020 join by date table

SELECT transactions.*, date.* FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020;
#Show total revenue in year 2020,

SELECT SUM(transactions.sales_amount) 
FROM transactions INNER JOIN date 
ON 
transactions.order_date=date.date where date.year=2020 
and transactions.currency="INR\r" or transactions.currency="USD\r";

#Show total revenue in year 2020, January Month,

SELECT 
SUM(transactions.sales_amount)
FROM transactions INNER JOIN date 
ON transactions.order_date=date.date where date.year=2020 a
nd and date.month_name="January" and (transactions.currency="INR\r" or transactions.currency="USD\r");

#Show total revenue in year 2020 in Chennai

SELECT 
SUM(transactions.sales_amount) 
FROM transactions INNER JOIN date
 ON transactions.order_date=date.date where date.year=2020 
 and transactions.market_code="Mark001";
