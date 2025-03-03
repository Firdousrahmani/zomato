# zomato
Zomato_sales_trend

![schema](https://github.com/Firdousrahmani/zomato/blob/main/schema.png)

# I recently worked on a Zomato Sales Analysis project using SQL and Excel, where I explored key customer spending behaviors and product trends.
# This dataset provided valuable insights into customer purchasing patterns and menu popularity.

**Key Insights:**

Total Customer Spend – Calculated how much each user spent in total on Zomato.
Customer Visit Frequency – Analyzed how many unique days each customer visited.
First Purchase Analysis – Identified the first product each customer purchased.
Most Purchased Item – Found the highest-selling product and how often it was bought.
Customer Preferences – Determined each user's most frequently purchased item.

1. what is the total amount each customer spent on zomato ?

    select sales.userid as users_id  , sum(product.price) as total_spent
    from sales
    join product 
    on sales.product_id = product.product_id
    group by users_id
    order by users_id asc

2. how many days has each customer visited zomato?

     select userid, count(distinct created_date)
     from sales
    group by userid

3. what was the first product purcahsed by each customer?

    select * from
    (select * , rank() over(partition by userid order by created_date ) 
    rank from sales) a where rank = 1

4. what is the most purchased item in the menu and how
 many times was it purchased by all customers.


    SELECT userid, COUNT(product_id) AS cnt
    FROM sales
    WHERE product_id = 
    (SELECT product_id
    FROM sales
    GROUP BY product_id
    ORDER BY COUNT(product_id) DESC
    LIMIT 1)

    GROUP BY userid;

5. which item was the most popular for each customer.

    select * from

    (select *, rank() over (partition by userid order by cnt desc) rnk
     from

    (select userid, product_id, count(product_id) as cnt
    from sales 
    group by userid, product_id)a)b

    where rnk = 1


