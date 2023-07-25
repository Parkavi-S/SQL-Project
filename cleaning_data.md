What issues will you address by cleaning the data?

1. Cleaned the timeonsite value in the all_sessions table 
2. Updated the currency code across the all_sessions table
3. Analyzed the dupes from the analytics table
4. Cleaning Revenue columns for the all_sessions table
5. Cleaning value for City to remove junk values from all_sessions table
6. Temp table creation queries


Queries:
Below, provide the SQL queries you used to clean your data.

**/* Cleaned the timeonsite value in the all_sessions table  */ **
select to_char(to_timestamp(time1),'HH:MM:SS') from
public.all_sessions

ALTER TABLE public.all_sessions
ADD COlUMN TIME VARCHAR(20)

update public.all_sessions a
set TIME = b.CRT_TIME
FROM ( select time1, to_char(to_timestamp(time1),'HH:MM:SS') as CRT_TIME from
public.all_sessions) b
WHERE a.time1 = b.time1

   **--- Updated the currency code across the all_sessions table --- 
**
select * from public.all_sessions
where currencycode is null

select distinct currencycode from public.all_sessions

update public.all_sessions
set currencycode='USD'
where currencycode is null

**--- Data cleaning analytics table ---**

select * from public.analytics
limit 100

select visitnumber,visitid,visitstarttime,date,fullvisitorid,unit_price, count(1)
from public.analytics
group by visitnumber,visitid,visitstarttime,date,fullvisitorid,unit_price
having count(1)>1

ALTER TABLE public.analytics_old
RENAME TO analytics_withdupes

ALTER TABLE public.all_sessions
RENAME TO tmp_all_sessions

select * from public.tmp_all_sessions limit 100

select * from all_sessions limit 100

select fullvisitorid, channelgrouping, time, country, city, total_transaction_revenue as totaltransactionrevenue, transactions,
		timeonsite1 as timeonsite,pageviews,sessionqualitydim,date,visitid,type,productquantity, 
		(cast(productprice as double precision) / 1000000) as productprice,product_revenue as productrevenue,
		productsku,v2productname,v2productcategory, productvariant,currencycode,transaction_revenue as transactionrevenue,
		transactionid,pagetitle,pagepathlevel1, ecommerceaction_type,ecommerceaction_step,ecommerceaction_option
		into all_sessions
from tmp_all_sessions

from public.sales_report
group by productsku
having count(1)>1

**--- Cleaning the revenue columns from all_sessions ---**

select * from public.all_sessions limit 10

select * from public.all_sessions where totaltransactionrevenue is not null - /* 81 rows */

select * from public.all_sessions where productrevenue is not null - /* 4 rows */

select * from public.all_sessions where transactionrevenue is not null - /* 4 rows */

select * from public.all_sessions limit 100

select * from public.all_sessions where transaction_revenue is not null

**--- Cleaning City values from all_session ---**

select fullvisitorid, channelgrouping, time, country, 
		CASE WHEN (city='not available in demo dataset' OR city='(not set)')
		THEN NULL
		ELSE city
		END City, 
		total_transaction_revenue as totaltransactionrevenue, transactions,
		timeonsite1 as timeonsite,pageviews,sessionqualitydim,date,visitid,type,productquantity, 
		(cast(productprice as double precision) / 1000000) as productprice,product_revenue as productrevenue,
		productsku,v2productname,v2productcategory, productvariant,currencycode,transaction_revenue as transactionrevenue,
		transactionid,pagetitle,pagepathlevel1, ecommerceaction_type,ecommerceaction_step,ecommerceaction_option
		into all_sessions
		
from tmp_all_sessions

**----  Analytics Temp table and Main table creation ----**

select * from public.tmp_analytics limit 10

select count(*) from public.tmp_analytics limit 10

select * from public.tmp_analytics where bounces is not null

ALTER TABLE public.analytics_old
RENAME TO analytics_withdupes

 
select visitnumber,visitid,visitstarttime,date,fullvisitorid,channelgrouping,socialengagementtype,units_sold,pageviews,
		timeonsite, bounces,revenue,unit_price into analytics
from tmp_analytics

**---  All_Sessions Temp table and Main table creation ---**

ALTER TABLE public.all_sessions
RENAME TO tmp_all_sessions

select * from public.tmp_all_sessions limit 100

select * from all_sessions limit 100

select fullvisitorid, channelgrouping, time, country, city, total_transaction_revenue as totaltransactionrevenue, transactions,
		timeonsite1 as timeonsite,pageviews,sessionqualitydim,date,visitid,type,productquantity, 
		(cast(productprice as double precision) / 1000000) as productprice,product_revenue as productrevenue,
		productsku,v2productname,v2productcategory, productvariant,currencycode,transaction_revenue as transactionrevenue,
		transactionid,pagetitle,pagepathlevel1, ecommerceaction_type,ecommerceaction_step,ecommerceaction_option
		into all_sessions
from tmp_all_sessions


select * from public.products limit 100

alter table public.products
rename to tmp_products

select * 
from  tmp_products

drop table public.sales_by_sku_try

select * from sales_by_sku

select * from tmp_sales_by_sku

alter table public.tmp_sales_by_sku
rename to sales_by_sku

select * 
from tmp_sales_by_sku

drop table tmp_sales_by_sku

drop table public.tmp_products

select * from public.tmp_analytics

select * from public.analytics

drop table public.analytics
