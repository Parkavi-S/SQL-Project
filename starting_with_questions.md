Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

**SQL Queries:**

WITH CTE_REV AS
(
select 	country,
		city, 
		max(totaltransactionrevenue) max_tot_trans_rev
 from public.all_sessions a
where totaltransactionrevenue is not null
group by a.country, city
)
Select country,
		city,
			max_tot_trans_rev
FROM CTE_REV
where city is not null
order by max_tot_trans_rev desc

**Answer**: The query gives an ouput of 56 combinations of country and their corresponding cities, out of which Atlanta from Unitedstates, Sunnyvale from Unitedstates and Tel Aviv- Yafo from Israel holds the top 3 positions based on the total transaction revenues.
Followed by Los Angeles from UnitedStates and Sydney from Australia taking 4th and 5th positions.

![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/9c83ccae-3ba4-4984-b92b-37a542fdb844)




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







