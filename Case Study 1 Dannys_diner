# Dannys_Diner
This is part of Danny Ma's 8 weeks SQL challenge. You can find the challenge on his LinkedIn Profile @ Danny Ma or [here](https://8weeksqlchallenge.com/case-study-1/).

## Background
Danny who loves japanese food, opened up a small restaurant to sell his 3 favorite foods and needs help to keep the restaurant afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.

## Challenge
Danny wants to use the data to answer a few simple questions about his customers, which will hep him decide whether he should expand the existing customer loyalty program. 
Danny has shared with you 3 key datasets for this case study:
* sales
* menu
* members

Then we can go on ahead and answer the questions
PS: Each of the following case study questions can be answered using a single SQL statement:

Questions and their Solutions
First let us View our Tables

SELECT *
FROM dannys_diner.menu;
SELECT *
FROM dannys_diner.sales;
SELECT *
FROM dannys_diner.members;

-- 1. What is the total amount each customer spent at the restaurant?

SELECT 
	customer_id, 
    SUM(price)
FROM dannys_diner.menu 
JOIN dannys_diner.sales
On menu.product_id = sales.product_id
GROUP BY customer_id

-- 2. How many days has each customer visited the restaurant?

SELECT customer_id,
COUNT (distinct (order_date)) 
FROM  dannys_diner.sales
GROUP BY customer_id 


-- 3. What was the first item from the menu purchased by each customer?

With FIRSTPURCHASE AS
(
SELECT sales.customer_id, 
       menu.product_name, 
       sales.order_date,
       DENSE_RANK() OVER (PARTITION BY sales.Customer_ID ORDER BY sales.order_date) AS purchase_rank
FROM dannys_diner.menu
JOIN dannys_diner.sales
ON menu.product_id = sales.product_id
GROUP BY sales.customer_id, menu.product_name, sales.order_date
)
SELECT customer_id, product_name
FROM firstpurchase
WHERE purchase_rank = 1

-- 4. What is the most purchased item on the menu and how many times was it purchased by all customers?

SELECT menu.product_name, COUNT(sales.product_id) AS Top 1
 FROM dannys_diner.menu
    JOIN dannys_diner.sales
    ON menu.product_id = sales.product_id
    GROUP BY menu.product_name
ORDER BY COUNT(sales.product_id) desc
limit 1

-- 5. Which item was the most popular for each customer?

with Rank as(
SELECT sales.customer_id,
menu.product_name,
COUNT(*) AS order_count,
DENSE_RANK() OVER (PARTITION BY sales.customer_id
ORDER BY COUNT(sales.customer_id)desc) as Rank
 FROM dannys_diner.menu
    JOIN dannys_diner.sales
    ON menu.product_id = sales.product_id
GROUP BY sales.customer_id,menu.product_name
)
SELECT Customer_id,Product_name,order_Count
FROM Rank
WHERE Rank = 1

-- 6. Which item was purchased first by the customer after they became a member?

WITH Top_member_sales AS 
(
SELECT sales.customer_id, members.join_date, sales.order_date, sales.product_id,
DENSE_RANK() OVER(PARTITION BY sales.customer_id
ORDER BY sales.order_date) AS Rank
FROM sales AS s
JOIN members AS m
ON menu.customer_id = sales.customer_id
WHERE sales.order_date >= member.join_date
)

SELECT s.customer_id, s.order_date, m2.product_name 
FROM top_member_sales AS s
JOIN menu AS m2
ON m2.product_id = s.product_id
WHERE rank = 1;

-- 7. Which item was purchased just before the customer became a member?

With prior_member_customer_purchase as
(
Select  sales.customer_id,
        members.join_date,
  		sales.order_date,
  		sales.product_id,
	DENSE_RANK() OVER (PARTITION by sales.customer_id ORDER BY sales.order_date) as Rank
From sales AS S
Join Menu AS M
ON m.product_id = s.product_id
JOIN Members Mem
ON Mem.Customer_id = S.customer_id
Where S.order_date < Mem.join_date  
)
SELECT customer_ID, Product_name
FROM Rank
WHERE Rank = 1

-- 8. What is the total items and amount spent for each member before they became a member?

SSELECT s.customer_id, COUNT(DISTINCT s.product_id) AS unique_menu_item, SUM(m.price) AS total_sales
FROM dannys_diner.sales AS s
JOIN dannys_diner.members AS Mem
ON Mem.customer_id = s.customer_id
JOIN dannys_diner.menu AS m
ON m.product_id = S.product_id
WHERE S.order_date < mem.join_date
GROUP BY s.customer_id;

Data Output
"customer_id"	"unique_menu_item"	"total_sales"
"A"	2	25
"B"	2	40


-- 9.  If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?

WITH Price_points AS
(
SELECT *, 
CASE
WHEN product_id = 1 THEN price * 20
ELSE price * 10
END AS points
FROM dannys_diner.menu
) SELECT S.customer_id, SUM(P.points) AS total_points
FROM price_points AS P
JOIN dannys_diner.sales AS S
ON p.product_id = s.product_id
GROUP BY s.customer_id 
order by total_points desc
;

DataOutput
"customer_id"	"total_points"
"B"	940
"A"	860
"C"	360

-- 10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

WITH dates AS 
(
   SELECT *, 
      DATEADD(DAY, 6, join_date) AS valid_date, 
      EOMONTH('2021-01-31') AS last_date
   FROM members 
)
SELECT S.Customer_id, 
       SUM(
	   CASE 
	  WHEN m.product_id = 1 THEN m.price*20
	  WHEN S.order_date BETWEEN D.join_date AND D.valid_date THEN m.price*20
	  Else m.price*10
	  END 
	  ) as Points
From Dates AS D
join Sales AS S
On D.customer_id = S.customer_id
Join Menu M
On M.product_id = S.product_id
Where S.order_date < d.last_date
Group by S.customer_id
