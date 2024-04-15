# diwali-sales-analytics

## Introduction

In this analysis, we explore Diwali sales data to uncover consumer behavior patterns and trends using Python. Diwali, the festival of lights, is a significant time for shopping and festivities, making it an ideal period to analyze customer preferences and market trends. This analysis aims to provide valuable insights for businesses to optimize their strategies during festive seasons.

## Column Details

- **User_ID:** Unique identifier for each customer.
- **Cust_name:** Customer's name.
- **Product_ID:** Unique identifier for each product.
- **Gender:** Customer's gender (M for male, F for female).
- **Age:** Customer's age.
- **Marital_Status:** Customer's marital status (0 for unmarried, 1 for married).
- **State:** Customer's state of residence.
- **Zone:** Geographical zone (Western, Southern, Central, Northern).
- **Occupation:** Customer's occupation.
- **Product_Category:** Category of the product purchased.
- **Orders:** Number of orders placed.
- **Amount:** Total amount spent on purchases.
- **Status:** Status of the transaction.
- **Unnamed:** Additional information (if applicable).

  ## Query details

--01 HOW MANY RESTAURANTS HAVE A RATING GREATER THAN 4.5?
SELECT restaurant_name
FROM swiggy
WHERE rating > 4.5;
 
--WHICH IS THE TOP 1 CITY WITH THE HIGHEST NUMBER OF RESTAURANTS?
SELECT city, count(restaurant_name) as restaurant
FROM swiggy
group by city
order by restaurant desc
LIMIT 1;

--HOW MANY RESTAURANTS SELL( HAVE WORD "PIZZA" IN THEIR NAME)?
SELECT count(distinct restaurant_name) as restaurant
FROM swiggy
WHERE menu_category LIKE'%%pizza%';

--WHAT IS THE MOST COMMON CUISINE AMONG THE RESTAURANTS IN THE DATASET?
SELECT restaurant_name, count(cuisine) as cuisinee
FROM swiggy
group by restaurant_name
order by cuisinee desc;

--WHAT IS THE AVERAGE RATING OF RESTAURANTS IN EACH CITY?
SELECT restaurant_name, city, round(avg(rating), 1) as avg_rate
FROM swiggy
group by restaurant_name,city;

--WHAT IS THE HIGHEST PRICE OF ITEM UNDER THE 'RECOMMENDED' MENU CATEGORY FOR EACH RESTAURANT?
SELECT restaurant_name, max(price) as highest, item
FROM swiggy
WHERE menu_category = 'Recommended'
group by restaurant_name, item
order by highest desc
limit 1;

--FIND THE TOP 5 MOST EXPENSIVE RESTAURANTS THAT OFFER CUISINE OTHER THAN INDIAN CUISINE. 
SELECT restaurant_name, max(price) as highest
FROM swiggy
WHERE cuisine <> 'Indian Cuisine'
group by restaurant_name, cuisine
order by highest desc
LIMIT 5;

--FIND THE RESTAURANTS THAT HAVE AN AVERAGE COST WHICH IS HIGHER THAN THE TOTAL AVERAGE COST OF ALLRESTAURANTS TOGETHER.
select restaurant_name, cost_per_person
from swiggy
where cost_per_person>(
select avg(cost_per_person) from swiggy);
	
--RETRIEVE THE DETAILS OF RESTAURANTS THAT HAVE THE SAME NAME BUT ARE LOCATED IN DIFFERENT CITIES.

select distinct A.restaurant_name, A.city as City1, B.city as City2  
from swiggy as A, swiggy as B
where A.restaurant_name = B.restaurant_name and A.city <> B.city;

--WHICH RESTAURANT OFFERS THE MOST NUMBER OF ITEMS IN THE 'MAIN COURSE' CATEGORY?

select restaurant_name, menu_category, count(item) AS count_items
from swiggy
where menu_category = 'Main Course'
group by restaurant_name, menu_category
order by count_items desc
limit 1;

  --LIST THE NAMES OF RESTAURANTS THAT ARE 100% VEGETARIAN IN ALPHABETICAL ORDER OF RESTAURANT NAME
SELECT restaurant_name, 
       (COUNT(CASE WHEN veg_or_nonveg like 'Veg' THEN 1 END) * 100 / COUNT(*)) AS veg_percent
FROM swiggy
GROUP BY restaurant_name
HAVING (COUNT(CASE WHEN veg_or_nonveg like 'Veg' THEN 1 END) * 100 / COUNT(*)) = 100.00
ORDER BY restaurant_name;

  
 
--WHICH IS THE RESTAURANT PROVIDING THE LOWEST AVERAGE PRICE FOR ALL ITEMS?
select restaurant_name, avg(price) as avg_price
from swiggy
group by restaurant_name
order by avg_price
limit 1;

--WHICH TOP 5 RESTAURANT OFFERS HIGHEST NUMBER OF CATEGORIES?
SELECT distinct restaurant_name, count(distinct menu_category) as no_of_category
from swiggy
group by restaurant_name
order by no_of_category desc
limit 5;

--WHICH RESTAURANT PROVIDES THE HIGHEST PERCENTAGE OF NON-VEGETARIAN FOOD?
SELECT restaurant_name, 
       (COUNT(CASE WHEN veg_or_nonveg like 'Non-veg' THEN 1 END) * 100 / COUNT(*)) AS nonveg_percent
FROM swiggy
GROUP BY restaurant_name
ORDER BY nonveg_percent desc
limit 1;

--Determine the Most Expensive and Least Expensive Cities for Dining:
with cityexpense as(
select city, 
max(cost_per_person) as max_cost,
min(cost_per_person) as min_cost
from swiggy
group by city
)
select city, max_cost, min_cost
from cityexpense
order by max_cost desc;


--Calculate the Rating Rank for Each Restaurant Within Its City
WITH ratingrankbycity AS(
	select distinct 
	restaurant_name, 
	city, 
	rating,
	DENSE_RANK() OVER (PARTITION BY city ORDER BY rating DESC) AS rating_rank
FROM swiggy
)
SELECT restaurant_name, city, rating, rating_rank
FROM ratingrankbycity
WHERE rating_rank = 1;

--What is the average cost per person across all restaurants?
select round(avg(cost_per_person),1) as avg_cost
from swiggy;

--How does the distribution of ratings vary among different cuisine types?
select rating, cuisine
from swiggy
group by rating, cuisine
order by rating desc;

--Are there any correlations between restaurant ratings and their menu categories?
select rating, menu_category, restaurant_name
from swiggy
group by restaurant_name, menu_category, rating;


--Which city has the highest concentration of high-rated restaurants?
select city, max(rating) as highest_rating
from swiggy
group by city
order by highest_rating desc
limit 1;



## Conclusion

Based on the analysis, we found that married women in the age group of 26 to 35 years from states like Uttar Pradesh, Maharashtra, and Karnataka showed a higher interest in purchasing products during Diwali. They are primarily employed in sectors such as IT, Healthcare, and Aviation. Popular product categories among this demographic include food, clothing, and electronics.

These insights can guide businesses in tailoring their product offerings, marketing campaigns, and strategies to target this specific demographic effectively, thereby optimizing sales and enhancing customer satisfaction during festive seasons.

