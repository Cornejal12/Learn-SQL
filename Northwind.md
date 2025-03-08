# Base de Datos Northwind
###### Diagrama 
![Captura de pantalla 2025-03-07 220621](https://github.com/user-attachments/assets/69fefc12-bf7d-426d-8055-67d580baf47c)

## Nivel Básico
##### Pregunta 1
Show the category_name and description from the categories table sorted by category_name.
```
select
category_name, 
description
from categories
order by category_name
```
##### Pregunta 2
Show all the contact_name, address, city of all customers which are not from 'Germany', 'Mexico', 'Spain'
```
select
contact_name,
address,
city
from customers
where country not in('Germany','Mexico','Spain')
```
##### Pregunta 3
Show order_date, shipped_date, customer_id, Freight of all orders placed on 2018 Feb 26
```
select
order_date,
shipped_date,
customer_id,
freight
from orders
where date(order_date) = '2018-02-26'
```
##### Pregunta 4
Show the employee_id, order_id, customer_id, required_date, shipped_date from all orders shipped later than the required date
```
select
employee_id,
order_id, 
customer_id,
required_date,
shipped_date
from orders
where shipped_date > required_date
```
##### Pregunta 5
Show all the even numbered Order_id from the orders table
```
select
order_id
from orders
where order_id%2=0
```
##### Pregunta 6
Show the city, company_name, contact_name of all customers from cities which contains the letter 'L' in the city name, sorted by contact_name
```
select
city,
company_name,
contact_name
from customers
where city like '%L%'
order by contact_name
```
##### Pregunta 7
Show the company_name, contact_name, fax number of all customers that has a fax number. (not null)
```
select
company_name,
contact_name,
fax
from customers
where fax not null
```
##### Pregunta 8
Show the first_name, last_name. hire_date of the most recently hired employee.
```
select
first_name,
last_name,
max(hire_date)
from employees
```
##### Pregunta 9
Show the average unit price rounded to 2 decimal places, the total units in stock, total discontinued products from the products table.
```
select
ROUND(avg(unit_price),2),
sum(units_in_stock),
COUNT(CASE WHEN discontinued = 1 THEN 1 END) AS discontinued_count
from products
```

## Nivel Medio
##### Pregunta 1
Show the ProductName, CompanyName, CategoryName from the products, suppliers, and categories table
```
select
a.product_name, 
b.company_name,
c.category_name
from products as a
left join suppliers as b on b.supplier_id = a.supplier_id
left join categories as c on c.category_id = a.category_id
```
##### Pregunta 2
Show the category_name and the average product unit price for each category rounded to 2 decimal places.
```
select
c.category_name,
ROUND(avg(a.unit_price),2)as average_unit_price
from products as a
left join categories as c on c.category_id = a.category_id
GROUP BY c.category_name
```
##### Pregunta 3
Show the city, company_name, contact_name from the customers and suppliers table merged together.
Create a column which contains 'customers' or 'suppliers' depending on the table it came from.
```
SELECT city, company_name, contact_name, 'customers' as relationship
FROM customers
UNION
SELECT city, company_name, contact_name,'suppliers' as relationship
FROM suppliers
```
##### Pregunta 4
Show the total amount of orders for each year/month.
```
select
year(order_date) as order_year,
month(order_date) as order_month,
count(*) as no_of_orders
from orders
group by year(order_date), month(order_date)
```
## Nivel Avanzado
##### Pregunta 1
Show the employee's first_name and last_name, a "num_orders" column with a count of the orders taken, and a column called "Shipped" that displays "On Time" if the order shipped_date is less or equal to the required_date, "Late" if the order shipped late, "Not Shipped" if shipped_date is null.
Order by employee last_name, then by first_name, and then descending by number of orders.
```
SELECT
    b.first_name, 
    b.last_name,
    COUNT(order_id) AS num_orders,
    CASE 
        WHEN a.shipped_date <= a.required_date THEN 'On Time'
        WHEN a.shipped_date > a.required_date THEN 'Late'
        ELSE 'Not Shipped'
    END AS shipped
FROM orders AS a
LEFT JOIN employees AS b ON b.employee_id = a.employee_id
GROUP BY b.first_name, b.last_name, shipped
ORDER BY b.last_name, b.first_name, num_orders DESC;
```
##### Pregunta 2
Show how much money the company lost due to giving discounts each year, order the years from most recent to least recent. Round to 2 decimal places
```
select
year(b.order_date) as order_year,
round(sum(a.discount * a.quantity * c.unit_price),2) as discount
from order_details as a
left join orders as b on b.order_id = a.order_id
left join products as c on c.product_id =  a.product_id
group by order_year
order by year(b.order_date) desc
```
