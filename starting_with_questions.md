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


**SQL Queries:** 

**Query for average number of products ordered from visitors in each City**

 select a.city, avg(b.total_ordered) as avgpdtsordered
 from public.all_sessions as a
 join public.sales_report as b on a.productsku = b.productsku
 group by a.city
 having count(b.total_ordered)>1
 order by avgpdtsordered DESC;

**Query for average number of products ordered from visitors in each Country**

select a.country, avg(b.total_ordered) as avgpdtsordered
 from public.all_sessions as a
 join public.sales_report as b on a.productsku = b.productsku
 group by a.country
 having count(b.total_ordered)>1
 order by avgpdtsordered DESC;


**Answer:** The given query for City gives a resultset of 150 records out of which Rexburg, Saint Petersburg and Avon holds the top positions for the average number of products. Also the query for Country gives 100 records in the resultset with Saudi Arabia, Kuwait and Loas in the top positions
for the average number of products

![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/d3eec277-24bb-44e3-926a-22cd4422b076)

![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/289881aa-6bee-40a4-9869-f68ceffe6f79)





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


**SQL Queries:**

WITH CTE_PRDCATEG
AS
(	select s.city, s.country, s.v2productcategory, count(*) as prodcategcount,
 	ROW_NUMBER() OVER (PARTITION BY COUNTRY, CITY ORDER BY count(*) desc) as ROW_NM
	from public.all_sessions as s
	where s.city is not null
	and s.v2productcategory not like '(not%'
	group by country, city, v2productcategory
	having count(*)>1
	order by country, prodcategcount desc	
)
SELECT  country,
		city,
		v2productcategory,
		prodcategcount
FROM CTE_PRDCATEG
WHERE ROW_NM=1
group by v2productcategory,country,city, prodcategcount
ORDER BY country,city,v2productcategory,prodcategcount



**Answer:**
The above query provides a resultset of 117 rows out of which the product category of "Home/Apparel/Men's/Men's-T-Shirts/ " holds the top positions in all the countries. Followed by the product category "Home/Shop by Brand/YouTube/" in the second position. Attached is the .csv file for reference with the resultset




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


**SQL Queries:**

**Query for top selling product for each country**

WITH
total_products_sold AS (
    SELECT 
        country, 
        v2productname, 
        SUM(s.total_ordered) AS sum_total_ordered,
        ROW_NUMBER() OVER (PARTITION BY country ORDER BY SUM(s.total_ordered) DESC) AS rn
    FROM all_sessions AS als
    JOIN sales_report AS s ON s.productsku = als.productsku
    WHERE 
        als.v2productname IS NOT NULL
        AND als.country not like '(not%'
        AND s.total_ordered > 0
    GROUP BY als.country, als.v2productname
)
SELECT 
    country AS "Country",
    v2productname AS " Product Name",
    sum_total_ordered AS "Total Sold Product Quantity"
FROM total_products_sold
WHERE rn = 1
ORDER BY sum_total_ordered DESC
;

**Query for top selling product for each city**
WITH
total_products_sold AS (
    SELECT 
        city, 
        v2productname, 
        SUM(s.total_ordered) AS sum_total_ordered,
        ROW_NUMBER() OVER (PARTITION BY city ORDER BY SUM(s.total_ordered) DESC) AS rn
    FROM all_sessions AS als
    JOIN sales_report AS s ON s.productsku = als.productsku
    WHERE 
        als.v2productname IS NOT NULL
        AND als.city IS NOT NULL
        AND s.total_ordered > 0
    GROUP BY als.city, als.v2productname
)
SELECT 
    city AS "City",
    v2productname AS " Product Name",
    sum_total_ordered AS "Total Sold Product Quantity"
FROM total_products_sold
WHERE rn = 1
ORDER BY sum_total_ordered DESC
;

**Answer:** On analyzing the above query for the top selling products by Country, we see the pattern as "Google 17oz Stainless Steel Sport Bottle" , "YouTube Hard Cover Journal", "Ballpoint LED Light Pen" and "Leatherette Journal" are the most sold products.
Also for the top selling products by City, the pattern is "Nest® Cam Indoor Security Camera - USA" , "Google 17oz Stainless Steel Sport Bottle", "Ballpoint LED Light Pen", "Leatherette Journal" and "YouTube Hard Cover Journal". Overall we could see "Google 17oz Stainless Steel Sport Bottle" , "YouTube Hard Cover Journal", "Ballpoint LED Light Pen" and "Leatherette Journal" are the most sold products for each country and City.

![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/998a7461-d4dc-4bf8-a199-458d0d83df88)


![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/6a3423fc-5fcb-4731-b2b4-13da4f1bfbc5)




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

**SQL Queries:**

select country, city, sum(totaltransactionrevenue) as Total_Transaction_Revenue
from public.all_sessions
where totaltransactionrevenue is not null
and city is not null
group by country, city, totaltransactionrevenue
order by Total_Transaction_Revenue desc

**Answer:** Based on the query resultset, Atlanta and Sunnyvale cities of UnitedStates shows the most revenue generated compared to the other countries such as Israel, Australia and Canada.
This shows that United States is the highest is most revenue generated

![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/41f443d6-4aa0-4af4-930d-98b35dfb5ad8)








