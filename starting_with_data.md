**Question 1:** What is the highest of product viewed in United States and what is the sales for the same from the sales report vs ordered quantity

**SQL Queries:** Highest product viewed in United States

select a.productsku, count(*) product_count, p.name, p.orderedquantity, b.productsku, b.total_ordered
from public.all_sessions a 
inner join public.products p on a.productsku =p.sku
inner join public.sales_report b on a.productsku =b.productsku
where a. country='United States' and a.productsku='GGOEGAAX0104'
group by a.productsku,p.name ,p.orderedquantity, b.productsku, b.total_ordered
order by product_count desc

**Answer:** The above query shows that the product that has highest views and orders is " Men's 100% Cotton Short Sleeve Hero Tee White" which belongs to the "Men's T-Shirts" product Category, which resonates the results of starting with questions

![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/421697cd-4c6b-4e75-b034-e2b413977808)



**Question 2:** What are the different split for the above product by channel grouping and other observations related to the product

**SQL Queries:**

-- Query for different channel grouping for the given product --
select channelgrouping, count(*) 
from public.all_sessions where productsku='GGOEGAAX0104' and country='United States'
group by channelgrouping

-- Different prices for the sameproduct --
select productsku,channelgrouping,v2productname, productprice
from public.all_sessions where productsku='GGOEGAAX0104'  and country='United States'
LIMIT 10;

**Answer:** On research product sku - GGOEGAAX0104 - which is -  Men's 100% Cotton Short Sleeve Hero Tee White that is being most searched on the web page from United states.
Out of these searches- most of the page hits are related organic search channel grouping. Also the product price does vary within the same product, based on the 
type how the product is searched. If the product is searched through organic search, the price is 16.99 USD, but if it is searched via referral it is 13.59 USD

![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/5ff5bf13-9539-4623-9313-bf700145d2f6)



**Question 3:** Which is the product that has the most total sales across the world

**SQL Queries:**
  select a.productsku, count(*) product_count, p.name, p.orderedquantity, b.productsku, b.total_ordered
from public.all_sessions a 
inner join public.products p on a.productsku =p.sku
inner join public.sales_report b on a.productsku =b.productsku
where a.productsku='GGOEGOAQ012899'
group by a.productsku,p.name ,p.orderedquantity, b.productsku, b.total_ordered
order by product_count desc

**Answer:** The above query shows that the product "Ballpoint LED Light Pen" is the most sold product across the world.

![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/163baa6f-7ce0-4f08-bbf9-5439401114be)


