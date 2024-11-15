# Flipkart E-commerce SQL Project



Welcome to my SQL project, where I analyze real-time data from **Flipkart**! This project uses a dataset of **20,000+ sales records** and additional tables for payments, products, and shipping data to explore and analyze e-commerce transactions, product sales, and customer interactions. The project aims to solve business problems through SQL queries, helping Flipkart make informed decisions.

## Table of Contents
- [Introduction](#introduction)
- [Project Structure](#project-structure)
- [Database Schema](#database-schema)
- [Business Problems](#business-problems)
- [SQL Queries & Analysis](#sql-queries--analysis)
- [Getting Started](#getting-started)
- [Questions & Feedback](#questions--feedback)
- [Contact Me](#contact-me)

---

## Introduction

This project demonstrates essential SQL skills by analyzing e-commerce data from **Flipkart**, focusing on sales, payments, products, and customer data. Through SQL, we answer critical business questions, uncover trends, and derive actionable insights that help improve business strategies and customer experiences. The project covers different SQL techniques including **Joins**, **Group By**, **Aggregations**, and **Date Functions**.

## Project Structure

1. **SQL Scripts**: Contains code to create the database schema and write queries for analysis.
2. **Dataset**: Real-time data representing e-commerce transactions, product details, customer information, and shipping status.
3. **Analysis**: SQL queries crafted to solve business problems, each focusing on understanding e-commerce sales and performance.

---

## Database Schema

Here’s an overview of the database structure:

### 1. **Customers Table**
- **customer_id**: Unique identifier for each customer
- **customer_name**: Name of the customer
- **state**: Location (state) of the customer

### 2. **Products Table**
- **product_id**: Unique identifier for each product
- **product_name**: Name of the product
- **price**: Price of the product
- **cogs**: Cost of goods sold
- **category**: Category of the product
- **brand**: Brand name of the product

### 3. **Sales Table**
- **order_id**: Unique order identifier
- **order_date**: Date the order was placed
- **customer_id**: Linked to the `customers` table
- **order_status**: Status of the order (e.g., Completed, Cancelled)
- **product_id**: Linked to the `products` table
- **quantity**: Quantity of products sold
- **price_per_unit**: Price per unit of the product

### 4. **Payments Table**
- **payment_id**: Unique payment identifier
- **order_id**: Linked to the `sales` table
- **payment_date**: Date the payment was made
- **payment_status**: Status of the payment (e.g., Payment Successed, Payment Failed)

### 5. **Shippings Table**
- **shipping_id**: Unique shipping identifier
- **order_id**: Linked to the `sales` table
- **shipping_date**: Date the order was shipped
- **return_date**: Date the order was returned (if applicable)
- **shipping_providers**: Shipping provider (e.g., Ekart, Bluedart)
- **delivery_status**: Status of delivery (e.g., Delivered, Returned)

## Business Problems

The following queries were created to solve specific business questions. Each query is designed to provide insights based on sales, payments, products, and customer data.

1.Retrieve all products along with their total sales revenue from completed orders.
2.List all customers and the products they have purchased, showing only those who have ordered more than two products.
3.Find the total amount spent by customers in 'Gujarat' who have ordered products priced greater than 10,000.
4.Retrieve the list of all orders that have not yet been shipped.
5.Find the average order value per customer for orders with a quantity of more than 5.
6.Get the top 5 customers by total spending on 'Accessories'.
7.Retrieve a list of customers who have not made any payment for their orders.
8.Find the most popular product based on total quantity sold in 2023.
9.List all orders that were cancelled and the reason for cancellation (if available).
10.Retrieve the total quantity of products sold by category in 2023.
11.Get the count of returned orders by shipping provider in 2023.
12.Show the total revenue generated per month for the year 2023.
13.Find the customers who have made the most purchases in a single month.
14.Retrieve the number of orders made per product category in 2023 and order by total quantity sold.
15.List the products that have never been ordered (use LEFT JOIN between products and sales).

---

## SQL Queries & Analysis

The `queries.sql` file contains all SQL queries developed for this project. Each query corresponds to a business problem and demonstrates skills in SQL syntax, data filtering, aggregation, grouping, and ordering.

---

## Getting Started

### Prerequisites
- PostgreSQL (or any SQL-compatible database)
- Basic understanding of SQL

### Steps
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/flipkart-sql-project.git
   ```
2. **Set Up the Database**:
   - Run the `schema.sql` script to set up tables and insert sample data.

3. **Run Queries**:
   - Execute each query in `queries.sql` to explore and analyze the data.

---

## Questions & Feedback

Feel free to add your questions and code snippets below and submit them as issues for further feedback!

**Example Questions**:
1. **Question**: How can I filter orders placed in the last 6 months?
   **Code Snippet**:
   ```sql
   SELECT * FROM sales
   WHERE order_date >= CURRENT_DATE - INTERVAL '6 months';
   ```

2. --Retrieve all products along with their total sales revenue from completed orders.
```sql
select 
	p.product_name,
	p.product_id,
	sum(s.price_per_unit*s.quantity) as total_sales
from 
products p join sales s on p.product_id=s.product_id
where order_status='Completed'
group by p.product_name,p.product_id
```

3.--List all customers and the products they have purchased, showing only those who have ordered more than 
--two products.
```sql
select 
	c.customer_id,
	c.customer_name,
	s.quantity 
from customers c join sales s 
on s.customer_id=c.customer_id
where s.quantity > 2
```
4.--Find the total amount spent by customers in 'Gujarat' who have ordered products priced greater than 10,000.

```sql
select 
	c.customer_name,
	c.state,
	sum(s.quantity*s.price_per_unit) as total_amount
from customers c  join sales s on s.customer_id=c.customer_id
join products p on p.product_id=s.product_id
where c.state='Gujarat' 
group by c.customer_name,c.state
having  sum(s.quantity*s.price_per_unit)> 10000
```
5.--Retrieve the list of all orders that have not yet been shipped.
```sql
select 
	sh.order_id,
	sh.shipping_id,
	sh.delivery_status 
from shippings sh join sales s 
on s.order_id=sh.order_id
where delivery_status<>'shipped'

```
6.--Find the average order value per customer for orders with a quantity of more than 5.
```sql
select 
	c.customer_name,
	s.customer_id,
	round(avg(quantity),2) as avg_order 
from customers c join sales s 
on c.customer_id=s.customer_id 
where s.quantity > 5
group by c.customer_name,s.customer_id
```
7.--Get the top 5 customers by total spending on 'Accessories'.
```sql
select  
	c.customer_id,
	c.customer_name,
	sum(s.quantity*s.price_per_unit) as total_spend 
from customers c join sales s
on s.customer_id=c.customer_id
join products p on p.product_id=s.product_id
where category='Accessories'
group by p.category,p.product_id,c.customer_id,s.order_id
order by total_spend desc
limit 5
```
8.--Retrieve a list of customers who have not made any payment for their orders.

```sql
select * from payments

select c.customer_id,c.customer_name
from customers c left join sales s on s.customer_id=c.customer_id left join payments p 
on p.order_id=s.order_id
where p.order_id is null
```
9.--Find the most popular product based on total quantity sold in 2023.
select * from sales
select * from products
```sql
select 
	p.product_id,
	p.product_name,
	sum(quantity) as total_quantity
from sales s join products p 
on s.product_id=p.product_id
where EXTRACT(Year from order_date)='2023'
group by 1,2
order by sum(quantity) desc
limit 1
```
10.--List all orders that were cancelled and the reason for cancellation (if available).
```sql
select distinct order_status from sales
select 
	order_id,
	order_status
from sales
where order_status='Cancelled'
```
11.--Retrieve the total quantity of products sold by category in 2023.
```sql
select 
	p.category,
	sum(s.quantity) as total_quantity
from products p join sales s on s.product_id=p.product_id
where EXTRACT(YEAR from s.order_date)='2023'
group by 1
```
12.--Get the count of returned orders by shipping provider in 2023.
```sql
select distinct delivery_status from shippings
select * from sales
select * from shippings

select
	shipping_providers,
	count(s.order_id)
from shippings sh join sales s on s.order_id=sh.order_id
where delivery_status='Returned' and EXTRACT(YEAR from shipping_date)='2023'
group by 1
```

13.--Show the total revenue generated per month for the year 2023.
```sql
select * from sales
select
	to_char(order_date,'month') as months,
	sum(price_per_unit*quantity) as total_revenue
from sales
where extract(year from order_date)='2023'
group by 1
```
14.--Find the customers who have made the most purchases in a single month.
```sql
select 
	c.customer_id,
	c.customer_name,
	to_char(s.order_date,'month') as months,
	count(order_id) as total_purchase

from customers c join sales s
on c.customer_id=s.customer_id
group by 1,2,3
order by count(order_id) desc
limit 1
```
15.--Retrieve the number of orders made per product category in 2023 and order by total quantity sold.
```sql
select * from sales
select 
	p.category,
	extract(year from s.order_date) as years,
	count(s.quantity) as total_quantity
from products p join sales s
on s.product_id=p.product_id
where extract(year from s.order_date)='2023'
group by 1,2
```
16.--List the products that have never been ordered (use LEFT JOIN between products and sales).
```sql
select 
	p.product_id,
	p.product_name
from products p left join sales s 
on p.product_id=s.product_id
where s.product_id is NULL
```













