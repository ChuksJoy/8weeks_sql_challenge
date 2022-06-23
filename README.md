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

CREATE TABLE sales (
  "customer_id" VARCHAR(1),
  "order_date" DATE,
  "product_id" INTEGER
);

INSERT INTO sales
  ("customer_id", "order_date", "product_id")
VALUES
  ('A', '2021-01-01', '1'),
  ('A', '2021-01-01', '2'),
  ('A', '2021-01-07', '2'),
  ('A', '2021-01-10', '3'),
  ('A', '2021-01-11', '3'),
  ('A', '2021-01-11', '3'),
  ('B', '2021-01-01', '2'),
  ('B', '2021-01-02', '2'),
  ('B', '2021-01-04', '1'),
  ('B', '2021-01-11', '1'),
  ('B', '2021-01-16', '3'),
  ('B', '2021-02-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-07', '3');


CREATE TABLE menu (
  "product_id" INTEGER,
  "product_name" VARCHAR(5),
  "price" INTEGER
);

INSERT INTO menu
  ("product_id", "product_name", "price")
VALUES
  ('1', 'sushi', '10'),
  ('2', 'curry', '15'),
  ('3', 'ramen', '12');


CREATE TABLE members (
  "customer_id" VARCHAR(1),
  "join_date" DATE
);

INSERT INTO members
  ("customer_id", "join_date")
VALUES
  ('A', '2021-01-07'),
  ('B', '2021-01-09');
