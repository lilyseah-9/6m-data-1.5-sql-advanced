# Assignment

## Brief

Write the SQL statements for the following questions.

## Instructions

Paste the answer as SQL in the answer code section below each question.

### Question 1

Using the `claim` and `car` tables, write a SQL query to return a table containing `id, claim_date, travel_time, claim_amt` from `claim`, and `car_type, car_use` from `car`. Use an appropriate join based on the `car_id`.

Answer:
SELECT 
    cl.id, 
    cl.claim_date, 
    cl.travel_time, 
    cl.claim_amt, 
    ca.car_type, 
    ca.car_use
FROM claim AS cl
JOIN car AS ca 
  ON cl.car_id = ca.id;

### Question 2

Write a SQL query to compute the running total of the `travel_time` column for each `car_id` in the `claim` table. The resulting table should contain `id, car_id, travel_time, running_total`.

Answer:
SELECT 
    id, 
    car_id, 
    travel_time,
    SUM(travel_time) OVER (
        PARTITION BY car_id 
        ORDER BY id
    ) AS running_total
FROM claim;

### Question 3

Using a Common Table Expression (CTE), write a SQL query to return a table containing `id, resale_value, car_use` from `car`, where the car resale value is less than the average resale value for the car use.

Answer:
WITH AverageResale AS (
    SELECT 
        car_use, 
        AVG(resale_value) AS avg_resale
    FROM car
    GROUP BY car_use
)
SELECT 
    c.id, 
    c.resale_value, 
    c.car_use
FROM car AS c
JOIN AverageResale AS ar 
    ON c.car_use = ar.car_use
WHERE c.resale_value < ar.avg_resale;

# **Level Up: The Insurance Auditor Project**

Scenario:  
You are the Lead Data Analyst for "SafeDrive Insurance". The CEO suspects that certain car types in specific cities are disproportionately expensive.  
Your Task:  
Create a comprehensive SQL report that answers the following in a single script (using CTEs):

1. **Market Comparison:** For every claim, show the claim\_amt alongside the **average claim amount for that specific car\_type**.  
2. **Risk Ranking:** Within each state, rank the clients by their total claim amounts.  
3. **Efficiency:** Only show the **top 2** highest-claiming clients per state.  
4. **Final Output:** The table should include: Client Name, State, Car Type, Total Claimed, State Rank.

Submission:  
A single .sql file with comments explaining your logic.
