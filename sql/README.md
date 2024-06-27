```sql
use e_commerce;
```

# Add age_group column to customers table
```sql
ALTER TABLE customers
ADD age_group varchar(50);
```

# Add value to age_group column in customers table
```sql
update customers
set age_group =
	case
		when age between 18 and 24 then 'Young Adults'
        when age between 25 and 44 then 'Adults'
        when age between 45 and 64 then 'Middle-aged Adults'
        else 'Seniors'
	end;
```

# Gender-based Purchase 
```sql
select * from transactions;
select
    c.gender,
    count(t.transaction_id) as total_purchases,
    round(sum(t.amount), 2) as total_purchase_amount,
    round(100.0 * count(t.transaction_id) / sum(count(t.transaction_id)) over (), 2) as gender_percentage
from transactions t
join customers c on t.customer_id = c.customer_id
group by c.gender;
```

# Age-Group-based Purchase 
```sql
select
    c.age_group,
    count(t.transaction_id) as total_purchases,
    round(sum(t.amount), 2) as total_purchase_amount,
    round(100.0 * count(t.transaction_id) / sum(count(t.transaction_id)) over (), 2) as percentage
from transactions t
join customers c on t.customer_id = c.customer_id
group by c.age_group;
```

# Gender-based Purchase 
```sql
select
    c.gender,
    sum(t.total_purchases) as total_purchases,
    round(sum(amount),2) as total_purchase_amount
from Transactions t
join Customers c on t.Customer_ID = c.Customer_ID
group by c.gender
order by total_purchase_amount desc;
```

# Country-based Purchase 
```sql
select
    c.country,
    sum(t.total_purchases) as total_purchases,
    round(sum(amount),2) as total_purchase_amount
from Transactions t
join Customers c on t.Customer_ID = c.Customer_ID
group by c.country
order by total_purchase_amount desc;
```


# Country-based transactions
```sql
select
    c.country,
    count(t.transaction_id) as total_transactions
from Transactions t
join Customers c on t.Customer_ID = c.Customer_ID
group by c.country;
```

```sql
SELECT
    country,
    total_transactions,
    ROW_NUMBER() OVER (ORDER BY total_transactions DESC) AS transaction_rank
FROM (
    SELECT
        c.country,
        COUNT(t.transaction_id) AS total_transactions
    FROM Transactions t
    JOIN Customers c ON t.Customer_ID = c.Customer_ID
    GROUP BY c.country
) AS transaction_summary;
```

# State-based Purchase 
```sql
select
    c.state,
    count(t.Transaction_ID) as total_transactions,
    round(sum(amount),2) as total_purchase_amount
from Transactions t
join Customers c on t.Customer_ID = c.Customer_ID
group by c.state
order by total_purchase_amount desc;
```

# Income-based grouping
```sql
select income, count(distinct customer_id) as total_customers
from customers
group by income
order by total_customers desc;
```

# Customer- Segment based grouping
```sql
select customer_segment, count(distinct customer_id) as total_customers
from customers
group by customer_segment
order by total_customers desc;
```

# Calculate Total Revenue per Segment
```sql
SELECT
    c.customer_segment,
    round(SUM(t.amount),2) AS total_revenue
FROM customers c
JOIN transactions t ON c.customer_id = t.customer_id
GROUP BY c.customer_segment
order by total_revenue desc;
```

# Average Customer Rating per Segment
```sql
select c.customer_segment, avg(t.ratings) as average_ratings, count(t.feedback) as feedback_count
from customers as c
join transactions as t
on c.customer_id=t.customer_id
group by customer_segment
order by average_ratings,feedback_count desc;
```


```sql
WITH SentimentAnalysis AS (
    SELECT
        customer_segment,
        SUM(CASE WHEN ratings >= 4 THEN 1 ELSE 0 END) AS positive_feedback,
        SUM(CASE WHEN ratings <= 2 THEN 1 ELSE 0 END) AS negative_feedback,
        SUM(CASE WHEN ratings = 3 THEN 1 ELSE 0 END) AS neutral_feedback,
        COUNT(*) AS total_feedback
    FROM customers as c
    join transactions as t
    on c.customer_id=t.customer_id
    GROUP BY customer_segment
)
SELECT
    customer_segment,
    positive_feedback,
    negative_feedback,
    neutral_feedback,
    total_feedback,
    ROUND(100.0 * positive_feedback / total_feedback, 2) AS percentage_positive,
    ROUND(100.0 * negative_feedback / total_feedback, 2) AS percentage_negative,
    ROUND(100.0 * neutral_feedback / total_feedback, 2) AS percentage_neutral
FROM SentimentAnalysis;
```


```sql
WITH SentimentAnalysis AS (
    SELECT
        c.customer_segment,
        SUM(CASE WHEN t.feedback = 'excellent' THEN 1 ELSE 0 END) AS excellent_feedback,
        SUM(CASE WHEN t.feedback = 'good' THEN 1 ELSE 0 END) AS good_feedback,
        SUM(CASE WHEN t.feedback = 'average' THEN 1 ELSE 0 END) AS average_feedback,
        SUM(CASE WHEN t.feedback = 'bad' THEN 1 ELSE 0 END) AS bad_feedback,
        COUNT(*) AS total_feedback
    FROM transactions as t
    JOIN customers as c ON t.customer_id = c.customer_id
    GROUP BY c.customer_segment
)
SELECT
    customer_segment,
    excellent_feedback,
    good_feedback,
    average_feedback,
    bad_feedback,
    total_feedback,
    ROUND(100.0 * excellent_feedback / total_feedback, 2) AS percentage_excellent,
    ROUND(100.0 * good_feedback / total_feedback, 2) AS percentage_good,
    ROUND(100.0 * average_feedback / total_feedback, 2) AS percentage_average,
    ROUND(100.0 * bad_feedback / total_feedback, 2) AS percentage_bad
FROM SentimentAnalysis;
```

# Total sales over time
```sql
select year, month, round(sum(amount),2) as total_sales
from transactions
group by year, month
order by total_sales desc;
```

#Averagte transaction value
```sql
select round(avg(amount),2) as average_amount
from transactions;
```

```sql
select c.gender, round(avg(amount),2) as average_amount
from transactions as t join customers as c on c.customer_id = t.customer_id
group by c.gender;
```

```sql
select c.age_group, round(avg(amount),2) as average_amount
from transactions as t join customers as c on c.customer_id = t.customer_id
group by c.age_group;
```


```sql
select c.age_group, round(sum(amount),2) as total_amount
from transactions as t join customers as c on c.customer_id = t.customer_id
group by c.age_group;
```

# Top selling product
```sql
select p.product_name, round(sum(t.amount),2) as total_sales
from transactions as t 
join product_name as p on t.product_name_id = p.product_name_id
group by p.product_name
order by total_sales desc;
```

# Top selling brand
```sql
select b.product_brand, round(sum(t.amount),2) as total_sales
from transactions as t 
join brand as b on t.brand_id = b.brand_id
group by b.product_brand
order by total_sales desc;
```

# Revenue by product category
```sql
select c.product_category , round(sum(t.amount),2) as total_sales
from transactions as t 
join category as c on t.brand_id = c.category_id
group by c.product_category
order by total_sales desc;
```

# payment method count
```sql
select t.payment_method, count(t.payment_method) as payment_count
from transactions as t
group by t.payment_method;
```


# shipping method count
```sql
select t.shipping_method, count(t.shipping_method) as shipping_method_count
from transactions as t
group by t.shipping_method;
```

# payment and shipping count
```sql
select t.shipping_method, t.payment_method, round(sum(t.amount),2) as total_amount
from transactions as t
group by t.shipping_method, t.payment_method
order by total_amount desc;
```

```sql
SELECT 
    shipping_method,
    payment_method,
    total_amount,
    RANK() OVER (ORDER BY total_amount DESC) AS ranking
FROM (
    SELECT 
        t.shipping_method,
        t.payment_method,
        ROUND(SUM(t.amount), 2) AS total_amount
    FROM 
        transactions AS t
    GROUP BY 
        t.shipping_method, t.payment_method
) AS ranked_data
ORDER BY 
    total_amount DESC;
```