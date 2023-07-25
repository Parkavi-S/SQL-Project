**I have identified the following risk areas to highlight some of them below:**

1. Duplicates inside the analytics table. Had to work on cleaning the table thoroughly
2. Ensured the key columns to be used such as unit_sold from analytics, visitid and fullvisitorid from analytics and all_session tables are not having null values
3. Identification of proper key columns to join analytics and all_session table was a huge challenge
4. the Sales_sku table looked like a subset of sales_report and felt it as redundant
5. Not all the transaction ids and transaction revenues were available, which made the process of writting query for the given questions tedious
6. Absence of proper primary key values in the tables such as all_Session and analytics makes it difficult to arrive at unique values for ERD generation


**QA Process:**

**1. Ensured the key columns did not have any null values in it**

select * from public.all_sessions where (fullvisitorid is null or visitid is null)
![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/64f6db74-53ed-4fe8-ad95-1db16d2a5dc4)


select * from public.sales_report where total_ordered is null
![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/e3e86546-fae0-4707-8be7-9695bb8a8e33)


**2. Validated the dataset consistency across the tables, for data type constraints**

		SELECT totaltransactionrevenue, (cast(totaltransactionrevenue as double precision) / 1000000) TOT_TRANS_REV
		FROM all_sessions
		WHERE totaltransactionrevenue is not null
	
	    SELECT productrevenue, (cast(productrevenue as double precision) / 1000000) PROD_REV
		FROM all_sessions
		WHERE productrevenue is not null
	
	
	    SELECT transactionrevenue, (cast(transactionrevenue as double precision) / 1000000) TRANS_REV
		FROM all_sessions
		WHERE transactionrevenue is not null

  	  	select revenue, units_sold from public.analytics

  	select time, timeonsite from public.all_sessions limit 10
   	![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/eecd521b-8d4f-4a16-b3e5-a44518c0af5c)    	


**3. Identifying outliers and investigating data points that has deviations from expected value range**

	select min(total_ordered) from public.sales_report

	select max(total_ordered) from public.sales_report
