1.
Create a SQL view named TopRatedRestaurants that selects the restaurant name, average rating, and total number of reviews from a table of Zomato-style restaurant reviews, showing only restaurants with an average rating above 4.0.

***SOLUTION***
CREATE VIEW TopRatedRestaurants AS
SELECT
    r.name AS restaurant_name,
    AVG(rv.rating) AS average_rating,
    COUNT(rv.rating) AS total_reviews
FROM restaurants r
JOIN reviews rv
    ON r.restaurant_id = rv.restaurant_id
GROUP BY r.name
HAVING AVG(rv.rating) > 4.0;

2.
Update the TopRatedRestaurants view to also include the city column from the original restaurants table by joining the relevant tables.<br><br><em><strong>Hint:</strong> Use an INNER JOIN to combine data from both tables in your view definition.</em>

***SOLUTION***
CREATE OR REPLACE VIEW TopRatedRestaurants AS
SELECT
    r.name AS restaurant_name,
    r.city,
    AVG(rv.rating) AS average_rating,
    COUNT(rv.rating) AS total_reviews
FROM restaurants r
INNER JOIN reviews rv
    ON r.restaurant_id = rv.restaurant_id
GROUP BY r.name, r.city
HAVING AVG(rv.rating) > 4.0;

3.
Try to update the average rating column directly through the TopRatedRestaurants view and observe what error or limitation occurs. Write down the exact error message and explain why this happens based on SQL view limitations.

***SOLUTION***
UPDATE TopRatedRestaurants
SET average_rating = 5
WHERE restaurant_name = 'Some Restaurant';

4.
Create a view called DailyOrderSummary that shows, for each date, the total number of food orders and the total revenue from a Swiggy-style orders table. Ensure the view only includes dates from the last 30 days.<br><br><em><strong>Constraint:</strong> Use WHERE and GROUP BY clauses in your view definition.</em>

***SOLUTION***
CREATE VIEW DailyOrderSummary AS
SELECT
    order_date,
    COUNT(order_id) AS total_orders,
    SUM(total_amount) AS total_revenue
FROM orders
WHERE order_date >= CURRENT_DATE - INTERVAL '30 DAY'
GROUP BY order_date;

5.
List 3 good practices you should follow when creating SQL views for analytics dashboards, and for each, give a one-line example related to a Flipkart sales reporting scenario.

***SOLUTION***
1. Keep views simple and focused (avoid unnecessary complexity)
Example:
👉 Create a view FlipkartDailySales only for daily revenue, not mixing returns + inventory + marketing.

2. Use filters early to reduce data volume
Example:
👉 In a view FlipkartTopCategories, filter only last 90 days of sales instead of full historical data.

3. Avoid heavy calculations in real-time views (pre-aggregate when possible)
Example:
👉 Instead of calculating profit in dashboard query, store it in FlipkartProfitSummary view using precomputed margins.



