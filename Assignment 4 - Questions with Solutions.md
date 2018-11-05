# Q1 Write a query that displays only the state with the largest amount of fruit supply.

SELECT state
FROM fruit_imports
GROUP BY state
ORDER BY SUM(supply) desc
LIMIT 1

# Q2 Write a query that returns the most expensive cost_per_unit of every season. The query should display 2 columns, the season and the cost_per_unit

SELECT season, MAX(cost_per_unit) max_cost_per_unit_for_season
FROM fruit_imports
GROUP BY season

# Q3 Write a query that returns the state that has more than 1 import of the same fruit. 

SELECT state
FROM fruit_imports
GROUP BY state, name
HAVING COUNT(name) > 1

# Q4 Write a query that returns the seasons that produce either 3 fruits or 4 fruits.

SELECT season
FROM fruit_imports
GROUP BY season
HAVING COUNT(name) = 3 OR COUNT(name) = 4;

# Q5 Write a query that takes into consideration the  supply and cost_per_unit columns for determining the total cost and returns the most expensive state with the total cost.

SELECT state, ROUND(SUM(supply * cost_per_unit)) total_cost 
FROM fruit_imports
GROUP BY state
ORDER BY total_cost DESC
LIMIT 1;

# Q6 Write a query that returns the count of 4. You'll need to count on the column fruit_name and not use COUNT(*)

SELECT COUNT(COALESCE(fruit_name, 'SOMEVALUE'))
FROM fruits;