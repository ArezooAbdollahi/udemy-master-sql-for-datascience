# Q1 Write a query that displays 3 columns. The query should display the fruit and it's total supply along with a category of either LOW, ENOUGH or FULL. Low category means that the total supply of the fruit is less than 20,000.  The enough category means that the total supply is between 20,000 and 50,000. If the total supply is greater than 50,000 then that fruit falls in the full category. 

SELECT name, total_supply,
CASE WHEN total_supply < 20000 THEN 'LOW'
           WHEN total_supply >= 20000 AND total_supply <= 50000 THEN 'ENOUGH'
           WHEN total_supply > 50000 THEN 'FULL'
END as category
FROM (
SELECT name, sum(supply) total_supply
FROM fruit_imports
GROUP BY name
    ) a

# Q2 Taking into consideration the supply column and the cost_per_unit column, you should be able to tabulate the total cost to import fruits by each season. The result will look something like this:
# "Winter" "10072.50"
# "Summer" "19623.00"
# "All Year" "22688.00"
# "Spring" "29930.00"
# "Fall" "29035.00"

* Write a query that would transpose this data so that the seasons become columns and the total cost for each season fills the first row?

SELECT SUM(CASE WHEN season = 'Winter' THEN supply * cost_per_unit END) as "Winter",
             SUM(CASE WHEN season = 'Summer' THEN supply * cost_per_unit END) as "Summer",
             SUM(CASE WHEN season = 'All Year' THEN supply * cost_per_unit END) as "All Year",
             SUM(CASE WHEN season = 'Spring' THEN supply * cost_per_unit END) as "Spring",
             SUM(CASE WHEN season = 'Fall' THEN supply * cost_per_unit END) as "Fall"
FROM fruit_imports
