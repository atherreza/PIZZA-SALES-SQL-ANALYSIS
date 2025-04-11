# PIZZA-SALES-SQL-ANALYSIS
🍕 PIZZA SALES SQL ANALYSIS
THIS PROJECT CONTAINS SQL QUERIES USED TO ANALYZE SALES DATA FROM A PIZZA STORE. THE GOAL IS TO DERIVE INSIGHTS FROM ORDER INFORMATION USING BASIC AND INTERMEDIATE SQL OPERATIONS.

📂 DATASET DESCRIPTION
THE PROJECT IS BASED ON A RELATIONAL DATABASE WITH THE FOLLOWING TABLES:

ORDERS – CONTAINS ORDER ID, ORDER DATE, AND ORDER TIME.

ORDER_DETAILS – CONTAINS ORDER DETAILS INCLUDING QUANTITY AND PIZZA ID.

PIZZAS – CONTAINS PIZZA SIZE, PRICE, AND PIZZA TYPE ID.

PIZZA_TYPES – CONTAINS PIZZA NAME AND CATEGORY.

🔍 QUERIES INCLUDED
🔹 BASIC QUERIES
RETRIEVE TOTAL NUMBER OF ORDERS PLACED

CALCULATE TOTAL REVENUE GENERATED FROM PIZZA SALES

IDENTIFY THE HIGHEST-PRICED PIZZA

FIND THE MOST COMMON PIZZA SIZE ORDERED

LIST TOP 5 MOST ORDERED PIZZA TYPES BY QUANTITY

🔸 INTERMEDIATE QUERIES
FIND TOTAL QUANTITY OF EACH PIZZA CATEGORY ORDERED

ANALYZE DISTRIBUTION OF ORDERS BY HOUR

FIND CATEGORY-WISE DISTRIBUTION OF PIZZAS

CALCULATE AVERAGE NUMBER OF PIZZAS ORDERED PER DAY

🛠️ TOOLS USED
MYSQL

SQL JOINS, AGGREGATE FUNCTIONS, GROUP BY, ORDER BY, LIMIT, DATE & TIME FUNCTIONS

🎯 OBJECTIVE
TO PRACTICE AND DEMONSTRATE SQL SKILLS THROUGH A HANDS-ON PROJECT THAT ANALYZES REALISTIC SALES DATA AND GENERATES MEANINGFUL BUSINESS INSIGHTS.

📈 SKILLS DEVELOPED
DATA RETRIEVAL & FILTERING

DATA AGGREGATION & ANALYSIS

RELATIONAL DATABASE UNDERSTANDING

QUERY OPTIMIZATION BASICS
-- Basic:
-- 1.Retrieve the total number of orders placed.
select count(*) from orders;

-- 2.Calculate the total revenue generated from pizza sales.
select 
round(sum(od.quantity*p.price),2)
from order_details od
join pizzas p on od.pizza_id = p.pizza_id;

-- 3. Identify the highest-priced pizza.

select pt.name, p.price
from pizzas p
join pizza_types pt on p.pizza_type_id = pt.pizza_type_id
order by price desc
limit 1
;

-- 4.Identify the most common pizza size ordered.
select p.size, count(od.order_details_id)
from pizzas p
join order_details od on p.pizza_id = od.pizza_id
group by p.size
order by p.size
limit 1;

-- 5.List the top 5 most ordered pizza types along with their quantities.
select pt.name, sum(od.quantity) as total_quantity
from pizza_types pt
join pizzas p on pt.pizza_type_id = p.pizza_type_id
join order_details od on od.pizza_id = p.pizza_id
group by  pt.name
order by sum(od.quantity) desc
limit 5;

-- Intermediate:
-- 6.Join the necessary tables to find the total quantity of each pizza category ordered.
SELECT * FROM pizzahut.pizza_types;
select pt.category, sum(od.quantity) as quantity
from pizza_types pt
join pizzas p on pt.pizza_type_id = p.pizza_type_id
join order_details od on od.pizza_id = p.pizza_id
group by  pt.category;



-- 7.Determine the distribution of orders by hour of the day.
select  hour(order_time) as hours, count(order_id) as order_count
from orders
group by hour(order_time);

-- 8.Join relevant tables to find the category-wise distribution of pizzas.
select category, count(category) as count
from pizza_types
group by category;

-- 9. Group the orders by date and calculate the average number of pizzas ordered per day.
select orders.order_date, sum(order_details.quantity)
from orders join order_details
on orders.order_id = order_details.order_id
group by orders.order_date;
