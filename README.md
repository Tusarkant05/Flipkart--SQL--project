# Flipkart--SQL--project
![](https://github.com/Tusarkant05/Flipkart--SQL--project/blob/main/flipkart.jpg)

## Flipkart E-commerce SQL project

Here my sql project, where i analyze real-time data from flipkart! This project uses a dataset of 20,000+ sales records and additionals tables for payments, products, and shipping data to explore and analyze e-commerce transactions, product sales, and customer interactions. The project aims to slove business problems through sql quries, helping Flipkart make informed decisions.

## Table of contents
- [Introduction](#introduction)
- [Project Structure](#project-structure)
- [Database Schema](#database-schema)
- [Business Problems](#business-problems)
- [SQL Queries & Analysis](#sql-queries--analysis)
- [Contact Me](#contact-me)

---

## Introduction
This project demonstrates essential SQL skills by analyzing e-commerce data from flipkart, focuing on sales, payments, products, and customer data. Through SQL, we answer critical business questions, uncover trends, and derive actionable insights that help improve business strategies and customer experiences. This project covers different SQL techniques including joins, group by, aggregations, and Date functions.

## Project Structure

1. **SQL Scripts**: Contains Code to create the database schema and write queries for analysis.
2. **Dataset**: Real-time data representing e-commerce transactions, product details, customer information, and shipping status.
3. **Analysis**: SQL queries crafted to slove business problems, each focusing on understanding e-commerce sales and performance.

---

## Database Schema

Here’s an overview of the database structure:

### 1. **Customers Table**
- **customer_id**: Unique identifier for each customer
- **customer_name**: Name of the customer
- **state**: Location(State) of the customer

### 2. **Products Table**
- **Product_id**: unique identifier for each product
- **product_name**: Name of the product
- **price**: price of the product
- **cogs**: cost of goods sold
- **category**: category of the product
- **brand**: Brand name of the product
 
### 3. **Sales Table**
- **order_id**: Unique order identifier
- **order_date**: Date the order was placed
- **customer_id**: Linked to the **Customers** table
- **order_status**: Status of the order (e.g., Completed, Cancelled)
- **product_id**: Linked to the **products** table
- **quantity**: Quantity of products sold
- **price_per_unit**: Price per unit of the product

### 4. **Payments Table**
- **Payment_id**: Unique payment identifier
- **order_id**: Linked to the **sales** table
- **Payment_date**: date the payment was made
- **payment-status**: status of the payment (e.g., Payment successed, payment failed)
     
### 5. **shipping Table**
- **shipping_id**: Unique shipping identifier
- **order_id**: Linked to the **sales** table
- **shipping_date**: Date the order was shipped
- **return_date**: Date the order was returned (if applicable)
- **Shipping_providers**: Shipping provider (e.g., Ekart, Bludart)
- **Delivery_status**: Status of the delivery (e.g., Delivered, Returned)

 ## Business Problems

 The following queries were created to solve specific business questions. Each query is designed to provide insights based on slaes, payments, products, and customer data.

1. List all products and show the details of customers who have placed orders for them. Include products that have no orders.
2. list all orders and their shipping status. Include orders that do not have any shipping records.
3. Find the total number of completed orders made by customer from the state 'Delhi'.
4. Retrive a list of products ordered by customers from the state 'karnataka' with price greater than 10,000.
5. Find the total quantity of each product ordered by customers from 'Delhi' and only include products with a quantity greater than 5.
6. get the average payment amount per customer who has placed more than 3 orders.
7. Show the number of customers in each state who have made purchases with a total spend greater than 50,000.
8. Retrieve the total sales per customer in 'Delhi' where the order status is 'Completed', only include those with total sales greater than 50,000. and order the results by total sales.
9. Show the total quantity sold per product in "Accessories" category where the total quantity sold is greater than 50 and order the result by product name.
10. Find the total number of orders for customer from "Maharashtra" who have spent more than 1,00,000 and order the result by the total amount spent.
11. retrieve the number of payments made per customer where payment status is "Payment Successed" and group by customer, and ordering by payment count.
12. list all orders that were placed within the year 2023.
13. Retrieve customer who have made purchased in the month of january.


## Getting Started

### Creating Tables

```sql
create table products
(
	product_id int primary key,
	product_name  varchar(45),
	price float,
	cogs float,
	category varchar(25),
	brand varchar(25)
);
```

```sql
create table customers
(
	customer_id int primary key,
	customer_name varchar(35),
	state varchar(35)
);
```

```sql
create table sales
(
	order_id int primary key,
	order_date date,
	customer_id int references customers (customer_id),
	order_status varchar(15),
	product_id int references products (product_id),
	quantity int,
	price_per_unit float
);
```

```sql
create table payments
(
	payment_id int primary key,
	order_id int references sales(order_id),
	payment_date date,
	payment_status varchar(35)
);
```

```sql
create table shipping 
(
	shipping_id int primary key,
	order_id int references sales(order_id),
	shipping_date date,
	return_date date,
	shipping_providers varchar(20),
	delivery_status varchar(25)
);
```
## Q&A
### 1.List all products and show the details of customers who have placed orders for them. Include products that have no orders.
```sql
select *
from products as p
left join 
sales as s
on p.product_id = s.product_id
```
### 2. list all orders and their shipping status. Include orders that do not have any shipping records.
```sql
select * 
from sales as s
left join 
shippings as sp
on s.order_id = sp.order_id
```
### 3. Find the total number of completed orders made by customer from the state 'Delhi'.
```sql
select * 
from customers as c
join
sales as s
on c.customer_id = s.customer_id
where 
	c.state = 'Delhi'
	and
	s.order_status = 'Completed'
```
### 4. Retrive a list of products ordered by customers from the state 'karnataka' with price greater than 10,000.
```sql
select * 
from customers as c
join
sales as s
on c.customer_id = s.customer_id
join
products as p
on s.product_id = p.product_id
where
	c.state = 'Karnataka'
	and
	p.price > 10000
```
### 5. Find the total quantity of each product ordered by customers from 'Delhi' and only include products with a quantity greater than 5.
```sql
select
	p.product_name,
	sum(s.quantity) as total_qty
from customers as c
join
sales as s
on c.customer_id = s.customer_id
join
products as p
on s.product_id = p.product_id
where
	c.state = 'Delhi'
group by 1
having sum(s.quantity) >5;
```
### 6. get the average payment amount per customer who has placed more than 3 orders.
```sql
select 
	s.customer_id,
	avg(s.price_per_unit) as avg_price
from sales as s
join payments as pay
on s.order_id = pay.order_id
group by 1
having count(pay.order_id) >3
```
### 7. Show the number of customers in each state who have made purchases with a total spend greater than 50,000.
```sql
select
	c.state,
	count(distinct c.customer_name)as total_customer,
	sum(s.price_per_unit) as total_spent
from sales as s
join 
customers as c
on s.customer_id = c.customer_id
group by 1
having sum(s.price_per_unit) > 50000
```
### 8. Retrieve the total sales per customer in 'Delhi' where the order status is 'Completed', only include those with total sales greater than 50,000. and order the results by total sales.
```sql
select
	c.state,
	s.order_status,
	sum(s.price_per_unit) as total_sales
from customers as c
join
sales as s
on c.customer_id = s.customer_id
where
	c.state = 'Delhi'
	and
	s.order_status = 'completed'
group by 1,2
having sum(s.price_per_unit) > 50000
order by 3
```
### 9. Show the total quantity sold per product in "Accessories" category where the total quantity sold is greater than 50 and order the result by product name.
```sql
select
	p.product_name,
	p.category,
	sum(s.quantity) as total_qty
from sales as s
join
products as p
on s.product_id = p.product_id
where
	p.category = 'Accessories'
group by 1,2
having sum(s.quantity) >50
order by 1
```
### 10. Find the total number of orders for customer from "Maharashtra" who have spent more than 1,00,000 and order the result by the total amount spent.
```sql
select
	c.customer_id,
	c.customer_name,
	c.state,
	count(s.order_id) as total_orders,
	sum(s.price_per_unit) as total_spent
from customers as c
join
sales as s
on c.customer_id = s.customer_id
where
	c.state = 'Maharashtra'
group by 1,2
having sum(s.price_per_unit) > 100000
order by 4 desc
```
### 11. retrieve the number of payments made per customer where payment status is "Payment Successed" and group by customer, and ordering by payment count.
```sql
select
	c.customer_id,
	c.customer_name,
	pay.payment_status,
	count(pay.payment_id) as payment_count
from customers as c
join
sales as s
on c.customer_id = s.customer_id
join
payments as pay
on s.order_id = pay.order_id
where
	pay.payment_status = 'Payment Successed'
group by 1,2,3
order by 4
```
### 12. list all orders that were placed within the year 2023.
```sql
select *,
	extract(year from order_date) as year
from sales
where
	extract(year from order_date) = 2023
```
### 13. Retrieve customer who have made purchased in the month of january. 
```sql
select *
from sales
where
	to_char(order_date, 'month') = 'January'
```

## Contact Me

📄 **[Resume](#)**  
📧 **[Email](jtusarkant@gmail.com)**  
📞 **Phone**: +91-7684-028977  

--
