# CAR-MODEL-WITH-SQL

#Determine Popular Items

select productName, sum(quantityOrdered) as totalSold
from orderdetails 
join products  on orderdetails.productCode = products.productCode
group by productName
order by totalSold desc;

#Analysing the effect of price on sales

select productName, priceEach, sum(quantityOrdered) as totalSold
from products
join orderdetails
on products.productCode = orderdetails.productCode
group by productName, priceEach
order by priceEach;

#Managing Inventory and potential warehouse closure

update products
set quantityInStock = quantityInStock * 0.95;

select productCode, avg(quantityOrdered) as avgMonthlySales
from orderdetails
join orders on orderdetails.orderNumber = orders.orderNumber
where orderDate >= 
date_sub(now(),interval 12 month)
group by productCode;

select productCode, productName, avg(quantityOrdered)
as avgMonthlySales
from orderdetails
join products on orderdetails.productCode = products.productCode
group by productCode, productName
order by avgMonthlySales desc;

select warehouseCode, sum(quantityInStock) as totalStock
from products group by warehouseCode;

select productCode, productName, sum(quantityOrdered) as totalSold
from orderdetails join products 
on orderdetails.productCode = products.productCode
group by productCode,productName
having totalSold < threshold_values;

